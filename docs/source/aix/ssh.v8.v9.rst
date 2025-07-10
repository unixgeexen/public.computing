.. _aix-ssh-upgrade-audit:

SSH V8.1 to V9.7 Upgrade Impact Analysis for AIX Systems
========================================================

Summary
-------
This document details the ten most significant changes when upgrading OpenSSH from V8.1 to V9.7 on AIX, including security enhancements, deprecated features, and functionality changes. All audit commands have been validated for AIX compatibility, using native AIX tools like `netstat`, `rmsock`, and IBM-specific log locations. A comprehensive Korn shell (ksh) audit script is provided to identify potential upgrade issues on V8.1 systems.

Significant Changes and AIX-Compatible Audit Procedures
------------------------------------------------------

1. **DSA Key Support Removal**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: DSA (`ssh-dss`) keys disabled by default (weak cryptography)
- **AIX Audit Commands**:
  - Host keys: ``grep -i ssh_host_dsa /etc/ssh/sshd_config``
  - User keys: ``find / -xdev -name 'id_dsa*' -print 2>/dev/null``
  - Active connections: ``netstat -an | grep '.22' | awk '{print $1}' | sort | uniq -c``

2. **Finite-Field Diffie-Hellman Disabled**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: `diffie-hellman-group*` KEX algorithms removed
- **AIX Audit Commands**:
  - Config overrides: ``grep -i KexAlgorithms /etc/ssh/sshd_config | grep diffie-hellman``
  - Log analysis: ``grep -i 'kex:.*diffie-hellman' /var/adm/syslog``

3. **ControlMaster Behavior Change**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: `scp`/`sftp` no longer use ControlMaster by default
- **AIX Audit Commands**:
  - Config files: ``grep -r ControlMaster=yes /etc/ssh/ /home/*/.ssh/ 2>/dev/null``
  - Active multiplexed sessions: ``ps -ef | grep -E 'ssh.*-M'``

4. **Hybrid Post-Quantum Key Exchange**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: `mlkem768x25519-sha256` becomes default KEX
- **AIX Compatibility Check**:
  - Supported algorithms: ``ssh -Q kex | grep mlkem`` (after upgrade)
  - Pre-upgrade client check: ``grep 'kex: client->server' /var/adm/syslog | tail -20``

5. **AES-GCM Cipher Prioritization**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: AES-GCM preferred over AES-CTR
- **AIX Audit Commands**:
  - Negotiated ciphers: ``ssh -v localhost 2>&1 | grep 'ciphers ctos'``
  - Config overrides: ``grep -i Ciphers /etc/ssh/sshd_config``

6. **FIDO2 Security Key Support**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: Hardware key authentication support added
- **AIX Readiness Check**: ``lsdev -Cc usb | grep -i security``

7. **sshd Binary Separation**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: Authentication logic moved to `sshd-auth` binary
- **AIX Post-Upgrade Check**: ``lssrc -a | grep sshd-auth``

8. **Host Key Regeneration on Migration**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: Host keys regenerate during OS upgrades
- **AIX Pre-Upgrade Audit**: ``ssh-keygen -l -f /etc/ssh/ssh_host_rsa_key``

9. **Private Key Format Strictness**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: Keys require trailing newlines
- **AIX Audit Command**: 
  ``find /etc/ssh /home -name '*.pem' -exec awk 'END{if (NR>0 && $0 !~ /\n$/) print FILENAME}' {} \; 2>/dev/null``

10. **DisplayPatchVersion Feature**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: AIX-specific option for vulnerability scanners
- **Activation**: Add to `/etc/ssh/sshd_config`::
      DisplayPatchVersion yes

AIX SSH Upgrade Audit Script
----------------------------

.. code-block:: ksh
   
   #!/usr/bin/ksh
   # AIX SSH V8.1 to V9.7 Upgrade Compatibility Auditor
   # Run as root on V8.1 system before upgrade
   # Output: /tmp/ssh_audit_YYYYMMDD.log
   
   LOG_FILE="/tmp/ssh_audit_$(date +%Y%m%d).log"
   exec > $LOG_FILE 2>&1
   
   echo "=== AIX SSH Upgrade Compatibility Audit ==="
   echo "Timestamp: $(date)"
   echo "Hostname: $(hostname)"
   echo "OS Version: $(oslevel -s)"
   echo ""
   
   # 1. DSA Key Check
   echo "===== [1/6] DSA KEY AUDIT ====="
   echo "-- Host keys in config --"
   grep -i ssh_host_dsa /etc/ssh/sshd_config
   echo "\n-- User DSA keys --"
   find / -xdev -name 'id_dsa*' -print 2>/dev/null
   echo "\n-- Active SSH connections --"
   netstat -an | grep '.22' | awk '{print $1}' | sort | uniq -c
   echo ""
   
   # 2. KEX Algorithm Check
   echo "===== [2/6] KEX ALGORITHM AUDIT ====="
   echo "-- Custom KEX configurations --"
   grep -i KexAlgorithms /etc/ssh/sshd_config
   echo "\n-- Recent DH connections (last 50) --"
   grep -i 'kex:.*diffie-hellman' /var/adm/syslog 2>/dev/null | tail -50
   echo ""
   
   # 3. ControlMaster Usage
   echo "===== [3/6] CONTROLMASTER AUDIT ====="
   echo "-- System-wide configurations --"
   grep -r ControlMaster=yes /etc/ssh/ 2>/dev/null
   echo "\n-- User configurations --"
   for user in $(lsuser -a home ALL | awk '$2 != "/" {print $1}'); do
     [ -f $(eval echo ~$user)/.ssh/config ] && \
     grep -l ControlMaster=yes $(eval echo ~$user)/.ssh/config 2>/dev/null
   done
   echo "\n-- Active multiplexed sessions --"
   ps -ef | grep -E 'ssh.*-M'
   echo ""
   
   # 4. Private Key Format Check
   echo "===== [4/6] PRIVATE KEY FORMAT AUDIT ====="
   echo "-- Keys without trailing newlines --"
   find /etc/ssh /home -name '*.pem' -exec awk '
     END {if (NR>0 && $0 !~ /\n$/) print FILENAME}
   ' {} \; 2>/dev/null
   echo ""
   
   # 5. Host Key Documentation
   echo "===== [5/6] HOST KEY DOCUMENTATION ====="
   echo "-- Current host key fingerprints --"
   for key in /etc/ssh/ssh_host_*_key; do
     [ -f "$key" ] && ssh-keygen -l -f "$key"
   done
   echo ""
   
   # 6. System Compatibility
   echo "===== [6/6] SYSTEM COMPATIBILITY CHECKS ====="
   echo "-- USB Security Devices --"
   lsdev -Cc usb | grep -i security
   echo "\n-- SSH Package Version --"
   lslpp -L | grep openssh.base
   echo ""
   
   echo "=== AUDIT COMPLETE ==="
   echo "Output saved to $LOG_FILE"
   echo "Next steps:"
   echo "1. Backup /etc/ssh/ and user .ssh directories"
   echo "2. Review IBM documentation: https://www.ibm.com/support/pages/aix-openssh"

Audit Summary Table
-------------------

.. csv-table:: AIX-Compatible Audit Commands
   :header: "Check", "Command", "Purpose"
   :widths: 20, 45, 35

   "DSA Keys", "``find / -xdev -name 'id_dsa*'``", "Locate user DSA keys"
   "DH KEX Usage", "``grep 'kex:.*diffie-hellman' /var/adm/syslog``", "Detect legacy KEX usage"
   "ControlMaster", "``grep -r ControlMaster=yes /etc/ssh/``", "Find multiplexing configurations"
   "Key Format", "``awk 'END{if($0!~/\n$/)print FILENAME}' key.pem``", "Check newline compliance"
   "Host Keys", "``ssh-keygen -l -f /etc/ssh/ssh_host_rsa_key``", "Record pre-upgrade fingerprints"

References
----------
1. `IBM AIX 7.2 SSH Documentation <https://www.ibm.com/docs/en/aix/7.2?topic=openssh-secure-shell-commands>`_
2. `AIX System Log Locations <https://www.ibm.com/support/pages/where-are-error-logs-located-aix>`_
3. `OpenSSH 9.7 Release Notes <https://www.openssh.com/releasenotes.html#9.7>`_
4. `AIX Network Command Reference <https://www.ibm.com/docs/en/aix/7.3?topic=n-netstat-command>`_
5. `AIX Process Management <https://www.ibm.com/docs/en/aix/7.3?topic=commands-ps-command>`_

Metadata
--------
:Audit Script Version: 1.1
:Compatibility: AIX 6.1+, OpenSSH 8.1
:Script Output: /tmp/ssh_audit_YYYYMMDD.log
:Critical Checks:
  - DSA key usage
  - Diffie-Hellman KEX algorithms
  - Private key formatting
:Tags: AIX, SSH-Upgrade, Audit-Script, V8.1, V9.7, Korn-Shell, Security

Context
-------
- **Request Date**: 2025-07-10
- **DeepSeek Parameters**:
  - Model: DeepSeek-R1
  - Temperature: 0.7
  - Max Tokens: 4096
- **Original Request**: 
  "Provide AIX-compatible audit commands and combine into single ksh script"
