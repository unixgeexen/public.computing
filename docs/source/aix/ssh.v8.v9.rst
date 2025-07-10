.. _aix-ssh-upgrade-audit:

SSH V8.1 to V9.7 Upgrade Impact Analysis for AIX Systems
========================================================

Summary
-------
This document details the ten most significant changes when upgrading OpenSSH from V8.1 to V9.7 on AIX, including security enhancements, deprecated features, and functionality changes. The audit script has been updated to output directly to stdout with each line prefixed by the short hostname and timestamp (yyyymmdd.hhmmss), clearly indicating potential issues with [WARNING] and [INFO] labels.

Significant Changes and AIX-Compatible Audit Procedures
------------------------------------------------------

1. **DSA Key Support Removal**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: DSA (`ssh-dss`) keys disabled by default (weak cryptography)
- **AIX Audit Commands**:
  - Host keys: ``grep -i ssh_host_dsa /etc/ssh/sshd_config``
  - User keys: ``find / -xdev -name 'id_dsa*' -print 2>/dev/null``

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
   :caption: AIX SSH Upgrade Compatibility Auditor (ssh_audit.ksh)
   
   #!/usr/bin/ksh
   # AIX SSH V8.1 to V9.7 Upgrade Compatibility Auditor
   # Outputs directly to stdout with hostname and timestamp prefix
   # Each line shows status with [INFO] or [WARNING]
   
   HOST=$(hostname -s)
   TIMESTAMP=$(date +%Y%m%d.%H%M%S)
   PREFIX="$HOST $TIMESTAMP"
   
   print_header() {
     echo "$PREFIX: [INFO] ===== $1 ====="
   }
   
   print_warning() {
     echo "$PREFIX: [WARNING] $1"
   }
   
   print_info() {
     echo "$PREFIX: [INFO] $1"
   }
   
   print_header "AIX SSH Upgrade Compatibility Audit"
   print_info "Timestamp: $(date)"
   print_info "OS Version: $(oslevel -s)"
   echo ""
   
   # 1. DSA Key Check
   print_header "1/6: DSA KEY AUDIT"
   # Check host keys
   grep -i ssh_host_dsa /etc/ssh/sshd_config | while read -r line; do
     print_warning "DSA host key configured: $line"
   done
   
   # Find user keys
   find / -xdev -name 'id_dsa*' -print 2>/dev/null | while read -r key; do
     print_warning "DSA user key found: $key"
   done
   
   # Active connections
   print_info "Active SSH connections:"
   netstat -an | grep '.22' | awk '{print $1}' | sort | uniq -c | while read -r conn; do
     print_info "Connection: $conn"
   done
   echo ""
   
   # 2. KEX Algorithm Check
   print_header "2/6: KEX ALGORITHM AUDIT"
   # Config overrides
   grep -i KexAlgorithms /etc/ssh/sshd_config | grep diffie-hellman | while read -r line; do
     print_warning "Legacy KEX configured: $line"
   done
   
   # Log analysis
   print_info "Recent DH connections:"
   grep -i 'kex:.*diffie-hellman' /var/adm/syslog 2>/dev/null | tail -5 | while read -r entry; do
     print_info "Log entry: $entry"
   done
   echo ""
   
   # 3. ControlMaster Usage
   print_header "3/6: CONTROLMASTER AUDIT"
   # System configurations
   grep -r ControlMaster=yes /etc/ssh/ 2>/dev/null | while read -r config; do
     print_warning "ControlMaster enabled: $config"
   done
   
   # User configurations
   for user in $(lsuser -a home ALL | awk '$2 != "/" {print $1}'); do
     [ -f $(eval echo ~$user)/.ssh/config ] && \
     grep -l ControlMaster=yes $(eval echo ~$user)/.ssh/config 2>/dev/null | while read -r file; do
       print_warning "User ControlMaster config: $user: $file"
     done
   done
   
   # Active sessions
   print_info "Active multiplexed sessions:"
   ps -ef | grep -E 'ssh.*-M' | while read -r session; do
     print_info "Session: $session"
   done
   echo ""
   
   # 4. Private Key Format Check
   print_header "4/6: PRIVATE KEY FORMAT AUDIT"
   find /etc/ssh /home -name '*.pem' -exec awk '
     END {if (NR>0 && $0 !~ /\n$/) print FILENAME}
   ' {} \; 2>/dev/null | while read -r keyfile; do
     print_warning "Missing trailing newline: $keyfile"
   done
   echo ""
   
   # 5. Host Key Documentation
   print_header "5/6: HOST KEY DOCUMENTATION"
   print_info "Current host key fingerprints:"
   for key in /etc/ssh/ssh_host_*_key; do
     [ -f "$key" ] && ssh-keygen -l -f "$key" | while read -r fp; do
       print_info "Fingerprint: $fp"
     done
   done
   echo ""
   
   # 6. System Compatibility
   print_header "6/6: SYSTEM COMPATIBILITY CHECKS"
   print_info "USB Security Devices:"
   lsdev -Cc usb | grep -i security | while read -r device; do
     print_info "Device: $device"
   done
   
   print_info "SSH Package Version:"
   lslpp -L | grep openssh.base | while read -r pkg; do
     print_info "Package: $pkg"
   done
   echo ""
   
   print_header "AUDIT COMPLETE"

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
:Audit Script Version: 2.0
:Output Format: Stdout with prefix HOSTNAME YYYYMMDD.HHMMSS
:Status Indicators: 
  - [INFO]: Normal operational message
  - [WARNING]: Potential upgrade issue
:Critical Checks:
  - DSA key usage
  - Diffie-Hellman KEX algorithms
  - Private key formatting
:Tags: AIX, SSH-Upgrade, Audit-Script, Stdout-Logging, Prefix-Format, Security

Context
-------
- **Request Date**: 2025-07-10
- **DeepSeek Parameters**:
  - Model: DeepSeek-R1
  - Temperature: 0.7
  - Max Tokens: 4096
- **Original Request**: 
  "Update script to output to stdout with hostname and timestamp prefix per line"
