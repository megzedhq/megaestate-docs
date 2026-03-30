# Setup Admin Panel - Prerequisite

<div class="setup-banner">
  <div>
    <h3>Need a Faster Launch? We Can Help With Setup</h3>
    <p>Use the steps below for self-installation, or proceed directly to guided setup assistance.</p>
  </div>
  <a class="card-btn" href="installation/">Get Started</a>
</div>

The Admin Panel is the core of MegaClassify. Complete these requirements before installation.

!!! info "CodeCanyon Buyers"
    You must use this online guide together with the documentation included inside your CodeCanyon ZIP package.

## 1) Server Requirements

Recommended production stack:

- Linux (Ubuntu 20.04+/AlmaLinux/RockyLinux)
- cPanel/WHM access (or equivalent hosting control panel)
- Nginx (recommended) or Apache
- PHP 8.1+ with required extensions
- MySQL 5.7+ or MariaDB 10.4+
- Composer 2.x
- Node.js (if frontend assets need rebuild)
- Valid SSL certificate

## 2) Required PHP Extensions

Enable common Laravel extensions:

- bcmath
- ctype
- fileinfo
- json
- mbstring
- openssl
- pdo
- pdo_mysql
- tokenizer
- xml
- curl
- zip
- gd

## 3) Domain & DNS

Prepare:

- Admin domain/subdomain (example: `admin.example.com`)
- API endpoint domain if separated
- DNS `A` record pointing to server IP
- SSL certificate issuance readiness (AutoSSL/Let's Encrypt in cPanel)

## 4) Database Preparation

Create a dedicated database and user:

- Database Name: `{{DB_NAME}}`
- Username: `{{DB_USER}}`
- Password: `{{DB_PASSWORD}}`
- Host: `127.0.0.1` (or managed DB host)

Grant full privileges for this DB user on the created DB.

## 5) Buyer Verification Checklist (Pre-Install)

- [ ] Valid purchase code available
- [ ] CodeCanyon buyer account details verified
- [ ] CodeCanyon ZIP downloaded and extracted
- [ ] Included offline documentation reviewed
- [ ] Deployment target domain ready

## 6) Folder Ownership & Permissions Strategy

For cPanel hosting, keep Laravel writable directories accessible to the account user and web server process:

```bash
find /home/{{CPANEL_USER}}/{{APP_PATH}} -type f -exec chmod 644 {} \;
find /home/{{CPANEL_USER}}/{{APP_PATH}} -type d -exec chmod 755 {} \;
chmod -R 775 /home/{{CPANEL_USER}}/{{APP_PATH}}/storage
chmod -R 775 /home/{{CPANEL_USER}}/{{APP_PATH}}/bootstrap/cache
```

Avoid `777` in production.
