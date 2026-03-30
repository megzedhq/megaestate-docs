# Setup Web Application - Deploy Steps

## 1) Configure environment (React/Vite)

Set production values in your frontend environment file (example: `.env.production`):

```dotenv
VITE_APP_NAME=MegaClassify
VITE_API_BASE_URL=https://admin.yourdomain.com/api/v1
```

Also configure branding keys and analytics IDs if your build uses them.

## 2) Install and build (React/Vite)

From web source directory:

```bash
npm install
npm run build
```

Build output is typically generated in `dist/`.

## 3) Deploy build output to web server

- Upload `dist/` contents to your frontend domain document root
- If hosted in cPanel, use **File Manager** or Git deployment
- Configure SPA fallback so unknown routes return `index.html`
- Ensure HTTPS is enabled

### Nginx SPA fallback example

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

### Apache (`.htaccess`) SPA fallback example

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

## 4) Configure `.well-known` verification files (recommended)

For Android App Links and iOS Universal Links, deploy these static JSON files:

- `/.well-known/assetlinks.json`
- `/.well-known/apple-app-site-association`

They must return:
- `HTTP 200`
- `Content-Type: application/json`
- No redirects for these paths

### Nginx example for `.well-known`

```nginx
location = /.well-known/assetlinks.json {
  try_files $uri =404;
  add_header Content-Type application/json;
}

location = /.well-known/apple-app-site-association {
  try_files $uri =404;
  add_header Content-Type application/json;
}
```

### Apache (`.htaccess`) example for `.well-known`

```apache
RewriteEngine On
RewriteRule ^\.well-known/(assetlinks\.json|apple-app-site-association)$ - [L]

<Files "assetlinks.json">
  Header set Content-Type application/json
</Files>
<Files "apple-app-site-association">
  Header set Content-Type application/json
</Files>
```

## 5) Post-deploy validation

- Open homepage and listing pages
- Verify login/register actions
- Confirm API data loads without CORS errors
- Refresh direct route URLs (for example `/profile` or `/listing/123`) to validate SPA fallback
- Verify `.well-known` files with curl:

```bash
curl -I https://yourdomain.com/.well-known/assetlinks.json
curl -I https://yourdomain.com/.well-known/apple-app-site-association
```

## Troubleshooting

### Blank page after deploy

- Check browser console for missing asset paths
- Verify Vite `base` setting and deployment path
- Confirm `dist/assets/*` files are uploaded

### CORS error on API calls

- Allow frontend domain in backend CORS config
- Ensure preflight `OPTIONS` requests are handled

### 404 on refresh (SPA routes)

- Web server fallback rule is missing or incorrect
- Re-check Nginx/Apache rewrite config

### Android App Links not working

- Confirm `assetlinks.json` path and content
- Verify SHA-256 fingerprint matches release signing key
- Ensure no redirect on `/.well-known/assetlinks.json`

### iOS Universal Links not working

- Confirm AASA `appID` format is `TEAMID.bundleId`
- Ensure file is served with `application/json`
- Ensure no redirect on `/.well-known/apple-app-site-association`
