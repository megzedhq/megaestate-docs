# FAQs

## Do I only need this website to install MegaClassify?
No. You must also read and follow the documentation included inside the CodeCanyon ZIP package delivered with your purchase.

## I completed installation but get a 500 error. What should I check?
- Confirm `.env` values are valid
- Check PHP version and required extensions
- Verify file permissions for `storage/` and `bootstrap/cache/`
- Run cache clear commands:

```bash
php artisan optimize:clear
```

## Uploaded images are not visible.
Most commonly, the storage symlink is missing. Run:

```bash
php artisan storage:link
```

## Mobile/Web app cannot connect to API.
- Check API base URL is correct (`https://apimega.megzed.com` format)
- Validate CORS configuration in backend
- Confirm SSL certificate is valid
- Ensure no firewall blocks API routes

## Database migration failed on production.
Use safe migration command:

```bash
php artisan migrate --force
```

Also ensure database credentials and privileges are correct.
