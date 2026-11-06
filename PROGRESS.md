# PROGRESS — Teras Bakaran

**Rencana diringkas (disetujui pemilik, 5 Nov 2026):** fase 4–9 digabung menjadi 3 tahap. Tidak ada fitur brief yang dibuang.

| Tahap | Isi | Status |
|---|---|---|
| Fase 1–3 | Rancangan, design system, beranda & katalog | ✅ Selesai |
| **A. Online dulu** | Website saat ini (beranda, menu, keranjang, pesan via WA) online di Cloudflare Pages | 🔧 Repo siap — menunggu pemilik menghubungkan Cloudflare |
| **B. Pesan online + dashboard** | Fase 4 checkout, 5 database/backend, 6 dashboard admin, 7 integrasi | ✅ Selesai & teruji di Supabase lokal — menunggu pemilik memasang Supabase online (docs/PANDUAN-PASANG.md) |
| **C. Penyempurnaan** | Fase 8 testing & optimasi + fitur prioritas 2 (HPP, stok bahan, PWA, dll.) | ⏳ |

<details><summary>Tabel fase lama (sebelum diringkas)</summary>

| Fase | Status |
|---|---|
| 1. Analisis kebutuhan & arsitektur | ✅ |
| 2. Design system | ✅ |
| 3. Homepage & katalog | ✅ |
| 4. Keranjang & checkout | → Tahap B |
| 5. Database & backend | → Tahap B |
| 6. Dashboard admin | → Tahap B |
| 7. Integrasi sistem | → Tahap B |
| 8. Testing & optimasi | → Tahap C |
| 9. Deployment | → Tahap A (+ diperbarui tiap tahap) |

</details>

---|---|
| 1. Analisis kebutuhan & arsitektur | ✅ Selesai — disetujui pemilik |
| 2. Design system | ✅ Selesai — disetujui pemilik |
| 3. Homepage & katalog | ✅ Selesai — menunggu tinjauan pemilik |
| 4. Keranjang & checkout | ⏳ Belum dimulai |
| 5. Database & backend | ⏳ Belum dimulai |
| 6. Dashboard admin | ⏳ Belum dimulai |
| 7. Integrasi sistem | ⏳ Belum dimulai |
| 8. Testing & optimasi | ⏳ Belum dimulai |
| 9. Deployment | ⏳ Belum dimulai |

---

## Fase 1 — Analisis & arsitektur (5 Nov 2026)

**Hasil:**
- `docs/FASE-1-RANCANGAN.md`: analisis kebutuhan, sitemap pelanggan & admin, arah visual, design system (warna + uji kontras, huruf, skala, jarak), arsitektur teknis, rancangan database, alur pesanan, rencana fase, dan 16 pertanyaan untuk pemilik.
- Papan visual (halaman artifact) berisi contoh hero desktop & HP, kartu menu, keranjang, nota, dan timeline tracking.
- `CLAUDE.md`: aturan kerja proyek.

**Pengujian:** Kontras semua pasangan warna brand dihitung dengan rumus WCAG (lihat tabel di dokumen rancangan).

**Pembaruan: logo diterima.**
- File asli disimpan di `brand/logo/logo-asli.jpg`.
- Dibuat turunannya: versi transparan lengkap & ringkas (cream, maroon, espresso), ikon api, dan favicon. Desain logo tidak diubah.
- Papan visual diperbarui dengan bagian "Logo", dan logo dipasang di header desktop, header HP, dan footer.
- ~~Menunggu keputusan pemilik soal logo~~ → diserahkan ke Claude: maroon #641F2B, versi ringkas & ikon api dipakai. File asli resolusi tinggi tidak ada (cukup untuk web).

**Pembaruan: putaran 2 (jawaban pemilik).**
- Poster menu disimpan di `docs/referensi/menu-poster.jpg`, lalu disalin ke `docs/data/MENU.md` (7 kategori, 21 produk + Bakaran per tusuk).
- `docs/FASE-1-KEPUTUSAN.md` berisi: nomor WA, usulan ongkir, alur QRIS manual → otomatis, arsitektur Rp0, dan 24 fitur tambahan berprioritas.
- Papan visual diperbarui: kategori baru, contoh kartu & nota memakai menu asli, dan tabel ongkir.
- Menunggu: link Google Maps, jam buka, media sosial, gambar QRIS, isi menu Bakaran, persetujuan ongkir & fitur.

**Pembaruan: putaran 3 (data lengkap dari pemilik).**
- Diterima: link Google Maps, gambar QRIS (`brand/pembayaran/qris-teras-bakaran.jpg`), TikTok @terasbakarand6, jam buka setiap hari 15.00–22.00, dan tarif ongkir dari pemilik (<3 km Rp5.000, 3–<8 km Rp8.000, ≥8 km Rp12.000).
- Menu Bakaran diisi sendiri oleh pemilik lewat dashboard.
- Koordinat toko belum bisa dibaca (Google Maps diblokir di lingkungan kerja) → diminta ke pemilik / diatur lewat dashboard.
- Menunggu: persetujuan rancangan untuk memulai Fase 2.

**Belum ada kode aplikasi.** Fase 2 dimulai setelah pemilik menyetujui rancangan dan menjawab pertanyaan bertanda ★.

---

## Fase 2 — Design system (5 Nov 2026)

**Keputusan pemilik sebelum mulai (putaran 4):** ongkir <3 km Rp5.000 · 3–<8 km Rp8.000 · 8–10 km Rp12.000 · >10–15 km ditentukan admin via WA · >15 km tidak dilayani; minimal belanja antar Rp15.000; ambil sendiri gratis; koordinat toko diisi sendiri di dashboard; rancangan disetujui.

**Hasil:**
- Kerangka monorepo (pnpm): `apps/web`, `packages/ui`, `packages/shared`.
- `packages/ui`: token desain di `styles.css` (warna brand saja, palet Tailwind bawaan dimatikan), huruf Fraunces + Plus Jakarta Sans di-host sendiri, dan 20 komponen (lihat `packages/ui/README.md`).
- `packages/shared`: format rupiah/jarak, link WhatsApp, info toko yang sudah dikonfirmasi, 7 kategori, 6 status pesanan, dan status bayar yang jujur.
- `apps/web`: React Router 7, halaman disiapkan sebagai HTML statis (siap Cloudflare Pages). Halaman `/desain` berisi katalog komponen yang interaktif, dan `/` berisi beranda sementara (tombol WhatsApp + lokasi).
- Logo versi web (WebP) dan favicon dari logo resmi.

**Versi teknologi:** React 19.2, React Router 7.18, Vite 7.3, Tailwind 4.3, TypeScript 5.9. Sengaja memakai seri yang sudah stabil, bukan versi besar terbaru (React Router 8, Vite 8, TS 7).

**Pengujian:**
- 16 unit test lolos: format rupiah/jarak/WA, perhitungan subtotal + ongkir di nota, batas penghitung jumlah, kartu habis tidak bisa ditambah, filter kategori, error form, pilihan kartu, dan keranjang.
- `scripts/cek-desain.mjs`: 27 pemeriksaan browser lolos di 360/414/768/1440 px. Isinya: tidak ada geser ke samping, semua tombol ≥ 36 px (area sentuh utama 44 px), **0 pelanggaran WCAG AA (axe)**, 0 error console, interaksi keranjang, detail, Esc, dan filter, serta mode "kurangi gerakan".
- Masalah yang ditemukan & diperbaiki saat uji:
  - Animasi muncul menyembunyikan konten di HTML awal → konten sekarang selalu terlihat tanpa JS atau dengan "kurangi gerakan".
  - Garis tepi tombol outline hilang.
  - Pilihan "Antar ke alamat" terjepit di layar 360 px.

**Catatan:** label "Tersedia" tidak ditampilkan di setiap kartu agar katalog tidak ramai. Hanya menu **Habis** yang diberi label.

**Berikutnya, Fase 3:** beranda lengkap (hero, menu pilihan, Kenapa Teras Bakaran, Tentang, Lokasi, Kontak) dan halaman Menu + detail produk.

---

## Fase 3 — Beranda & katalog (5 Nov 2026)

**Hasil:**
- **Kerangka situs:**
  - Header menempel di atas: cream, lalu berubah espresso saat digulir. Berisi logo, menu, status Buka/Tutup otomatis (WIB), keranjang, dan tombol Pesan.
  - Navigasi bawah khusus HP: Beranda, Menu, Keranjang, Kontak.
  - Footer espresso, tombol WhatsApp mengambang (desktop), tautan "langsung ke isi" untuk pengguna keyboard, dan transisi halus antar halaman.
- **Beranda (`/`):**
  - Hero sesuai brief dengan animasi pembuka. Sisi kanan memakai logo di panel maroon sampai foto asli ada.
  - Pita tagline, 7 kartu kategori (jumlah & harga terendah dihitung otomatis dari data), menu pilihan, Kenapa Teras Bakaran, Tentang Kami, Lokasi (tombol Google Maps), dan Kontak (2 nomor WA + TikTok).
  - Data terstruktur "Restaurant" untuk Google.
- **Menu (`/menu`):**
  - Filter 7 kategori (alamat ikut berubah sehingga bisa dibagikan), pencarian, dan tampilan per kategori.
  - Kategori Bakaran menampilkan info harga + tombol "Tanya bakaran" via WA sampai pemilik mengisi menunya.
- **Detail (`/menu/nama-menu`):** lembar detail dengan alamat sendiri untuk tiap 21 menu, sudah disiapkan sebagai HTML (bisa dibaca Google). Berisi pilihan **level pedas 1–4 (wajib)** untuk Mie Jebeu, jumlah, catatan khusus, dan harga yang ikut berubah.
- **Keranjang:**
  - Tambah, ubah jumlah, hapus, dan subtotal. Tersimpan di HP, jadi tidak hilang saat dimuat ulang.
  - Tombol **"Pesan lewat WhatsApp"** mengirim rincian pesanan ke nomor utama. Sudah berfungsi sebelum checkout online ada di Fase 4.
- `packages/shared`: data 21 menu dari poster, status buka/tutup (WIB), logika keranjang, dan pesan WhatsApp.

**Pengujian:**
- 30 unit test lolos (19 baru):
  - harga sama dengan poster, tidak ada foto/deskripsi karangan, tidak ada nama "Gacoan"
  - jam buka 14.59/15.00/21.59/22.00 WIB
  - keranjang: gabung item, level berbeda = baris berbeda, batas 1–99, data rusak diabaikan
  - isi pesan WA
- `scripts/cek-situs.mjs`: **79 pemeriksaan browser lolos, 3 putaran berturut-turut**.
  - Beranda, menu, dan detail di 360/414/768/1440 px: tidak geser ke samping, tombol ≥ 40 px, 0 pelanggaran WCAG AA, 0 error.
  - Alur pelanggan lengkap: tambah dari beranda, level wajib, filter, pencarian, keranjang, pesan WA, simpan setelah dimuat ulang, header berubah warna, alamat langsung ke detail, dan mode kurangi gerakan.
- `scripts/cek-desain.mjs` (Fase 2) tetap lolos 27/27.
- **Bug yang ditemukan & diperbaiki:**
  - Klik chip kategori lalu langsung mengetik di pencarian bisa membuat kategori lama ikut terbawa. Filter sekarang disimpan di halaman, dan URL hanya salinannya.
  - Judul kecil footer memakai huruf serif.
  - Panel hero terlalu tinggi di HP.
  - Pita tagline mepet di layar 360 px.

**Keputusan desain sementara (perlu konfirmasi, lihat `docs/FASE-1-KEPUTUSAN.md` bagian 8):** teks Tentang Kami hanya berisi fakta yang ada, peta tertanam menunggu koordinat, level pedas harga sama, dan Mix 2 Rasa memakai catatan.

**Berikutnya, Fase 4:** checkout (nama, WA, alamat, pin lokasi, antar/ambil), ongkir otomatis sesuai tarif, minimal belanja, QRIS manual + unggah bukti, dan ringkasan nota sebelum konfirmasi.

---

## Persiapan Tahap A — Online (5 Nov 2026)

**Hasil:**
- Skill proyek dipasang di `.claude/skills/`: `frontend-design` dari anthropics/skills (sudah dibaca, aman) dan `teras-cek` (langkah build + semua uji). Skill `webapp-testing` tidak dipasang, karena butuh Playwright Python yang tidak tersedia dan uji Node proyek ini sudah lebih lengkap.
- Plugin Supabase & Cloudflare disarankan ke pemilik, tetapi **belum terpasang**. Tahap A memakai integrasi Git Cloudflare Pages (tidak butuh plugin).
- Branch `main` (berisi README awal dari pemilik) digabungkan ke branch kerja. Daftar fitur pemilik dipindah ke `README.md`, dan `README.MD` dihapus untuk mencegah bentrok nama di Windows/Mac.
- Siap Cloudflare Pages:
  - `.node-version` (22), `apps/web/public/_headers` (keamanan + cache aset 1 tahun), `robots.txt`
  - halaman 404 sungguhan dengan header/footer, plus `menu/404.html` untuk link menu yang salah ketik
  - petunjuk pengaturan build di `README.md`
- Halaman detail menu tidak lagi bergantung pada data server (data menu sudah ikut terkirim), sehingga link menu yang salah tidak menimbulkan error.

**Pengujian:**
- `pnpm install --frozen-lockfile` + build dari nol berhasil (sama seperti di Cloudflare).
- `scripts/server-statis.py` meniru Cloudflare Pages (404.html terdekat).
- **81/81** pemeriksaan situs lolos (2 uji 404 baru), 27/27 katalog, 30/30 unit test.
- **Bug yang ditemukan & diperbaiki:** alamat yang tidak ada memicu error hidrasi React (#418), dan `/menu/menu-salah` menampilkan "Terjadi kendala". Sekarang keduanya tampil benar, berstatus 404, tanpa error.

---

## Tahap B — Pesan online + dashboard (5 Nov 2026)

**Keputusan pemilik (putaran 5):**
- akun 1 pemilik + 1 kasir
- pilihan produk (mis. Mix 2 Rasa) diatur sendiri di dashboard
- level pedas harga sama
- foto & teks diatur dari dashboard
- notifikasi diserahkan ke Claude
- dikerjakan tanpa berhenti

**Hasil:**

- **Database (Supabase, `supabase/migrations/`, 4 migrasi):**
  - 14 tabel dengan Row Level Security.
  - Logika di SQL: `buat_pesanan` (harga & ongkir dihitung ulang di server, stok, minimal belanja, jam buka, tutup sementara, batas 5 pesanan/10 menit/nomor), `cek_ongkir` (tarif bertingkat + "ditentukan admin" + di luar jangkauan), `lacak_pesanan` (kode + token rahasia), bukti QRIS (folder rahasia per token), `ubah_status_pesanan`, `tetapkan_ongkir`, `verifikasi_pembayaran`, `buat_pesanan_kasir`, `atur_ketersediaan`, `ringkasan_operasional`, `ringkasan_keuangan`.
  - Penyimpanan foto publik & bukti rahasia, realtime pesanan.
  - Data awal dari poster + tarif pemilik. Email pemilik awal otomatis menjadi pemilik.
- **`packages/data`:** koneksi Supabase, fungsi publik & admin, tipe, label status bersama.
- **Website pelanggan:**
  - Menu, harga, foto, jam, teks, dan kontak dibaca langsung dari database, dengan data bawaan sebagai cadangan.
  - **Checkout:** antar/ambil, data diri diingat, peta pin + "Pakai lokasi saya", ongkir langsung tampil, alamat & patokan, COD/QRIS, nota, validasi jelas.
  - **Lacak** (`/lacak`): timeline otomatis tiap 15 detik, QRIS + salin nominal + unggah bukti (foto dikecilkan), dan "Kirim ke WhatsApp toko".
  - **Pesanan saya** (`/pesanan`).
  - Pilihan ganda per grup, sisa stok, dan menu baru dari dashboard tampil tanpa build ulang.
- **Dashboard admin (`apps/admin`, terpisah):**
  - **Akun & akses:** masuk/daftar + persetujuan akun; peran pemilik/kasir (dicek di database, bukan hanya di tampilan).
  - **Dashboard:** kondisi saat ini + penjualan, keuntungan, grafik, terlaris, metode bayar.
  - **Pesanan:** muncul seketika + bunyi + notifikasi browser + judul tab berkedip; tombol status satu ketukan; detail dengan ongkir manual, verifikasi QRIS, batal beserta alasan, "Kabari pelanggan" (WA terisi otomatis + link lacak), dan cetak nota 58 mm.
  - **Operasional:** kasir dengan kembalian; menu (foto, HPP, grup pilihan, arsip); kategori; stok; pelanggan.
  - **Keuangan:** transaksi + pengeluaran; HPP + margin; laporan + unduh CSV/Excel.
  - **Pengaturan:** info toko, lokasi (GPS / tempel koordinat / peta), jam, tutup sementara, foto hero, QRIS, metode, tarif ongkir, akun.
- **Otomatisasi gratis (`.github/workflows`):** cek kode setiap push, menjaga Supabase tetap aktif (harian), dan cadangan database terenkripsi (harian, 30 hari). Dua terakhir aktif setelah secret diisi.
- **Panduan pemilik:** `docs/PANDUAN-PASANG.md` (pemasangan + pemakaian harian).
- **Skill proyek `teras-cek`** dan `scripts/uji-semua.sh` diperbarui.

**Pengujian (satu putaran bersih, `scripts/uji-semua.sh`):**
- 32 unit test.
- **32 uji database** terhadap Supabase lokal (Docker): akses tiap peran, ongkir di setiap batas km (2,9 / 3 / 7,99 / 8 / 10 / 10,01 / 15 / 15,01), harga tidak bisa diakali, stok & pembatalan, QRIS, kasir, laporan, akun.
- **81** situs, **27** katalog, **24** alur pesan online, **52** dashboard + integrasi. Mencakup 4 ukuran layar, 0 pelanggaran WCAG AA, dan 0 error console.

**Bug yang ditemukan uji & diperbaiki:**
1. **Celah keamanan:** pemeriksaan "apakah pemilik" menghasilkan NULL untuk tamu, sehingga fungsi laporan keuangan bisa dipanggil tanpa login (datanya kosong). Diperbaiki dengan `coalesce(…, false)` dan diuji.
2. Kasir mencoba mengubah tarif ongkir: tidak ada perubahan, tetapi juga tidak ada pesan. Sekarang muncul pesan "Hanya pemilik…".
3. Kolom unggah bukti tanpa label (aksesibilitas).
4. Halaman Kasir di HP: menu terbawah tertutup bar keranjang.
5. Label sumbu grafik terpotong.

**Belum diverifikasi (di luar jangkauan lingkungan kerja):**
- Supabase online & Cloudflare: lingkungan kerja tidak bisa menjangkau keduanya. Sudah diuji dengan Supabase lokal versi sama.
- Ubin peta OpenStreetMap & peta Google tertanam: diblokir di lingkungan uji, jadi diganti gambar kosong saat uji.
- Perangkat HP sungguhan.

**Catatan & batasan yang perlu diketahui pemilik:**
- Jarak = garis lurus × 1,3 (perkiraan jalan, gratis). Faktornya bisa diubah di dashboard.
- QRIS statis diverifikasi manual. QRIS otomatis butuh pendaftaran gateway.
- Email bawaan Supabase dibatasi beberapa email/jam. Cukup untuk 2 akun admin.
- Halaman yang disiapkan saat build sesaat menampilkan data bawaan sebelum data terbaru dimuat.
- Notifikasi saat dashboard tertutup → Tahap C.

**Berikutnya:**
- Pemilik mengikuti `docs/PANDUAN-PASANG.md` (Supabase + 2 proyek Cloudflare).
- Tahap C: notifikasi HP saat dashboard tertutup (web push), PWA, HPP berbasis resep & stok bahan, perbaikan dari pemakaian nyata.

---

## Tambahan setelah Tahap B — Ongkir rute jalan, skill, web app (5 Nov 2026)

**Permintaan pemilik:** jarak ongkir berdasarkan rute jalan (bukan garis lurus), pasang skill, dan jadikan website sebagai web app yang bisa dijalankan.

**Hasil:**
- **Skill resmi dipasang** (dibaca dulu, isinya hanya panduan): `supabase` dan `supabase-postgres-best-practices` (Supabase), serta `web-perf` (Cloudflare). `wrangler` tidak dipasang karena tidak cocok dengan cara proyek ini diluncurkan.
  - Plugin Supabase/Cloudflare (dengan MCP) **belum dipasang pemilik**, sehingga peluncuran online tetap butuh langkah pemilik.
- **Ongkir rute jalan** (migrasi 5):
  - Database memanggil OpenRouteService → cadangan OSRM lewat ekstensi `http`. Kalau gagal, ongkir menjadi "dikonfirmasi admin".
  - Hasil rute disimpan sementara per titik ±11 m selama 30 hari, dan dibuang saat titik toko dipindah.
  - Batas 30 rute baru per menit melindungi kuota.
  - Titik yang garis lurusnya sudah > 15 km langsung ditolak tanpa memanggil layanan.
  - Kunci API disimpan di skema `pribadi` (tidak terbuka lewat API) dan diatur pemilik dari **Pengaturan → Pesanan & ongkir → Rute jalan**.
  - Sesuai skill Postgres, rute dihitung **sebelum** stok menu dikunci, agar transaksi tetap singkat.
  - Kolom faktor garis lurus dihapus.
- **Kebijakan RLS dirapikan** (migrasi 6) sesuai `supabase db advisors`: sekarang **0 temuan** (sebelumnya 7 saran kinerja, tanpa temuan keamanan).
- **Web app (PWA)** untuk website & dashboard:
  - manifest, ikon (maroon untuk pelanggan, gelap untuk dashboard, versi maskable Android), service worker, dan halaman offline
  - tombol "Pasang aplikasi" (Android) dan petunjuk untuk iPhone

**Pengujian (`scripts/uji-semua.sh`, satu putaran bersih):**
- 32 unit test.
- **41 uji database** (9 baru untuk rute):
  - batas tarif memakai jarak rute
  - garis lurus 2,5 km tetapi rute 3,13 km → Rp8.000
  - cadangan OSRM, tanpa kunci, semua layanan gagal, simpanan sementara, batas per menit, terlalu jauh tanpa memanggil layanan
  - kunci hanya untuk pemilik dan tidak pernah dikirim balik
  - simpanan dibuang saat titik toko dipindah
- Browser: 81 situs, 27 katalog, 24 alur pesan, 52 dashboard, dan **16 web app**. Chrome menyatakan kedua aplikasi **layak dipasang**, dan perilaku offline benar.
- Layanan rute diganti `scripts/rute-tiruan.py` (lingkungan uji tidak punya internet ke OpenRouteService/OSRM).

**Bug yang ditemukan uji & diperbaiki:** Supabase menolak `DELETE` tanpa `WHERE` (safeupdate). Pengosongan simpanan rute akan gagal di Supabase online, jadi diperbaiki dengan `where true`.

**Belum diverifikasi:** panggilan sungguhan ke OpenRouteService/OSRM (diblokir di lingkungan kerja; format respons mengikuti dokumentasi resmi dan diuji dengan server tiruan), dan pemasangan di HP sungguhan.

**Agar bisa dijalankan pelanggan:** pemilik mengikuti `docs/PANDUAN-PASANG.md` (Supabase + 2 proyek Cloudflare + kunci OpenRouteService), atau mengizinkan Claude mengakses Supabase/Cloudflare (domain jaringan + token di pengaturan lingkungan, atau memasang plugin Supabase & Cloudflare).

## Pemasangan online (6 Nov 2026)

**Database Supabase: SUDAH ONLINE.**
- Organisasi "Teras Bakaran" dan proyek `teras-bakaran` (ref `xgaufhfjqiramugdkgxn`, region Singapura) dibuat lewat API akun pemilik.
- 6 migrasi terpasang berurutan dan dicatat di riwayat migrasi, jadi `supabase db push` berikutnya hanya memasang migrasi baru.
- Isi awal: 7 kategori, 21 menu. Site URL login = `https://teras-bakaran-admin.pages.dev`.
- Password database dibuat acak dan tidak ditampilkan. Pemilik bisa me-reset-nya di Supabase → Database settings.
- Advisors → Security: 34 peringatan jenis "fungsi SECURITY DEFINER bisa dipanggil anon/authenticated" (aturan pemeriksaan baru di Supabase). Ini memang rancangannya: fungsi pelanggan (buat_pesanan, lacak_pesanan, cek_ongkir) harus bisa dipanggil, dan semua fungsi admin/pemilik sudah memeriksa peran di dalam (`_wajib_admin` / `_wajib_pemilik`). Tidak ada tabel yang terbuka.
- Build dengan URL + publishable key asli lulus `pnpm cek`.

**Status lain:**
- Email pemilik `doniwae2003@gmail.com` sudah terdaftar di `public.pemilik_awal` (dikonfirmasi pemilik).
- Cloudflare Pages (menunggu token api.cloudflare.com).
