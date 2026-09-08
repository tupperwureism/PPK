# Ringkasan Proyek: Sistem Reservasi & Pelaporan Fasilitas Kampus (PPK 2026)

Dokumen ini merangkum seluruh pemahaman domain, kesepakatan arsitektur use case, serta artefak perancangan sistem yang telah diselesaikan untuk aktor **Pengguna** dalam proyek **PPK 2026 – Web Platform Sebelum UTS**.

---

## 1. Profil & Gambaran Umum Proyek

* **Nama Proyek:** Sistem Reservasi & Pelaporan Fasilitas Kampus
* **Mata Kuliah / Konteks:** Pengembangan Platform Khusus (PPK) 2026 (Sebelum UTS)
* **Tujuan Sistem:** Platform berbasis web untuk mengelola peminjaman fasilitas kampus (ruang kelas, aula, lab, lapangan, alat) serta pelaporan kerusakan fasilitas secara terpadu.
* **Arsitektur Teknis yang Diwajibkan:**
  * Pola struktur MVC: pemisahan `/public`, `/app (model/controller)`, `/views`, `/config`.
  * Validasi ganda: validasi form penting dilakukan di sisi *client* dan *server-side*.
  * Autentikasi: Registrasi mandiri, verifikasi admin, login, logout.
* **Aturan Bisnis Utama (Business Rules):**
  1. **Jam Operasional:** 07.00 – 20.00 WIB.
  2. **Slot Waktu Reservasi:** Durasi tetap kelipatan 30 menit (misal 07.00–07.30, 07.30–08.00).
  3. **Pencegahan Bentrok:** Validasi bentrok jadwal dilakukan ketat di sisi server.
  4. **Pembatalan Mandiri:** Pengguna hanya dapat membatalkan reservasinya sebelum batas toleransi waktu (*deadline rule*).
  5. **Pelaporan Kerusakan:** Melampirkan kategori, deskripsi detail, dan bukti foto kerusakan.

---

## 2. Struktur Aktor Sistem

1. **Pengunjung (Visitor - Tanpa Login):**
   * Mengakses katalog fasilitas publik, mencari/memfilter fasilitas, serta melihat jadwal ketersediaan per slot tanpa data identitas pemohon lain.
2. **Pengguna (Mahasiswa / Dosen / Staf - Login):**
   * Mendaftar dan login akun, mengajukan reservasi fasilitas, memantau riwayat & status reservasi, membatalkan reservasi mandiri, serta mengirim dan melacak laporan kerusakan.
3. **Petugas:**
   * Memproses antrean reservasi (setujui/tolak/batal mendesak) dan tiket laporan kerusakan, serta mengubah status ketersediaan fasilitas (misal: *Dalam Perbaikan*).
4. **Admin:**
   * Mengelola data master fasilitas, registrasi langsung akun petugas/pengguna, verifikasi akun registrasi mandiri, serta rekapitulasi okupansi/kerusakan fasilitas.

---

## 3. Cakupan Tugas Aktor Pengguna yang Telah Diselesaikan

Telah disepakati pembagian **10 Use Case** yang selaras 100% dengan granularitas bagian Pengunjung dari anggota tim:

| Kode UC | Nama Use Case | Cakupan / Penjelasan Singkat |
| :---: | :--- | :--- |
| **UC-01** | Melihat Daftar Fasilitas | Membuka katalog fasilitas default (nama, tipe, lokasi, kapasitas, deskripsi). |
| **UC-02** | Mencari Fasilitas | Memfilter daftar berdasarkan kata kunci, tipe, lokasi, atau kapasitas. |
| **UC-03** | Melihat Ketersediaan Fasilitas | Menampilkan matriks ketersediaan per slot 30 menit (07.00–20.00) tanpa identitas pemohon lain. |
| **UC-04** | Melakukan Registrasi Akun | Mendaftarkan akun mandiri (Nama, Email, Password terenkripsi, Kategori). |
| **UC-05** | Melakukan Login & Logout | Autentikasi kredensial, inisiasi sesi aktif, dan penghapusan sesi (*destroy*). |
| **UC-06** | Mengajukan Reservasi Fasilitas | Form booking waktu (kelipatan 30m, 07.00-20.00), tujuan peminjaman, validasi anti-bentrok. |
| **UC-07** | Melihat Riwayat & Status Reservasi | Daftar reservasi milik pengguna (`Pending`, `Approved`, `Rejected`, `Cancelled`) + modal detail. |
| **UC-08** | Membatalkan Reservasi | Pembatalan reservasi mandiri sebelum batas waktu toleransi dan pelepasan slot jadwal. |
| **UC-09** | Membuat Laporan Kerusakan | Formulir pelaporan, kategori, deskripsi masalah, dan unggah foto bukti. |
| **UC-10** | Melihat Status & Riwayat Laporan | Pelacakan progres tiket laporan (`Baru`, `Diproses`, `Selesai`, `Ditolak`) dan membaca catatan resolusi petugas. |

---

## 4. Struktur Berkas & Direktori Proyek

Semua artefak kerja telah disusun rapi di dalam direktori kerja:  
`C:\Users\chloud\.vscode\cli\MAINPROPERTY\UC And User Flow\`

```text
UC And User Flow/
├── ringkasan.md                              <- Dokumen ringkasan proyek ini
│
├── usecase/
│   ├── use_case_diagram_no_inheritance.puml  <- Diagram UC relasi langsung ke UC-01 s/d UC-10 (urutan rapi terikat constraint)
│   └── use_case_diagram.puml                 <- Diagram UC dengan relasi Generalization (Pengguna mewarisi Pengunjung)
│
├── activity_diagram/                         <- Activity Diagram Swimlane UML (Pengguna vs Sistem) dengan title
│   ├── ad_01_melihat_daftar_fasilitas.puml   <- Title: AD-01 : Melihat Daftar Fasilitas
│   ├── ad_02_mencari_fasilitas.puml          <- Title: AD-02 : Mencari Fasilitas
│   ├── ad_03_melihat_ketersediaan_fasilitas.puml <- Title: AD-03 : Melihat Ketersediaan Fasilitas
│   ├── ad_04_registrasi_akun.puml            <- Title: AD-04 : Melakukan Registrasi Akun
│   ├── ad_05_login_logout.puml               <- Title: AD-05 : Melakukan Login & Logout
│   ├── ad_06_mengajukan_reservasi.puml       <- Title: AD-06 : Mengajukan Reservasi Fasilitas
│   ├── ad_07_melihat_riwayat_reservasi.puml  <- Title: AD-07 : Melihat Riwayat & Status Reservasi
│   ├── ad_08_membatalkan_reservasi.puml      <- Title: AD-08 : Membatalkan Reservasi
│   ├── ad_09_membuat_laporan_kerusakan.puml  <- Title: AD-09 : Membuat Laporan Kerusakan
│   └── ad_10_melihat_status_laporan.puml     <- Title: AD-10 : Melihat Status & Riwayat Laporan
│
├── userflow/                                 <- Diagram alur layar pengguna (Task-based Grid Layout)
│   ├── flow_uc01_melihat_mencari_fasilitas.puml
│   ├── flow_uc02_registrasi_akun.puml
│   ├── flow_uc03_login_logout.puml
│   ├── flow_uc04_mengajukan_reservasi.puml
│   ├── flow_uc05_melihat_riwayat_reservasi.puml
│   ├── flow_uc06_membatalkan_reservasi.puml
│   ├── flow_uc07_membuat_laporan_kerusakan.puml
│   └── flow_uc08_melihat_status_laporan.puml
│
└── docs/
    ├── Skenario_Use_Case_Pengguna_PPK2026.docx <- File Word resmi berisi 10 tabel skenario lengkap
    └── use_case_scenarios.html                 <- Format HTML untuk salin-tempel langsung ke Google Docs tanpa error tabel
```

---

## 5. Keputusan Teknis & Solusi yang Telah Diterapkan

1. **Penataan Urutan Use Case Diagram:**
   * PlantUML dalam mode `left to right direction` secara bawaan menyusun urutan dari bawah ke atas jika tidak diatur. Masalah ini diselesaikan dengan menyematkan rantai relasi `UC_10 -[hidden] UC_09 ... UC_02 -[hidden] UC_01`, sehingga elips tersusun presisi dari atas ke bawah (UC-01 s/d UC-10).
2. **Kaidah Swimlane Activity Diagram:**
   * Di tingkat use case/fitur, swimlane dirancang **`Pengguna` vs `Sistem`**. Hal ini sesuai dengan konsep sistem web di mana interaksi antar aktor bersifat asinkron melalui database.
3. **Penyematan Judul Otomatis pada Activity Diagram:**
   * Setiap file activity diagram dilengkapi direktif `title AD-xx : [Nama Fitur]`, sehingga gambar hasil render langsung memuat judul tanpa perlu diedit manual.
4. **Catatan Koreksi & Audit Dokumen Kelompok (Temuan PDF 32 Halaman):**
   * **Halaman 2 (Pengunjung UC-03):** Judul tabel tertulis `Tabel UC-03: Melihat Ketersediaan Fasilitas`, namun pada baris pertama di dalam tabel tertulis `UC-02: Mencari Fasilitas`. Sudah diselaraskan dengan benar pada dokumentasi Pengguna kita.
   * **Halaman 25 (Admin UC-01):** Judul tabel di atas tertulis `Tabel UC-01: Mengubah Status Fasilitas Berdasarkan Laporan` (judul milik Petugas UC-05), padahal isinya adalah pendaftaran akun petugas (`UC-01: Mendaftarkan Akun Petugas`).
   * **Halaman 21 vs 24 (Petugas UC-05):** Pada skenario (Hal 21, *Exceptional path*) terdapat aturan konfirmasi jika fasilitas diubah jadi 'Dalam Perbaikan' saat masih ada reservasi aktif di masa mendatang. Namun pada Activity Diagram (Hal 24), percabangan ini belum digambarkan.
   * **Halaman 7 vs 8 & 27 (Registrasi vs Verifikasi):** Skenario registrasi Pengguna (Hal 7) awalnya belum menyebutkan status verifikasi admin, padahal Login dan Admin mensyaratkannya. Sudah disempurnakan pada `AD-04`, `use_case_scenarios.html`, dan `flow_uc02` dengan status eksplisit `'Menunggu Verifikasi Admin'`.

---

## 6. Rekomendasi Langkah Selanjutnya (Roadmap Tingkat Proyek)

Saat melanjutkan ke tahap berikutnya, tahapan logis pengembangannya adalah:
1. **Sequence Diagram (SD):**
   * Memodelkan interaksi teknis antar komponen MVC (`View` $\rightarrow$ `Controller` $\rightarrow$ `Model` $\rightarrow$ `Database`).
   * Khususnya untuk alur krusial: validasi bentrok jadwal reservasi (*conflict detection query*), validasi batas waktu pembatalan, dan proses upload file laporan kerusakan.
2. **Perancangan Basis Data (ERD & Skema Relasional):**
   * Tabel `Users` (id, nama, email, password, role, status_verifikasi).
   * Tabel `Facilities` (id, nama_fasilitas, tipe, lokasi, kapasitas, deskripsi, status_fasilitas).
   * Tabel `Reservations` (id, user_id, facility_id, tanggal, start_time, end_time, tujuan, status_reservasi, alasan_batal).
   * Tabel `Reports` (id, user_id, facility_id, kategori, deskripsi, foto_path, status_laporan, catatan_resolusi).
3. **Implementasi Kode & Setup Struktur MVC:**
   * Pengaturan routing di `/public/index.php`.
   * Logika validasi sisi server di Controller.
   * Template view berbasis komponen modular.
