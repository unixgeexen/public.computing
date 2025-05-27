=====================================
Nginx Installation & SSL Configuration
======================================

This guide covers professional Nginx setup with Let's Encrypt SSL on Ubuntu systems.

.. contents:: Table of Contents
   :depth: 2
   :local:

Prerequisites
=============
- Ubuntu server (20.04/22.04 LTS recommended)
- Root or sudo privileges
- Domain name pointing to server IP
- Ports 80 and 443 open in firewall

Installation
============

Nginx Installation
------------------
.. code-block:: bash

   # Update package lists
   sudo apt update

   # Install Nginx
   sudo apt install nginx -y

   # Enable and start service
   sudo systemctl enable --now nginx
   sudo systemctl status nginx

Basic Configuration
===================

Virtual Host Setup
------------------
.. code-block:: bash

   # Create site directory structure
   sudo mkdir -p /var/www/example.com/{html,logs}
   sudo chown -R www-data:www-data /var/www/example.com
   sudo chmod -R 755 /var/www/example.com

Configuration File
------------------
Create ``/etc/nginx/sites-available/example.com``:

.. code-block:: nginx

   server {
       listen 80;
       server_name example.com www.example.com;

       root /var/www/example.com/html;
       index index.html;

       access_log /var/www/example.com/logs/access.log;
       error_log /var/www/example.com/logs/error.log;

       location / {
           try_files $uri $uri/ =404;
       }
   }

Enable the site:

.. code-block:: bash

   sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl reload nginx

SSL Configuration
=================

Certbot Installation
--------------------
.. code-block:: bash

   sudo apt install certbot python3-certbot-nginx -y

Certificate Obtainment
----------------------
.. code-block:: bash

   # Single domain
   sudo certbot --nginx -d example.com

   # Multiple domains
   sudo certbot --nginx -d example.com -d www.example.com

Auto-Renewal Setup
------------------
.. code-block:: bash

   # Test renewal process
   sudo certbot renew --dry-run

SSL Hardening
-------------
Add to your SSL server block:

.. code-block:: nginx

   ssl_protocols TLSv1.2 TLSv1.3;
   ssl_prefer_server_ciphers on;
   ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256';
   ssl_ecdh_curve secp384r1;
   ssl_session_timeout 10m;
   ssl_session_cache shared:SSL:10m;
   ssl_stapling on;

Verification
============
.. code-block:: bash

   # Check SSL certificate
   openssl s_client -connect example.com:443 | openssl x509 -noout -text

   # Test configuration
   sudo nginx -t && sudo systemctl restart nginx

Maintenance
===========
Common commands:

.. list-table:: 
   :widths: 30 70
   :header-rows: 1

   * - Command
     - Purpose
   * - ``sudo certbot renew``
     - Manually renew certificates
   * - ``sudo journalctl -u nginx``
     - View service logs
   * - ``sudo nginx -T``
     - Show full configuration

Troubleshooting
===============
Common Issues:

.. admonition:: 502 Bad Gateway
   :class: error

   - Verify backend services are running
   - Check proxy_pass targets are correct

.. admonition:: SSL Not Working
   :class: warning

   - Confirm port 443 is open
   - Verify DNS records propagate
   - Check certificate paths in Nginx config

Next Steps
==========
- Set up reverse proxy for applications
- Configure HTTP basic authentication
- Implement rate limiting
- Enable fail2ban for brute force protection

Appendix
========
External Resources:

- `Nginx Official Docs <https://nginx.org/en/docs/>`_
- `Certbot Documentation <https://certbot.eff.org/docs/>`_
- `SSL Labs Test <https://www.ssllabs.com/ssltest/>`_
