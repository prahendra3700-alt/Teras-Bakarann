# Mulai di sini — Teras Bakaran

Untuk teman yang membantu memasang website dari PC.

## Sudah selesai (jangan diulang)

- **Database Supabase sudah online** dan sudah terisi: proyek `teras-bakaran`
  (`https://xgaufhfjqiramugdkgxn.supabase.co`), semua migrasi, 7 kategori + 21 menu,
  dan email pemilik `doniwae2003@gmail.com` sudah terdaftar.
  → **Lewati Langkah 1 dan 2** di `docs/PANDUAN-PC.md`.
- File `apps/web/.env` dan `apps/admin/.env` sudah terisi. Isinya hanya alamat dan kunci **publik**,
  jadi aman.
- Website dan dashboard **sudah di-build**, siap diunggah di folder `siap-unggah/`.

## Yang tersisa: taruh 2 situs di Cloudflare Pages (±5 menit)

### Cara paling mudah (lewat browser, tanpa perintah)

Di folder `siap-unggah/` ada `website-pelanggan.zip` dan `dashboard-admin.zip`.
Di dash.cloudflare.com → **Workers & Pages → Create → Pages → Upload assets**:
1. Project name `teras-bakaran` → unggah `website-pelanggan.zip` → Deploy.
2. Project name `teras-bakaran-admin` → unggah `dashboard-admin.zip` → Deploy.

Detail: `siap-unggah/CARA-UNGGAH.txt`.

### Cara lewat terminal

Butuh **Node.js 22** (https://nodejs.org) dan akun Cloudflare milik pemilik
(atau pemilik login di PC Anda).

Buka terminal (Windows: PowerShell) **di folder `teras-bakaran` ini**, lalu jalankan satu per satu:

```bash
npx wrangler@4 login
```

```bash
npx wrangler@4 pages project create teras-bakaran --production-branch main
```

```bash
npx wrangler@4 pages project create teras-bakaran-admin --production-branch main
```

```bash
npx wrangler@4 pages deploy siap-unggah/website --project-name teras-bakaran --branch main
```

```bash
npx wrangler@4 pages deploy siap-unggah/dashboard --project-name teras-bakaran-admin --branch main
```

Selesai. Alamatnya:
- Website pelanggan: https://teras-bakaran.pages.dev
- Dashboard pemilik: https://teras-bakaran-admin.pages.dev

> Jika wrangler bilang nama proyek sudah dipakai orang lain, alamatnya akan berbeda. Pakai nama lain,
> lalu ganti `VITE_SITUS_URL` di `apps/admin/.env`, build ulang dashboard
> (`corepack enable`, `pnpm install`, `pnpm --filter @teras/admin build`, hasil di `apps/admin/dist`),
> dan beri tahu pemilik agar **Site URL** di Supabase (Authentication → URL Configuration) diganti
> ke alamat dashboard yang baru.

## Setelah online

Ikuti `docs/PANDUAN-PC.md` **Langkah 4** (uji ±5 menit): pemilik mendaftar di dashboard dengan
email `doniwae2003@gmail.com`, mengisi titik lokasi toko, dan memasukkan API key openrouteservice.

## Cara lain: otomatis lewat GitHub

Repo `asfhia/teras-bakaran` sudah punya alur `.github/workflows/terbit.yml` (cabang
`claude/amazing-hamilton-0b42r9`). Cukup isi secret repo `CLOUDFLARE_API_TOKEN` dengan
**kode token** Cloudflare (sekitar 40 karakter acak dari template "Edit Cloudflare Workers", bukan nama tokennya).
Setelah itu setiap push akan mem-build dan mengunggah kedua situs sendiri.
