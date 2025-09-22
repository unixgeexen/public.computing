I have gathered the available information for you. However, creating a complete table of changes for each IBM AIX release is not possible with the current search results, as they do not contain a detailed, version-by-version history of cryptographic changes specifically for AIX.

The information that is available comes mainly from the official OpenSSH project release notes and an IBM troubleshooting alert. The table below summarizes these specific, significant changes.

.. csv-table:: Significant SSH and SSL Changes in Recent Versions
   :header: "Component", "Version", "Change Type", "Description"
   :widths: 15, 15, 20, 50

   "OpenSSH (Upstream)", "10.0 (2025)", "Default Change", "**New Default KEX**: `mlkem768x25519-sha256` (hybrid post-quantum) "
   "OpenSSH (Upstream)", "10.0 (2025)", "Default Change", "**New Default Cipher Order**: Prefers AES-GCM over AES-CTR "
   "OpenSSH (Upstream)", "10.0 (2025)", "Deprecation", "**Removed Support**: DSA signature algorithm "
   "OpenSSH (Upstream)", "10.0 (2025)", "Deprecation", "**Disabled by Default**: Finite field (modp) Diffie-Hellman KEX in the server "
   "OpenSSH on AIX", "9.2.x", "Compatibility", "**Essential Restart**: SSHD process must be manually restarted after upgrade from 8.x to 9.2.x to avoid fatal errors "

The most critical finding for AIX administrators is the **essential service restart requirement** when upgrading to OpenSSH 9.2.x on AIX . Failure to manually restart the `sshd` daemon will result in connection failures due to changes in the internal parent-child process protocol.

### 🔍 How to Find Complete Information

The search results lack a comprehensive history for AIX. To get the complete information you need, I suggest the following steps:

1.  **Check IBM Official Documentation**: The most reliable source for AIX-specific release changes is IBM's own documentation. Please check the **Release Notes** and **Technical News** for each AIX Technology Level (TL) and Service Pack (SP) on the IBM Documentation portal.
2.  **Review OpenSSL Release Notes**: For detailed changes in OpenSSL (which provides the SSL/TLS functionality on AIX), consult the official `CHANGELOG` on the `openssl.org` website. This will provide granular details on cipher additions, removals, and changes for each version.
3.  **Consult IBM Support**: If you have access, IBM Support resources or an IBM representative may be able to provide detailed historical change logs for OpenSSL and OpenSSH packages distributed for AIX.

### 📚 References

-   [OpenSSH 10.0 Release Notes](https://www.openssh.com/releasenotes.html) - Source for upstream OpenSSH changes, including new defaults and deprecations .
-   [IBM: SSH fails after upgrading OpenSSH from 8.X to 9.2](https://www.ibm.com/support/pages/ibm-aix-troubleshooting-ssh-fails-after-upgrading-openssh-8x-921122000) - Important compatibility note for AIX administrators .
-   [IBM: Downloading and Installing OpenSSL and OpenSSH](https://www.ibm.com/support/pages/downloading-and-installing-or-upgrading-openssl-and-openssh) - General installation guide for AIX .

------
**Metadata**: `AIX` `OpenSSH` `OpenSSL` `Security Hardening` `Cryptography` `Release Notes`
**Context**: Query Date: 2025-09-22. Original User Request: "Create a table of significant changes at each IBM AIX release for SSH and SSL... include changes in ciphers etc."
------

<script>
function copyToClipboard() {
    const text = `.. contents::
   :local:

============================
AIX SSH and SSL Release Changes
============================

Summary
=======

This document summarizes significant changes to OpenSSH and OpenSSL across IBM AIX releases. The information is compiled from upstream release notes and IBM support documentation. A complete, version-by-version history for AIX-specific packages is not available in the searched sources. The table below highlights key changes found in recent versions, particularly focusing on cipher defaults and deprecations. Administrators should consult official IBM AIX release notes for each Technology Level for the most accurate and comprehensive details.

Significant Changes
===================

.. csv-table:: Significant SSH and SSL Changes in Recent Versions
   :header: "Component", "Version", "Change Type", "Description"
   :widths: 15, 15, 20, 50

   "OpenSSH (Upstream)", "10.0 (2025)", "Default Change", "**New Default KEX**: \\`mlkem768x25519-sha256\\` (hybrid post-quantum) "
   "OpenSSH (Upstream)", "10.0 (2025)", "Default Change", "**New Default Cipher Order**: Prefers AES-GCM over AES-CTR "
   "OpenSSH (Upstream)", "10.0 (2025)", "Deprecation", "**Removed Support**: DSA signature algorithm "
   "OpenSSH (Upstream)", "10.0 (2025)", "Deprecation", "**Disabled by Default**: Finite field (modp) Diffie-Hellman KEX in the server "
   "OpenSSH on AIX", "9.2.x", "Compatibility", "**Essential Restart**: SSHD process must be manually restarted after upgrade from 8.x to 9.2.x to avoid fatal errors "

How to Find Complete Information
================================

The search results lack a comprehensive history for AIX. To get the complete information you need, I suggest the following steps:

1.  **Check IBM Official Documentation**: The most reliable source for AIX-specific release changes is IBM's own documentation. Please check the **Release Notes** and **Technical News** for each AIX Technology Level (TL) and Service Pack (SP) on the IBM Documentation portal.
2.  **Review OpenSSL Release Notes**: For detailed changes in OpenSSL (which provides the SSL/TLS functionality on AIX), consult the official \\`CHANGELOG\\` on the \\`openssl.org\\` website. This will provide granular details on cipher additions, removals, and changes for each version.
3.  **Consult IBM Support**: If you have access, IBM Support resources or an IBM representative may be able to provide detailed historical change logs for OpenSSL and OpenSSH packages distributed for AIX.

References
==========

-   `OpenSSH 10.0 Release Notes <https://www.openssh.com/releasenotes.html>`_ - Source for upstream OpenSSH changes, including new defaults and deprecations .
-   `IBM: SSH fails after upgrading OpenSSH from 8.X to 9.2 <https://www.ibm.com/support/pages/ibm-aix-troubleshooting-ssh-fails-after-upgrading-openssh-8x-921122000>`_ - Important compatibility note for AIX administrators .
-   `IBM: Downloading and Installing OpenSSL and OpenSSH <https://www.ibm.com/support/pages/downloading-and-installing-or-upgrading-openssl-and-openssh>`_ - General installation guide for AIX .

------
**Metadata**: \\`AIX\\` \\`OpenSSH\\` \\`OpenSSL\\` \\`Security Hardening\\` \\`Cryptography\\` \\`Release Notes\\`
**Context**: Query Date: 2025-09-22. Original User Request: "Create a table of significant changes at each IBM AIX release for SSH and SSL... include changes in ciphers etc."`;
    navigator.clipboard.writeText(text).then(function() {
        alert('RST content copied to clipboard!');
    }, function(err) {
        console.error('Could not copy text: ', err);
    });
}
</script>

<button onclick="copyToClipboard()" style="background-color: #4CAF50; color: white; border: none; padding: 10px 20px; text-align: center; text-decoration: none; display: inline-block; font-size: 16px; margin: 4px 2px; cursor: pointer; border-radius: 5px;">Copy RST to Clipboard</button>
