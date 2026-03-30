# Post-Install Settings (Admin Panel)

After your `/install` wizard is completed and you can login to the admin panel, finish these settings to make the system production-ready.

---

## 1) Login to Admin Panel

1. Open the admin login URL shown on the installer finish screen.
2. Login with the email + password shown.

✅ **Expected Result:** You can access the admin dashboard.

---

## 2) Firebase / Google settings

Go to:

**Admin Panel → Settings → Firebase / Google**

### Steps
1. Upload your Firebase service account JSON.
2. Fill required Firebase keys (if shown).
3. Save.

### Checklist
- [ ] JSON uploaded successfully
- [ ] Firebase keys saved
- [ ] Test login/notification works (if your app uses it)

> ⚠️ Common mistakes:
> - Uploading wrong JSON (not service account)
> - JSON has invalid format
> - Missing required Firebase fields

---

## 3) Firebase Authorised Domains (for web auth)

In Firebase Console:

**Authentication → Settings → Authorised domains**

Add your production domains:

- `yourdomain.com`
- `www.yourdomain.com`
- admin domain if auth redirects happen there

### Checklist
- [ ] Frontend domains are added
- [ ] Auth/login no longer throws unauthorized-domain errors

---

## 4) Push Notification setup (FCM)

In Admin Panel, open push notification settings and add your FCM key/config required by your package version.

### Checklist
- [ ] FCM config saved in admin panel
- [ ] Test push notification is delivered

---

## 5) SMTP email settings

Go to:

**Admin Panel → Settings → Email / SMTP**

### Steps
1. Fill:
   - SMTP Host
   - Port
   - Username
   - Password
   - Encryption (`tls` / `ssl`)
   - From Name + From Email
2. Click **Save**
3. Send a **Test Email**.

### Checklist
- [ ] SMTP saved
- [ ] Test email sent successfully

> ⚠️ Common mistakes:
> - Wrong port + encryption combo
> - Using Gmail without App Password
> - Server blocks outbound SMTP

---

## 6) AI settings (OpenAI / Gemini)

Go to:

**Admin Panel → Settings → AI**

### Steps
1. Add OpenAI API key and/or Gemini API key.
2. Save.
3. Run a test generation (if your panel provides test button).

### Checklist
- [ ] API keys saved (no extra spaces)
- [ ] Test success

> ⚠️ Common mistakes:
> - Keys pasted with quotes/spaces
> - Provider disabled but key added

---

## 7) Final verification

✅ Confirm these work:
- Admin login works
- DB is connected
- Uploads work (`public/storage`)
- Email works (test email)
- AI works (test prompt)
- Mobile login + push flow works
- Web login works without unauthorized-domain errors
