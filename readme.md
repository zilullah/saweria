# Hiyuki Saweria Overlay

Custom donation alert terinspirasi Hiyuki dari Wuthering Waves, dengan arah visual burning sakura through snow. Struktur mengikuti [AvuxDemons/SaweriaOverlay](https://github.com/AvuxDemons/SaweriaOverlay/blob/master/readme.md).

File produksi:

- `template/hiyuki/index.html`
- `template/hiyuki/style.css`

Tidak membutuhkan framework, backend, JavaScript, font eksternal, atau build. Dekorasi menggunakan CSS tanpa artwork resmi.

## Installation

1. Buka [Saweria Overlay](https://saweria.co/overlays), pilih Alert, lalu Custom.
2. Salin seluruh `template/hiyuki/index.html` ke kolom HTML.
3. Salin seluruh `template/hiyuki/style.css` ke kolom CSS.
4. Set Durasi Notifikasi ke `10000` ms. Jika durasinya berbeda, ubah `--alert-duration` dalam CSS ke nilai yang sama.
5. Simpan Tampilan dan jalankan test alert.
6. Gunakan URL overlay Saweria sebagai OBS Browser Source. Minimum `1000 × 600`, rekomendasi `1920 × 1080`.
7. Setelah perubahan, refresh Browser Source. Gunakan Refresh cache of current page bila tampilan lama masih muncul.

CSS tidak dapat membaca durasi Saweria atau mendeteksi event berikutnya. Animasi dimulai saat elemen dibuat. Replay pada donation berikutnya perlu diverifikasi di Saweria; mengganti teks pada elemen yang sama tidak otomatis mengulang animasi CSS.

## Theme Preview

Hiyuki memakai frame charcoal dengan sudut terpotong, garis frost, aksen crimson, dan bunga sakura abstrak. Amount paling dominan, diikuti donator, message, lalu media.

Preview browser lokal belum diverifikasi. Screenshot yang dibuat oleh pemeriksaan lokal memakai data contoh dari `TESTING.md`, bukan bukti donation nyata atau hasil Saweria/OBS.

## Configuration

Properti yang dapat diubah berada di awal `style.css`:

| Properti | Fungsi |
| --- | --- |
| `--amount` | Warna nominal |
| `--donator` | Warna nama donator |
| `--text` | Warna pesan |
| `--hiyuki-*` | Palet dari `DESIGN.md` |
| `--alert-width` | Lebar pilihan, tetap dibatasi `80vw` |
| `--alert-top` | Jarak alert dari atas canvas |
| `--alert-duration` | Total durasi, harus sama dengan pengaturan Saweria |
| `--exit-duration` | Lama transisi keluar |

Token tetap `{amount}`, `{donator}`, `{message}`, dan `{media}`. Jangan mengganti token dengan data contoh pada file produksi.

Nama panjang menggunakan ellipsis. Pesan dibatasi tiga baris sesuai `DESIGN.md`; teks lengkap tetap berada di DOM. Amount panjang dapat membungkus agar tidak keluar frame. Pesan kosong tidak menyisakan jarak pesan.

## Media

Tema mengikuti contoh Azusa upstream: `{media}` berada pada atribut `src` gambar. Gambar tampil setelah pesan, maksimal `120 × 120` px, dengan `object-fit: contain` agar rasio asli terjaga.

Media kosong atau token yang belum diganti disembunyikan. Gambar tanpa ukuran tetap tidak memesan kotak sebelum termuat. `alt` kosong menghindari teks gambar rusak yang mengganggu informasi donation. Perilaku URL rusak tetap perlu diuji di browser dan Saweria.

Dukungan ini untuk media gambar, bukan asumsi bahwa Saweria menyisipkan HTML, video, atau gift event. Jika output Saweria berbeda, verifikasi format resminya sebelum mengubah markup.

## Animation

Animasi dasar ada dalam `style.css`; tidak ada modul tambahan yang perlu disalin.

| Fase | Waktu default |
| --- | --- |
| Sakura dan ember masuk | Mulai `0` ms |
| Frost dan crimson sweep | `150-400` ms |
| Frame muncul | `250-700` ms |
| Amount dan donator muncul | `400-800` ms |
| Message dan media muncul | `600-950` ms |
| Hold tanpa gerakan berulang | `950-9400` ms |
| Pulse, petal keluar, fade | `9400-10000` ms |

Reduced motion menampilkan informasi langsung, menonaktifkan partikel bergerak, dan mempertahankan fade pendek pada akhir durasi. Tidak ada animasi infinite.

## Design decisions

- Charcoal solid di belakang teks menjaga kontras tanpa bergantung pada gameplay. Canvas di luar frame tetap transparan.
- Sudut asimetris dan garis frost membentuk geometri tajam tanpa pola HUD atau neon RGB.
- Crimson hanya pada donor dan garis energi; sakura serta ember tetap kecil agar nominal dominan.
- Segoe UI dengan fallback Arial menjaga pesan mudah dibaca tanpa request font eksternal.
- Kelopak membentuk sakura kecil di atas nominal. Gerakan hanya pada entry dan exit agar hold tenang.
- Padding memisahkan simbol, nominal, dan pesan; media berada di bawahnya agar tidak menutupi teks.
- ENERGY 2 / RHYTHM 1 / MOTION 2: satu komposisi terpusat dengan transisi singkat, bukan loop dekoratif.

## Development and testing

Baca `AGENTS.md`, `DESIGN.md`, `SPEC.md`, `ARCHITECTURE.md`, `PRD.md`, dan `TESTING.md`. Checklist yang ada berada di `TASK.md`.

File produksi tidak membutuhkan instalasi dependency. `tests/hiyuki.cjs` adalah pemeriksaan pengembangan opsional berbasis Node dan Playwright Core yang sudah tersedia di lingkungan pengembang, bukan build step.

```sh
node tests/hiyuki.cjs
```

Jika Playwright Core berada di luar project, arahkan `PLAYWRIGHT_CORE` ke instalasi tersebut. Browser Chromium yang cocok juga harus tersedia. Contoh PowerShell:

```powershell
$env:PLAYWRIGHT_CORE = 'C:/path/to/node_modules/playwright-core'
node tests/hiyuki.cjs
```

Pemeriksaan mencakup token, dependency, kontras, ukuran viewport, konten panjang, media gambar lokal, entry/hold/exit, reduced motion, dan resize teks. Pemeriksaan menyimpan screenshot data contoh ke `template/hiyuki/assets/preview/hiyuki-preview.png` setelah lulus.

Pemeriksaan browser lokal tidak menggantikan Saweria atau OBS. Lihat status verifikasi dan pengujian manual di `TESTING.md`.
