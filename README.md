# Teras Bakaran

Website pemesanan dan dashboard admin untuk **Teras Bakaran**: *Kopi · Makan · Ngemil · Nongkrong*.

## Fitur utama (dari pemilik)

- Website pelanggan
- Katalog menu
- Keranjang belanja
- Checkout pesanan
- Dashboard admin
- Manajemen stok
- Laporan penjualan

- **Panduan pemasangan & pemakaian (untuk pemilik): [`docs/PANDUAN-PASANG.md`](docs/PANDUAN-PASANG.md)**
- **Pasang dari PC (untuk programmer): [`docs/PANDUAN-PC.md`](docs/PANDUAN-PC.md)**
- Rancangan & keputusan: [`docs/FASE-1-RANCANGAN.md`](docs/FASE-1-RANCANGAN.md), [`docs/FASE-1-KEPUTUSAN.md`](docs/FASE-1-KEPUTUSAN.md)
- Data menu: [`docs/data/MENU.md`](docs/data/MENU.md)
- Kemajuan per fase: [`PROGRESS.md`](PROGRESS.md)
- Aturan kerja proyek: [`CLAUDE.md`](CLAUDE.md)

## Struktur

```
apps/web          Website pelanggan (React Router 7, prerender → HTML statis; data langsung dari Supabase)
apps/admin        Dashboard admin (React + Vite, SPA terpisah)
packages/ui       Design system: token warna/huruf + komponen (termasuk peta & pengecil foto)
packages/shared   Logika bersama: format rupiah, menu bawaan, keranjang, jam buka
packages/data     Koneksi Supabase: fungsi publik & admin, tipe data, label status; uji database
supabase/         Migrasi database (skema + RLS, logika pesanan, penyimpanan, data awal)
brand/            Logo resmi & QRIS dari pemilik
scripts/          Uji otomatis di browser & pembantu uji lokal
.github/workflows Cek kode, jaga Supabase aktif, cadangan database terenkripsi
```

## Menjalankan (untuk programmer)

Butuh Node 22+ dan pnpm 10.

```bash
pnpm install
```

```bash
pnpm dev
```

Lalu buka http://localhost:5173/desain untuk melihat katalog design system.

```bash
pnpm cek
```

`pnpm cek` menjalankan pemeriksaan tipe, unit test, dan build sekaligus.

### Supabase lokal (untuk uji database & alur pesan)

Butuh Docker. Image diambil dari Docker Hub:

```bash
SUPABASE_INTERNAL_IMAGE_REGISTRY=docker.io npx supabase@2 start -x studio,imgproxy,vector,logflare,supavisor,edge-runtime,mailpit
```

```bash
npx supabase@2 db reset
```

```bash
pnpm uji:db
```

`pnpm uji:db` menjalankan 32 uji database: keamanan per peran, ongkir, stok, QRIS, kasir, dan laporan.

Uji browser (tampilan 4 ukuran layar, aksesibilitas, interaksi) dijalankan setelah build:

```bash
python3 scripts/server-statis.py apps/web/build/client 4173
```

```bash
node scripts/cek-desain.mjs http://localhost:4173 ./hasil-cek
```

```bash
node scripts/cek-situs.mjs http://localhost:4173 ./hasil-cek
```

Uji alur pesan & dashboard butuh Supabase lokal, web di-build dengan `VITE_SUPABASE_*` lokal, dan dashboard disajikan dengan `--spa` di port 4174:

```bash
node scripts/cek-pesan.mjs http://localhost:4173 ./hasil-cek
```

```bash
node scripts/cek-admin.mjs http://localhost:4174 http://localhost:4173 ./hasil-cek
```

## Online (Cloudflare Pages) — website pelanggan

Website dihubungkan langsung ke repo GitHub ini. Setiap push ke branch produksi otomatis membangun ulang website.

| Pengaturan | Nilai |
|---|---|
| Framework preset | None |
| Build command | `pnpm install --frozen-lockfile && pnpm --filter @teras/web build` |
| Build output directory | `apps/web/build/client` |
| Root directory | (kosongkan) |
| Environment variable | `NODE_VERSION` = `22` |

Dashboard admin adalah proyek Cloudflare Pages kedua; langkahnya ada di `docs/PANDUAN-PASANG.md`.
Untuk pesan online, tambahkan `VITE_SUPABASE_URL` & `VITE_SUPABASE_PUBLISHABLE_KEY`.

File pendukung: `apps/web/public/_headers` (keamanan & cache), `apps/web/public/robots.txt`, dan `404.html` yang dibuat otomatis saat build.
