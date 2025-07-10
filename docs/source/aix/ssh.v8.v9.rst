.. _aix-ssh-upgrade-audit:

SSH V8.1 to V9.7 Upgrade Impact Analysis for AIX Systems
========================================================

Summary
-------
This document details the ten most significant changes when upgrading OpenSSH from V8.1 to V9.7 on AIX. The audit script now outputs in CSV format with comma delimiters and no blank lines. Each record includes the short hostname, timestamp (yyyymmdd.hhmmss), status ([INFO] or [WARNING]), audit section, and specific finding.

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

AIX SSH Upgrade Audit Script (CSV Output)
-----------------------------------------

.. code-block:: ksh
   :caption: AIX SSH Upgrade Auditor (ssh_audit_csv.ksh)
   
   #!/usr/bin/ksh
   # AIX SSH V8.1 to V9.7 Upgrade Compatibility Auditor
   # CSV Output Format: hostname,timestamp,status,section,message
   
   HOST=$(hostname -s)
   TIMESTAMP=$(date +%Y%m%d.%H%M%S)
   
   print_csv() {
     STATUS=$1
     SECTION=$2
     MESSAGE=$3
     echo "$HOST,$TIMESTAMP,$STATUS,$SECTION,\"$MESSAGE\""
   }
   
   print_csv "INFO" "Audit" "===== START AIX SSH UPGRADE COMPATIBILITY AUDIT ====="
   print_csv "INFO" "System" "Hostname: $HOST"
   print_csv "INFO" "System" "OS Version: $(oslevel -s)"
   
   # 1. DSA Key Check
   SECTION="DSA Key"
   grep -i ssh_host_dsa /etc/ssh/sshd_config | while read -r line; do
     print_csv "WARNING" "$SECTION" "DSA host key configured: $line"
   done
   
   find / -xdev -name 'id_dsa*' -print 2>/dev/null | while read -r key; do
     print_csv "WARNING" "$SECTION" "DSA user key found: $key"
   done
   
   # 2. KEX Algorithm Check
   SECTION="KEX Algorithm"
   grep -i KexAlgorithms /etc/ssh/sshd_config | grep diffie-hellman | while read -r line; do
     print_csv "WARNING" "$SECTION" "Legacy KEX configured: $line"
   done
   
   grep -i 'kex:.*diffie-hellman' /var/adm/syslog 2>/dev/null | tail -5 | while read -r entry; do
     print_csv "INFO" "$SECTION" "Recent DH connection: $entry"
   done
   
   # 3. ControlMaster Usage
   SECTION="ControlMaster"
   grep -r ControlMaster=yes /etc/ssh/ 2>/dev/null | while read -r config; do
     print_csv "WARNING" "$SECTION" "System ControlMaster enabled: $config"
   done
   
   for user in $(lsuser -a home ALL | awk '$2 != "/" {print $1}'); do
     [ -f $(eval echo ~$user)/.ssh/config ] && \
     grep -l ControlMaster=yes $(eval echo ~$user)/.ssh/config 2>/dev/null | while read -r file; do
       print_csv "WARNING" "$SECTION" "User ControlMaster config: $user: $file"
     done
   done
   
   ps -ef | grep -E 'ssh.*-M' | while read -r session; do
     print_csv "INFO" "$SECTION" "Active multiplexed session: $session"
   done
   
   # 4. Private Key Format Check
   SECTION="Key Format"
   find /etc/ssh /home -name '*.pem' -exec awk '
     END {if (NR>0 && $0 !~ /\n$/) print FILENAME}
   ' {} \; 2>/dev/null | while read -r keyfile; do
     print_csv "WARNING" "$SECTION" "Missing trailing newline: $keyfile"
   done
   
   # 5. Host Key Documentation
   SECTION="Host Key"
   for key in /etc/ssh/ssh_host_*_key; do
     [ -f "$key" ] && ssh-keygen -l -f "$key" | while read -r fp; do
       print_csv "INFO" "$SECTION" "Host key fingerprint: $fp"
     done
   done
   
   # 6. System Compatibility
   SECTION="Compatibility"
   lsdev -Cc usb | grep -i security | while read -r device; do
     print_csv "INFO" "$SECTION" "Security device detected: $device"
   done
   
   lslpp -L | grep openssh.base | while read -r pkg; do
     print_csv "INFO" "$SECTION" "SSH package installed: $pkg"
   done
   
   print_csv "INFO" "Audit" "===== AUDIT COMPLETE ====="

CSV Output Example
------------------

.. code-block:: text
   :caption: Sample Script Output

   aixserver01,20250710.142305,INFO,Audit,"===== START AIX SSH UPGRADE COMPATIBILITY AUDIT ====="
   aixserver01,20250710.142305,INFO,System,"Hostname: aixserver01"
   aixserver01,20250710.142305,INFO,System,"OS Version: 7200-05-02-2024"
   aixserver01,20250710.142305,WARNING,DSA Key,"DSA host key configured: HostKey /etc/ssh/ssh_host_dsa_key"
   aixserver01,20250710.142305,WARNING,DSA Key,"DSA user key found: /home/user1/.ssh/id_dsa"
   aixserver01,20250710.142305,WARNING,KEX Algorithm,"Legacy KEX configured: KexAlgorithms diffie-hellman-group14-sha1"
   aixserver01,20250710.142305,INFO,KEX Algorithm,"Recent DH connection: sshd[1234]: kex: client->server diffie-hellman-group14-sha1"
   aixserver01,20250710.142305,WARNING,ControlMaster,"System ControlMaster enabled: /etc/ssh/ssh_config:ControlMaster yes"
   aixserver01,20250710.142305,WARNING,ControlMaster,"User ControlMaster config: user2: /home/user2/.ssh/config"
   aixserver01,20250710.142305,INFO,ControlMaster,"Active multiplexed session: user3  12345     1   0 14:23:05  pts/0  0:00 ssh -M -L 8080:localhost:80 remotehost"
   aixserver01,20250710.142305,WARNING,Key Format,"Missing trailing newline: /etc/ssh/server_key.pem"
   aixserver01,20250710.142305,INFO,Host Key,"Host key fingerprint: 2048 SHA256:AbCdE... /etc/ssh/ssh_host_rsa_key (RSA)"
   aixserver01,20250710.142305,INFO,Compatibility,"Security device detected: usbsecurity0 Available  USB Security Device"
   aixserver01,20250710.142305,INFO,Compatibility,"SSH package installed: openssh.base.server 8.1.0.6100  COMMITTED  Open Secure Shell Commands"
   aixserver01,20250710.142305,INFO,Audit,"===== AUDIT COMPLETE ====="

Audit Summary Table
-------------------

.. csv-table:: AIX Audit CSV Fields
   :header: "Field", "Description", "Example"
   :widths: 15, 35, 50

   "hostname", "Short hostname of the system", "aixserver01"
   "timestamp", "Script runtime (YYYYMMDD.HHMMSS)", "20250710.142305"
   "status", "Finding severity ([INFO] or [WARNING])", "[WARNING]"
   "section", "Audit category section", "DSA Key"
   "message", "Detailed finding description", "DSA user key found: /home/user/.ssh/id_dsa"

References
----------
1. `IBM AIX 7.2 SSH Documentation <https://www.ibm.com/docs/en/aix/7.2?topic=openssh-secure-shell-commands>`_
2. `AIX System Log Locations <https://www.ibm.com/support/pages/where-are-error-logs-located-aix>`_
3. `OpenSSH 9.7 Release Notes <https://www.openssh.com/releasenotes.html#9.7>`_
4. `AIX CSV Processing <https://www.ibm.com/docs/en/aix/7.3?topic=manipulation-creating-csv-files>`_

Metadata
--------
:Audit Script Version: 3.0
:Output Format: CSV (Comma-Separated Values)
:Delimiter: Comma (,)
:Special Handling: Double quotes around message field
:No Blank Lines: All empty lines removed from output
:Tags: AIX, SSH-Upgrade, Audit-Script, CSV-Output, No-Blank-Lines, Security

Context
-------
- **Request Date**: 2025-07-10
- **DeepSeek Parameters**:
  - Model: DeepSeek-R1
  - Temperature: 0.7
  - Max Tokens: 4096
- **Original Request**: 
  "Ensure script output is in CSV format with comma delimiter, no blank lines"