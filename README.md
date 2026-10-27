# DINAR – situs + admin untuk Cloudflare Workers

Struktur (taruh langsung di ROOT repo): wrangler.jsonc dan worker.js (hanya dua file, tanpa folder)

## Pemasangan
1. Dashboard → Storage & databases → Workers KV → Create namespace (nama bebas, mis. dinar-config). Salin ID-nya.
2. Edit wrangler.jsonc di repo:
   - "name" harus sama dengan nama Worker di dashboard.
   - ganti ISI_ID_KV_DI_SINI dengan ID KV tadi.
3. Commit & push. Build akan jalan otomatis (deploy command: npx wrangler deploy).
4. Worker → Settings → Variables and Secrets → tambah SECRET: ADMIN_PASSWORD dan TOKEN_SECRET.
5. Buka https://<worker>.workers.dev/admin → login → isi pengaturan. Isi "link APK eksternal" (https) atau pakai R2 (lihat bawah).
6. Domain: Worker → Settings → Domains & Routes → Add custom domain.

## Memakai R2 untuk file APK (opsional)
1. Aktifkan R2 di dashboard, buat bucket (mis. dinar-apk), upload file .apk.
2. Di wrangler.jsonc aktifkan baris r2_buckets dan samakan bucket_name. Commit.
3. Di /admin pilih file APK dari daftar.

## Error umum
- "inherit binding ... is invalid [code 10057]": binding ditulis tanpa ID/nama. Isi ID KV (dan bucket_name bila pakai R2), jangan mengandalkan auto-provisioning.
- "Please enable R2" (10042): aktifkan R2 atau jangan pakai r2_buckets.
- Error nama Worker: samakan "name" dengan nama di dashboard.
- File tidak ditemukan: pastikan wrangler.jsonc dan worker.js ada di ROOT repo.

## Keamanan
- Rate limit publik: Security → WAF → Rate limiting rules untuk /api/* (mis. 60 req/menit/IP).
- Lindungi /admin dengan Cloudflare Access (gratis sampai 50 pengguna).
- Batas login memakai KV dan bersifat perkiraan. Hanya bagikan APK yang kamu punya hak distribusinya.
