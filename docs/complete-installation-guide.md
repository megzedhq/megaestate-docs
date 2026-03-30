# Complete Installation Guide (From DOCX)

This page mirrors the key implementation steps from `MegaClassify_Installation_Guide_Updated.docx` so all setup instructions are available directly in this documentation site.

## Part 1 — Laravel Admin Panel (cPanel / aaPanel)

### 1. Create domain and database
1. Create an API domain/subdomain (example: `api.example.com`).
2. Create database + DB user in cPanel/MySQL Databases.
3. Assign user to DB with **ALL PRIVILEGES**.

### 2. Upload project and prepare `.env`
1. Upload admin panel ZIP to domain root and extract it.
2. Confirm Laravel files exist (`artisan`, `composer.json`, `public/`, `storage/`, `bootstrap/cache/`).
3. Create/update `.env` with production values:

```env
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

### 3. Set document root and run terminal setup
Set document root to Laravel `public` folder:

```txt
/home/CPANELUSER/api.example.com/public
```

Then run:

```bash
cd /home/CPANELUSER/api.example.com
composer install --no-dev --optimize-autoloader
php artisan key:generate
php artisan optimize:clear
php artisan storage:link
chmod -R 775 storage bootstrap/cache
```

### 4. Run installer and complete post-install setup
1. Open `https://api.example.com/install`.
2. Complete requirement checks and DB configuration.
3. Log in to admin panel and configure:
   - Firebase/Google settings
   - SMTP mail settings
   - Push notifications
   - AI provider keys (if enabled)
4. Add scheduler cron job:

```bash
* * * * * php /home/CPANELUSER/api.example.com/artisan schedule:run >/dev/null 2>&1
```

## Part 2 — Flutter Mobile App (Android & iOS)

### 1. Prerequisites
- Flutter SDK
- Android Studio + SDK
- Xcode (for iOS)
- Firebase project
- Play Console / Apple Developer accounts

### 2. Project configuration
1. Run:

```bash
flutter clean
flutter pub get
```

2. Configure app identity values:
   - Android package name
   - iOS bundle ID
   - App name and launcher/splash assets

3. Place Firebase files:
   - `android/app/google-services.json`
   - `ios/Runner/GoogleService-Info.plist`

4. Set API URL to Laravel backend (`https://admin.yourdomain.com/api/v1` style).

### 3. Android/iOS build
Android release:

```bash
flutter build apk --release
# or
flutter build appbundle --release
```

iOS release:

```bash
flutter build ios --release
```

## Part 3 — React/Vite Web App (Nginx / Apache)

### 1. Build
Set `.env.production`:

```env
VITE_API_BASE_URL=https://admin.yourdomain.com/api/v1
```

Then run:

```bash
npm install
npm run build
```

Upload `dist/` contents to web domain document root.

### 2. SPA routing fallback
Nginx example:

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

Apache (`.htaccess`) example:

```apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule ^ index.html [L]
```

### 3. `.well-known` deep-link verification files
Keep these URLs directly accessible with `application/json` content type:
- `/.well-known/assetlinks.json`
- `/.well-known/apple-app-site-association`

Validate after deployment:

```bash
curl -I https://yourdomain.com/.well-known/assetlinks.json
curl -I https://yourdomain.com/.well-known/apple-app-site-association
```

Expected: `HTTP 200`, `content-type: application/json`, no redirects.

## Final cross-platform go-live checklist

### Admin/API
- [ ] Admin panel opens on HTTPS
- [ ] API ping returns JSON
- [ ] Uploads/images working
- [ ] `APP_DEBUG=false`
- [ ] Scheduler cron configured

### Mobile
- [ ] Correct package/bundle IDs configured
- [ ] Firebase files added
- [ ] FCM key configured in admin
- [ ] Release build succeeds
- [ ] Real-device login/listing/chat tested

### Web
- [ ] Correct `VITE_API_BASE_URL`
- [ ] `npm run build` succeeds
- [ ] SPA fallback active
- [ ] HTTPS active
- [ ] No CORS errors in browser console

---

For the original formatted source, see `site/pdf/MegaClassify_Installation_Guide_Updated.docx`.
