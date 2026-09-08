# Buku Log Utang Teknis & Desain (Technical Debt Ledger)

* **Proyek:** Sistem Reservasi & Pelaporan Fasilitas Kampus (PPK 2026)
* **Dokumen Acuan:** PDF System Design Kelompok (32 Halaman)
* **Kebijakan Pengelolaan:** Berkas asli rekan tim sengaja tidak diubah sepihak, melainkan dicatat di sini sebagai *Technical Debt* terkelola untuk ditindaklanjuti saat rapat konsolidasi tim atau fase perancangan data/logika.

---

## Daftar Utang Teknis Terbuka (Active Debt Items)

### 1. [TD-01] Typo Penamaan Baris Use Case Pengunjung
* **Aktor Penanggung Jawab:** Rekan Tim (Pengunjung)
* **Lokasi Berkas:** PDF Halaman 2, Tabel UC-03
* **Tingkat Keparahan (Severity):** Rendah (Low - Redaksional)
* **Status:** `Deferred (Belum Diubah)`
* **Deskripsi Temuan:**
  * Judul tabel tertulis: `Tabel UC-03: Melihat Ketersediaan Fasilitas`.
  * Namun isi baris pertama tabel tertulis: `Use Case | UC-02: Mencari Fasilitas`.
* **Dampak:** Ambiguitas pembacaan dokumen bagi penguji/dosen saat meninjau tabel skenario Pengunjung.
* **Jalur Remediasi (Upgrade Path):** Ubah teks baris menjadi `UC-03: Melihat Ketersediaan Fasilitas` saat penyatuan naskah laporan Word final.
* **Trigger Penanganan:** Fase finalisasi laporan Word UTS sebelum 11 Oktober 2026.

---

### 2. [TD-02] Typo Judul Tabel Skenario Admin
* **Aktor Penanggung Jawab:** Rekan Tim (Admin)
* **Lokasi Berkas:** PDF Halaman 25, Tabel UC-01
* **Tingkat Keparahan (Severity):** Rendah (Low - Redaksional)
* **Status:** `Deferred (Belum Diubah)`
* **Deskripsi Temuan:**
  * Judul tabel di atas tertulis: `Tabel UC-01: Mengubah Status Fasilitas Berdasarkan Laporan` (judul salinan dari Petugas UC-05).
  * Namun isi skenario di dalamnya adalah pendaftaran petugas: `Use Case | UC-01: Mendaftarkan Akun Petugas`.
* **Dampak:** Inkonsistensi antara judul tabel dan isi Use Case Admin.
* **Jalur Remediasi (Upgrade Path):** Ganti judul tabel menjadi `Tabel UC-01: Mendaftarkan Akun Petugas`.
* **Trigger Penanganan:** Fase penyusunan naskah kompilasi Word kelompok.

---

### 3. [TD-03] Ketiadaan Percabangan Pengecekan Reservasi Aktif pada Activity Diagram Petugas
* **Aktor Penanggung Jawab:** Rekan Tim (Petugas)
* **Lokasi Berkas:** PDF Halaman 21 (Skenario UC-05) vs Halaman 24 (Activity Diagram UC-05)
* **Tingkat Keparahan (Severity):** Sedang - Tinggi (Medium-High - Celah Logika Bisnis)
* **Status:** `Deferred (Belum Diubah)`
* **Deskripsi Temuan:**
  * Pada **Skenario (Halaman 21, Exceptional path)**, tercatat aturan bisnis:
    *"Jika saat mengubah status menjadi 'Dalam Perbaikan' ternyata masih terdapat jadwal reservasi aktif di masa mendatang pada fasilitas tersebut, sistem menampilkan pesan konfirmasi/peringatan: 'Perhatian: Terdapat reservasi aktif pada fasilitas ini yang mungkin perlu dibatalkan terlebih dahulu.'"*
  * Namun pada **Activity Diagram (Halaman 24)**, percabangan logika ini **hilang/tidak digambarkan sama sekali**. Alur langsung melompat dari pemilihan status ke penguncian kalender.
* **Dampak Jika Dibiarkan:**
  * Pengembang frontend/backend petugas bisa melewatkan pengecekan reservasi bentrok di masa depan ketika sebuah ruangan tiba-tiba ditutup untuk perbaikan darurat.
  * Berpotensi menimbulkan komplain dari pengguna yang reservasinya tidak tertangani saat fasilitas mendadak diperbaiki.
* **Jalur Remediasi (Upgrade Path):**
  1. Tambahkan *decision diamond* di Activity Diagram Petugas Halaman 24: `Ada reservasi aktif di masa depan? (Ya/Tidak)`.
  2. Jika ya, tampilkan dialog peringatan/konfirmasi pembatalan darurat (terintegrasi dengan Petugas UC-03).
* **Trigger Penanganan:** Sebelum perancangan Class Diagram dan implementasi Controller fasilitas petugas.

---

## Log Riwayat Remediasi (Closed Debts)

* `[TD-04]` Penyelarasan status verifikasi admin pada Registrasi Pengguna (Skenario UC-04, AD-04, dan Flow UC-02) -> **Resolved pada 2026-09-08** di direktori lokal Pengguna.
