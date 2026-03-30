# Setup Admin Panel - Configuration

After installation, finalize production-safe configuration.

## 1) Essential `.env` Configuration

Review and tune:

- `APP_ENV=production`
- `APP_DEBUG=false`
- `APP_URL=https://{{YOUR_DOMAIN}}`
- DB credentials
- Mail credentials
- Queue/session/cache drivers

Never commit real `.env` values to Git.

## 2) Queue & Cron (Recommended)

For better performance, configure worker/cron if package requires background jobs.

Typical scheduler cron:

```bash
* * * * * cd /home/{{CPANEL_USER}}/{{APP_PATH}} && php artisan schedule:run >> /dev/null 2>&1
```

## 3) Web Server Best Practices (Apache/Nginx)

- Enforce HTTPS
- Enable gzip/brotli (if available)
- Add reasonable client upload size for listing media
- Block direct access to sensitive files (`.env`, `.git`)

Sample location protection snippet:

```nginx
location ~ /\.(env|git) {
    deny all;
}
```

## 4) CORS/API Considerations

If mobile or web app cannot call API:

- Confirm `APP_URL` and API base URL are correct
- Configure allowed origins in CORS settings
- Ensure `OPTIONS` preflight requests are handled
- Validate SSL and intermediate certificates

## 5) File Permission Policy

Writable paths:

- `storage/`
- `bootstrap/cache/`

Use least-privilege settings and avoid world writable permissions.

## 6) Troubleshooting Guide

### HTTP 500 Error

1. Check Laravel log:

```bash
tail -n 200 storage/logs/laravel.log
```

2. Check Apache/Nginx error logs from cPanel (Metrics/Errors or domain logs)
3. Validate PHP version/extensions
4. Re-run:

```bash
php artisan optimize:clear
```

### Storage Link Issue (Images Not Loading)

```bash
php artisan storage:link
```

Ensure `public/storage` points to `storage/app/public`.

### CORS Errors

- Verify frontend origin is allowed
- Clear config cache:

```bash
php artisan optimize:clear
```

- Confirm reverse proxy is not stripping headers

### Permission Denied Errors

```bash
find /home/{{CPANEL_USER}}/{{APP_PATH}} -type f -exec chmod 644 {} \;
find /home/{{CPANEL_USER}}/{{APP_PATH}} -type d -exec chmod 755 {} \;
chmod -R 775 storage bootstrap/cache
```

### Migration Issues

```bash
php artisan migrate --force
```

Confirm DB user privileges (`ALTER`, `CREATE`, `INDEX`, `INSERT`, `UPDATE`, `DELETE`).

## 7) Backup & Update Policy

Before each update:

- Backup database
- Backup `.env`
- Backup uploaded media (`storage/app/public`)

After update:

```bash
composer install --no-dev -o
php artisan migrate --force
php artisan optimize:clear
```
