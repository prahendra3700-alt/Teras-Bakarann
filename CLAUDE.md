# CLAUDE.md — Teras Bakaran

Website pemesanan + dashboard admin untuk **Teras Bakaran** (Modern Indonesian Street Café).
Tagline: KOPI · MAKAN · NGEMIL · NONGKRONG.

## Aturan kerja

- Pemilik bukan programmer. Jelaskan setiap keputusan dalam **bahasa Indonesia sederhana**.
- Kerjakan per fase (lihat `PROGRESS.md`). Jangan lompat fase. Setiap fase diuji dan dicatat di `PROGRESS.md` sebelum lanjut.
- **Jangan mengarang** data produk, harga, foto, alamat, jam buka, atau kontak. Jika belum ada, pakai placeholder yang jelas bertanda "contoh" / "foto segera hadir", lalu tanyakan.
- Jangan mengambil keputusan bisnis (harga, tarif, metode bayar, nama menu) tanpa persetujuan pemilik.
- Jangan menghapus pekerjaan sebelumnya.
- Jangan menyimpan kredensial rahasia di frontend. Harga & ongkir selalu dihitung ulang di server.
- Jangan membuat sistem pembayaran palsu. Status bayar harus jujur ("Menunggu verifikasi", "Bayar di tempat", "Lunas" hanya jika benar-benar terkonfirmasi).

## Rancangan acuan

Semua keputusan desain & arsitektur ada di `docs/FASE-1-RANCANGAN.md`. Keputusan terbaru (berlaku jika berbeda) ada di `docs/FASE-1-KEPUTUSAN.md`. Data menu ada di `docs/data/MENU.md`.

### Data bisnis yang sudah dikonfirmasi
- WhatsApp utama 0851-5711-9609 (`6285157119609`), cadangan 0896-9162-4300 (`6289691624300`).
- 7 kategori: Bakaran, Mie Jebeu, Pisang Kembung, Pisang Wijen, Cemilan, Es Teh, Teras Blend. Jangan memakai nama "Mie Gacoan".
- Ongkir (final dari pemilik; disimpan di database, bisa diubah admin): ambil sendiri gratis · <3 km Rp5.000 · 3–<8 km Rp8.000 · 8–10 km Rp12.000 · >10–15 km ongkir ditentukan admin via WA · >15 km tidak dilayani. Minimal belanja antar Rp15.000. **Jarak = RUTE JALAN** (keputusan pemilik), bukan garis lurus: OpenRouteService (kunci API milik pemilik, disimpan di skema `pribadi`) → cadangan OSRM → jika gagal, ongkir "dikonfirmasi admin". Jangan kembali ke garis lurus.
- Jam buka: setiap hari 15.00–22.00 WIB (Asia/Jakarta).
- Lokasi: link Maps `https://maps.google.com?ftid=0x2e33e98105f908cb:0xedc2d44227cd68d`. Koordinat diisi sendiri oleh pemilik di dashboard (Pengaturan → Lokasi toko) → jangan dikarang.
- TikTok `@terasbakarand6`. Instagram belum dikonfirmasi.
- QRIS statis a.n. TERAS BAKARAN, NMID ID1026600840908 → `brand/pembayaran/qris-teras-bakaran.jpg`. Tampilkan apa adanya (jangan diubah/dipotong).
- Anggaran Rp0/bulan: Cloudflare Pages, Supabase Free, OpenStreetMap/Leaflet, Google Maps embed, wa.me link. Jangan menambah layanan berbayar tanpa izin pemilik.
- QRIS: manual dulu (unggah bukti, admin verifikasi). Gateway otomatis menyusul jika pemilik mendaftar.
- Menu Bakaran diisi sendiri oleh pemilik lewat dashboard (seed kosong).
- Diisi pemilik nanti lewat dashboard (jangan dikarang): koordinat toko, alamat tertulis, foto produk, menu Bakaran.

### Warna (token)
maroon `#641F2B` (tombol utama, harga) · cream `#F4EBDD` (latar) · coffee `#654638` (dekorasi, teks sekunder) · espresso `#211A18` (teks, footer) · warm-white `#FFFDF8` (kartu) · beige `#D9CBBB` (garis/placeholder — **jangan untuk teks**).
Maroon tidak boleh di atas espresso (kontras 1,45).

### Logo
Logo resmi dari pemilik ada di `brand/logo/` (lihat README di sana). Jangan mengubah desainnya. Gunakan logo ringkas untuk header, logo lengkap untuk hero/footer, dan ikon api untuk favicon.

### Huruf
Fraunces (judul) + Plus Jakarta Sans (isi, harga, tombol). Tidak menambah keluarga huruf lain.

### Teknologi
React 19 + TypeScript 5.9, React Router 7 + Vite 7 (web, prerender), Vite SPA (admin), Tailwind 4, Supabase (Postgres + RLS, Auth, Storage, Realtime), Leaflet/OSM, Recharts (grafik warna `grafik` #A3404C, lolos validasi dataviz). Monorepo pnpm: `apps/web`, `apps/admin`, `packages/ui`, `packages/shared`, `packages/data`, `supabase/`.
- Logika bisnis (harga, ongkir, stok, status, laporan) ada di fungsi SQL `supabase/migrations/*_fungsi.sql`. Frontend hanya menampilkan; jangan memindahkan perhitungan ke frontend.
- Error SQL berformat `"KODE: pesan untuk pengguna"`; `pesanGalat()` mengambil bagian pesan.
- Peran: `pemilik` (semua) dan `kasir` (pesanan, kasir, stok, pelanggan). Cek peran di SQL (`_wajib_admin`, `_wajib_pemilik`), bukan hanya di UI.
- Uji: `pnpm cek`, `pnpm uji:db` (Supabase lokal + `scripts/rute-tiruan.py`), `scripts/cek-*.mjs`, semuanya lewat `scripts/uji-semua.sh` (lihat skill `teras-cek`).
- Supabase menolak `DELETE`/`UPDATE` tanpa `WHERE` (safeupdate). Pakai `where true` jika memang semua baris.
- Web app (PWA): `public/manifest.webmanifest`, `public/sw.js`, `public/offline.html` di kedua aplikasi. Halaman diambil dari internet dulu; jangan menyimpan sementara data Supabase. Naikkan `VERSI` di `sw.js` jika strategi simpanan berubah.

### Standar
Mobile-first (360–430 px dulu), area sentuh ≥ 44 px, foto produk rasio 4:5, hormati `prefers-reduced-motion`, kontras teks ≥ 4,5:1.

### Skills proyek
Ada di `.claude/skills/` (lihat README di sana): `frontend-design` (anthropics/skills), `supabase` + `supabase-postgres-best-practices` (Supabase resmi), `web-perf` (Cloudflare resmi), `teras-cek` (build + semua uji), dan `teras-terbit` (pasang online). Pakai skill supabase/postgres sebelum mengubah migrasi database. Brief pemilik selalu menang atas saran umum `frontend-design`: cream, serif, maroon, label kapital, dan tagline bertitik tengah adalah identitas brand yang diminta.
