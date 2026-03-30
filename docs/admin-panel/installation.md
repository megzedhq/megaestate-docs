# Admin Panel Installation Guide (cPanel + Manual .env + Terminal + /install)

This guide is for beginners. Follow each step in order to install the MegaClassify Laravel Admin Panel (API backend) on cPanel shared hosting.

---

## 1) Create domain (cPanel → Domains)

1. Log in to **cPanel**.
2. Open **Domains**.
3. Create a new domain/subdomain:
   - `api.example.com`
4. Note the folder path created by cPanel, for example:
   - `/home/CPANELUSER/api.example.com`

### Checklist
- [ ] Domain/subdomain `api.example.com` is created
- [ ] Domain folder exists in File Manager

✅ **Expected Result:** domain is created and folder exists.

> ⚠️ **Common Mistakes**
> - Creating the domain with a typo (for example `ap1.example.com`).
> - Using the wrong folder path later in terminal commands.

---

## 2) Create DB + user (cPanel → MySQL Databases)

1. Open **MySQL Databases** in cPanel.
2. Create a new database (example):
   - `example_api`
3. Create a new database user (example):
   - `example_user`
4. Add the user to the database.
5. Grant **ALL PRIVILEGES**.

### Checklist
- [ ] Database created
- [ ] DB user created
- [ ] User added to DB
- [ ] ALL PRIVILEGES enabled

✅ **Expected Result:** DB user has full access.

> ⚠️ **Common Mistakes**
> - Forgetting to add user to database.
> - Not giving **ALL PRIVILEGES**.
> - Forgetting cPanel prefixes (actual names often look like `cpuser_example_api`).

---

## 3) Upload ZIP + unzip (cPanel → File Manager)

1. Open **File Manager**.
2. Go to:
   - `/home/CPANELUSER/api.example.com`
3. Upload your admin panel ZIP file.
4. Extract/unzip the file in this same folder.
5. Confirm these items exist:
   - `artisan`
   - `composer.json`
   - `public/`
   - `storage/`
   - `bootstrap/cache/`

### Checklist
- [ ] ZIP uploaded
- [ ] ZIP extracted
- [ ] Laravel files/folders confirmed

✅ **Expected Result:** Laravel project files are present.

> ⚠️ **Common Mistakes**
> - Extracting ZIP into a subfolder (for example `/api.example.com/project-name/`) instead of the domain root.
> - Uploading incomplete project files.

---

## 4) Manually edit `.env` (before installer)

1. In **File Manager**, open `.env` in:
   - `/home/CPANELUSER/api.example.com/.env`
2. If `.env` is missing, create it by copying:
   - `.env.example` → `.env`
3. Fill/update these values:

```env
APP_NAME="Your App Name"
APP_ENV=production
APP_DEBUG=false
APP_URL=https://api.example.com

DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=YOUR_DB_NAME
DB_USERNAME=YOUR_DB_USER
DB_PASSWORD="YOUR_DB_PASS"
```

4. Save the file.
5. Make sure `.env` is writable by your cPanel user.

### Checklist
- [ ] `.env` exists
- [ ] DB values are correct
- [ ] `APP_URL` is `https://api.example.com`
- [ ] File saved successfully

✅ **Expected Result:** `.env` has correct DB + URL values.

> ⚠️ **Common Mistakes**
> - Wrong DB credentials.
> - Leaving `APP_DEBUG=true` in production.
> - Setting `APP_URL` to the main website instead of API domain.

---

## 5) Set document root to `/public` (cPanel)

1. Go to **cPanel → Domains**.
2. Click **Manage** for `api.example.com`.
3. Set **Document Root** to:

```txt
/home/CPANELUSER/api.example.com/public
```

4. Save changes.

### Checklist
- [ ] Document Root updated to `/public`
- [ ] Saved in cPanel successfully

✅ **Expected Result:** Laravel runs from `/public` (secure + correct).

> ⚠️ **Common Mistakes**
> - Keeping document root at `/home/CPANELUSER/api.example.com` (not secure and usually breaks routing).

---

## 6) Run terminal commands (cPanel → Terminal or SSH)

Run these commands in order.

```bash
cd /home/CPANELUSER/api.example.com
composer install --no-dev --optimize-autoloader
php artisan key:generate
php artisan optimize:clear
php artisan storage:link
chmod -R 775 storage bootstrap/cache
```

### Checklist
- [ ] `vendor/` folder created
- [ ] No fatal permission errors
- [ ] `public/storage` exists as a symlink

✅ **Expected Result:**
- `vendor/` exists
- no permission errors
- `public/storage` is a symlink

> ⚠️ **Common Mistakes**
> - Running commands in the wrong folder.
> - Composer not available in cPanel environment.
> - Permissions not updated for `storage` and `bootstrap/cache`.

---

## 7) Open `/install` and finish wizard

1. Open:
   - `https://api.example.com/install`
2. **Screen 1:** Requirements check (PHP extensions, permissions, storage paths).
3. Continue to the next screen.
4. Fill installer fields:
   - Database details
   - Host URL
   - Public Site URL (if shown)
5. Click **Install**.

### Finish screen should show
- Admin login URL
- Login email
- Login password

### Checklist
- [ ] Requirements passed
- [ ] Install process completed
- [ ] Credentials shown on finish screen

✅ **Expected Result:** install completed and admin URL shown.

> ⚠️ **Common Mistakes**
> - Trying to run installer before `.env` is correct.
> - Ignoring failed requirements (permissions/extensions).

---

## 8) Verify API + storage after install

Run these checks before moving to mobile/web setup:

```bash
curl -I https://api.example.com
curl https://api.example.com/api/v1/ping
```

Also verify image upload path:

- `public/storage` must point to `storage/app/public`
- Upload one test image from admin panel and confirm it loads on the frontend URL

### Checklist
- [ ] API domain responds over HTTPS
- [ ] Ping endpoint returns JSON
- [ ] Uploaded image renders correctly

✅ **Expected Result:** backend is reachable and ready for app integrations.

---

## 9) aaPanel alternative (if not using cPanel)

If you deploy on **aaPanel**, keep the same Laravel principles:

- Site root should point to Laravel `public/`
- PHP version/extensions must match requirements
- Run the same Laravel setup commands
- Ensure `storage/` and `bootstrap/cache/` are writable
- Configure SSL before go-live

---

## 10) Troubleshooting

### 1. 500 error after document root change
- Confirm document root is exactly:
  - `/home/CPANELUSER/api.example.com/public`
- Confirm project files are one level above `public`.
- Run:

```bash
cd /home/CPANELUSER/api.example.com
php artisan optimize:clear
```

### 2. DB connection failed
- Recheck `.env` values:
  - `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`
- Ensure DB user is added to DB with **ALL PRIVILEGES**.
- Remember cPanel DB names often include account prefix.

### 3. Composer install failed
- If memory limit error appears, increase PHP/CLI memory in cPanel if available.
- If `composer` command is missing, use host-provided Composer path or ask hosting support to enable Composer.

### 4. `storage` / `bootstrap/cache` not writable
- Re-run:

```bash
cd /home/CPANELUSER/api.example.com
chmod -R 775 storage bootstrap/cache
```

### 5. `public/storage` not working
- `public/storage` must be a symlink, not a normal folder.
- Fix by removing wrong folder and running:

```bash
cd /home/CPANELUSER/api.example.com
php artisan storage:link
```

### 6. Seeder skipped (DB already has users)
- This is normal if database is not fresh.
- Installer usually seeds only on a fresh/empty DB.
- Existing users/data remain unchanged.

### 7. `/install` still accessible after install
- Check if lock file exists:
  - `storage/app/installed.lock`
- If missing, verify write permissions.
- If lock exists but page still opens, review installer middleware/route protection in your codebase.

✅ **Expected Result:** common issues are identified and fixed quickly.

> ⚠️ **Common Mistakes**
> - Changing many settings at once and not retesting step-by-step.
> - Not clearing cache after config fixes.
