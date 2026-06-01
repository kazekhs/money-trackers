# Panduan Deploy: Vercel (FE) + Railway (BE)

## Urutan deploy yang benar
1. Deploy backend ke Railway dulu
2. Dapat Railway URL → masukkan ke Vercel env vars
3. Deploy frontend ke Vercel

---

## 1. Deploy Backend ke Railway

### Langkah-langkah:
1. Push repo ke GitHub
2. Buka [railway.app](https://railway.app) → New Project → Deploy from GitHub
3. Pilih repo ini
4. Railway akan detect `railway.json` otomatis

### Environment Variables di Railway:
Masukkan semua ini di Railway dashboard → Settings → Variables:

```
SUPABASE_SERVICE_ROLE_KEY=   ← dari Supabase dashboard
VITE_SUPABASE_URL=           ← dari Supabase dashboard
CLASSIFIER_SPACE=LuxDream/receipt-category-classifier
CLASSIFIER_API_NAME=/predict_category
OCR_SERVICE_URL=https://luxdream-receipt-ocr-api.hf.space/ocr/receipt
FRONTEND_URL=                ← isi setelah dapat Vercel URL (contoh: https://money-tracker.vercel.app)
PORT=3001
```

> ⚠️ `FRONTEND_URL` bisa diisi belakangan setelah deploy Vercel selesai, lalu redeploy Railway.

Setelah deploy, Railway akan kasih URL seperti:
`https://money-tracker-server.up.railway.app`

---

## 2. Deploy Frontend ke Vercel

### Langkah-langkah:
1. Buka [vercel.com](https://vercel.com) → New Project → Import dari GitHub
2. Framework: **Vite**
3. Build command: `npm run build`
4. Output directory: `dist`

### Environment Variables di Vercel:
```
VITE_SUPABASE_URL=           ← sama seperti di Railway
VITE_SUPABASE_ANON_KEY=      ← dari Supabase dashboard (ANON key, bukan service role!)
VITE_API_URL=                ← Railway URL kamu, contoh: https://money-tracker-server.up.railway.app
```

> ⚠️ Jangan masukkan `SUPABASE_SERVICE_ROLE_KEY` ke Vercel — itu hanya untuk backend!

---

## 3. Setelah kedua platform deploy

1. Copy Vercel URL kamu (contoh: `https://money-tracker.vercel.app`)
2. Buka Railway → Settings → Variables → update `FRONTEND_URL` dengan Vercel URL
3. Railway akan auto-redeploy

---

## Checklist akhir
- [ ] Railway health check: `https://<railway-url>/api/health` → `{"status":"ok"}`
- [ ] Vercel bisa buka halaman login
- [ ] Upload struk → OCR jalan
- [ ] Klasifikasi kategori muncul
- [ ] Forgot password email redirect ke URL Vercel (bukan localhost)
