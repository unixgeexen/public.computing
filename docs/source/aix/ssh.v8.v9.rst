.. _ssh-upgrade-changes:

Significant Changes from SSH V8.1 to V9.7 on AIX: Impact Analysis and Audit Guide
==================================================================================

Summary
-------
This document outlines the ten most significant changes when upgrading OpenSSH from V8.1 to V9.7 on AIX systems, focusing on deprecated features, security enhancements, and functionality modifications. For each change, commands to audit existing usage on V8.1 systems are provided, along with Splunk search queries where applicable. Key changes include cryptographic algorithm deprecations (DSA, finite-field DH), control master behavior modifications, FIDO2 authentication support, and AIX-specific version reporting features. Audit commands leverage AIX-native tools (`lsof`, `sshd -T`, log analysis) to identify dependencies on deprecated functionality.

.. contents:: **Table of Contents**
   :depth: 3

Significant Changes and Audit Procedures
---------------------------------------

1. **Removal of DSA Key Support**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: DSA (`ssh-dss`) host/user keys disabled by default due to weak cryptography
- **Audit Commands**:
  - Check host keys: ``grep -i ssh_host_dsa /etc/ssh/sshd_config``
  - Find user keys: ``find / -name 'id_dsa*' 2>/dev/null``
  - Active connections: ``lsof -i | grep ssh | awk '{print $8}' | sort | uniq -c | grep dsa``
- **Splunk Query**: ``index=auth source="/var/log/auth.log" ("ssh_dss" OR "dsa-sha1")``

2. **Finite-Field Diffie-Hellman Disabled**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: `diffie-hellman-group*` KEX algorithms disabled in default `sshd_config`
- **Audit Commands**:
  - Client connections: ``grep -i "diffie-hellman-group" /var/log/auth.log``
  - Config overrides: ``grep -i "KexAlgorithms.*diffie-hellman" /etc/ssh/sshd_config``
- **Splunk Query**: ``index=network source="/var/log/sshd.log" "kex_dh_group*"``

3. **ControlMaster Implicit Session Disablement**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: `scp`/`sftp` no longer implicitly create ControlMaster sessions (`ControlMaster=no` enforced)
- **Audit Commands**:
  - Detect ControlMaster usage: ``grep -ir "ControlMaster=yes" /etc/ssh/ ~/.ssh/``
  - Session impact: ``ps -ef | grep -E 'scp|sftp' | grep -c controlpath``
- **Workaround**: Explicit multiplexing configurations required

4. **Hybrid Post-Quantum Key Exchange**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: `mlkem768x25519-sha256` becomes default KEX for quantum resistance
- **Audit Compatibility**:
  - Client support: ``ssh -Q kex | grep mlkem``
  - Log mismatches: ``grep -i "no matching key exchange" /var/log/auth.log``

5. **AES-GCM Cipher Preference**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: AES-GCM prioritized over AES-CTR for performance/security
- **Audit Commands**:
  - Negotiated ciphers: ``ssh -v user@localhost 2>&1 | grep "ciphers ctos"``
  - Config overrides: ``grep -i "Ciphers" /etc/ssh/sshd_config``

6. **FIDO2 Security Key Support**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: Native support for FIDO2 hardware keys (`ssh-keygen -t ed25519-sk`)
- **Audit Readiness**: Check token compatibility: ``ssh-keygen -K`` (requires FIDO middleware)

7. **sshd Binary Splitting**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: Authentication logic moved to `sshd-auth` binary for ASLR hardening
- **Audit Impact**:
  - Process list: ``ps -ef | grep sshd-auth``
  - Log origin: ``grep "sshd-auth" /var/log/auth.log``

8. **Host Key Regeneration on Migration**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: OS upgrades regenerate host keys, breaking `known_hosts`
- **Audit Command**: Compare pre/post upgrade fingerprints: ``ssh-keygen -lf /etc/ssh/ssh_host_rsa_key``
- **Mitigation**: Preserve keys during migration

9. **Private Key Format Strictness**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: Keys require trailing newlines (breaks Ansible on V9.7)
- **Audit Command**: ``find / -name '*.pem' -exec awk 'END { if ($0 !~ /\n$/) print FILENAME }' {} \;``
- **Fix**: ``echo "" >> key.pem``

10. **DisplayPatchVersion for Scanners**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- **Change**: AIX-specific option shows patched version to vulnerability scanners
- **Activation**: Add to `/etc/ssh/sshd_config`::
      DisplayPatchVersion yes
- **Verification**: ``ssh -V`` shows upstream version (e.g., "OpenSSH_9.8p1")

Audit Summary Table
-------------------

.. csv-table:: Key Audit Commands for Deprecated Features
   :header: "Feature", "Command", "Splunk Query"
   :widths: 25, 40, 40

   "DSA Keys", "``find / -name 'id_dsa*'``", "``index=auth \"ssh_dss\"``"
   "DH KEX", "``grep \"diffie-hellman\" /var/log/auth.log``", "``index=network kex_dh_group*``"
   "ControlMaster", "``grep \"ControlMaster=yes\" ~/.ssh/config``", "``index=ssh_sessions ControlMaster=yes``"
   "Private Key Format", "``awk 'END { if ($0 !~ /\\n$/) print FILENAME }' key.pem``", "N/A"

References
----------
1. `OpenSSH 10.0 Release Notes <https://www.openssh.com/releasenotes.html>`_
2. `AIX DisplayPatchVersion Documentation <https://www.ibm.com/support/pages/aix-security-vulnerability-scanner-tools-fails-detect-openssh-version>`_
3. `AIX OpenSSH 9.7 Compatibility Issues <https://community.ibm.com/community/user/discussion/ssh-97>`_
4. `SSH Algorithm Deprecations on AIX <https://www.ibm.com/support/pages/ibm-aix-various-ssh-problems-after-upgrading-openssh-7x>`_
5. `Checking SSH Versions <https://www.cyberciti.biz/tips/find-check-opnessh-sshd-client-server-version-on-linux-unix.html>`_
6. `Host Key Preservation During Migration <http://gibsonnet.net/blog/dwarchive/SSH%20Host%20keys%20and%20AIX%207.1%20Migration.%20(Chris's%20AIX%20Blog).html>`_

Metadata
--------
:Request Date: 2025-07-10
:DeepSeek Parameters: 
  - Model: DeepSeek-R1
  - Temperature: 0.7
  - Max Tokens: 4096
:Tags: AIX, SSH, Security-Upgrade, Audit-Commands, Deprecated-Crypto, Splunk-Integration

Context
-------
- **User Request**: 
  - Provide a list of the ten most significant changes from SSH V8.1 to V9.7 on AIX
  - For items of lost functionality, provide commands to report usage extent on V8.1
  - Include Splunk/log exploration methods
  - Format as reStructuredText (rst) with specified sections
- **Search Results Utilized**: [1][2][3][4][5][6]
