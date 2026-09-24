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

| Properti           | Fungsi                                             |
| ------------------ | -------------------------------------------------- |
| `--amount`         | Warna nominal                                      |
| `--donator`        | Warna nama donator                                 |
| `--text`           | Warna pesan                                        |
| `--hiyuki-*`       | Palet dari `DESIGN.md`                             |
| `--alert-width`    | Lebar pilihan, tetap dibatasi `80vw`               |
| `--alert-top`      | Jarak alert dari atas canvas                       |
| `--alert-duration` | Total durasi, harus sama dengan pengaturan Saweria |
| `--exit-duration`  | Lama transisi keluar                               |

Token tetap `{amount}`, `{donator}`, `{message}`, dan `{media}`. Jangan mengganti token dengan data contoh pada file produksi.

Nama panjang menggunakan ellipsis. Pesan dibatasi tiga baris sesuai `DESIGN.md`; teks lengkap tetap berada di DOM. Amount panjang dapat membungkus agar tidak keluar frame. Pesan kosong tidak menyisakan jarak pesan.

## Media

Tema mengikuti contoh Azusa upstream: `{media}` berada pada atribut `src` gambar. Gambar tampil setelah pesan, maksimal `120 × 120` px, dengan `object-fit: contain` agar rasio asli terjaga.

Media kosong atau token yang belum diganti disembunyikan. Gambar tanpa ukuran tetap tidak memesan kotak sebelum termuat. `alt` kosong menghindari teks gambar rusak yang mengganggu informasi donation. Perilaku URL rusak tetap perlu diuji di browser dan Saweria.

Dukungan ini untuk media gambar, bukan asumsi bahwa Saweria menyisipkan HTML, video, atau gift event. Jika output Saweria berbeda, verifikasi format resminya sebelum mengubah markup.

## Animation

Animasi dasar ada dalam `style.css`; tidak ada modul tambahan yang perlu disalin.

| Fase                        | Waktu default   |
| --------------------------- | --------------- |
| Sakura dan ember masuk      | Mulai `0` ms    |
| Frost dan crimson sweep     | `150-400` ms    |
| Frame muncul                | `250-700` ms    |
| Amount dan donator muncul   | `400-800` ms    |
| Message dan media muncul    | `600-950` ms    |
| Hold tanpa gerakan berulang | `950-9400` ms   |
| Pulse, petal keluar, fade   | `9400-10000` ms |

Reduced motion menampilkan informasi langsung, menonaktifkan partikel bergerak, dan mempertahankan fade pendek pada akhir durasi. Tidak ada animasi infinite.
