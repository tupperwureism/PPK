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
* **Lokasi Berkas:** PDF Halaman 26, Tabel UC-01
* **Tingkat Keparahan (Severity):** Rendah (Low - Redaksional)
* **Status:** `Deferred (Belum Diubah)`
* **Deskripsi Temuan:**
  * Judul tabel di atas tertulis: `Tabel UC-01: Mengubah Status Fasilitas Berdasarkan Laporan` (judul salinan dari Petugas UC-05).
  * Namun isi skenario di dalamnya adalah pendaftaran petugas: `Use Case | UC-01: Mendaftarkan Akun Petugas`.
* **Dampak:** Inkonsistensi antara judul tabel dan isi Use Case Admin.
* **Jalur Remediasi (Upgrade Path):** Ganti judul tabel menjadi `Tabel UC-01: Mendaftarkan Akun Petugas`.
* **Trigger Penanganan:** Fase penyusunan naskah kompilasi Word kelompok.

---

## Log Riwayat Remediasi (Closed Debts)

* `[TD-03]` Ketiadaan Percabangan Pengecekan Reservasi Aktif pada Activity Diagram Petugas -> **Closed / Resolved pada 2026-09-08**:
  * **Verifikasi:** Pada diagram *Activity Diagram: Mengubah Status Fasilitas* (PDF Halaman 25), alur percabangan (*decision diamond*) `Cek jadwal reservasi aktif di masa depan` beserta alur `Tampilkan dialog peringatan reservasi aktif` -> `Konfirmasi pembatalan reservasi terdampak` -> `Batalkan reservasi aktif terdampak` sudah tergambar secara lengkap, benar, dan selaras 100% dengan skenario UC-05 di Halaman 21.
* `[TD-04]` Penyelarasan status verifikasi admin pada Registrasi Pengguna (Skenario UC-04, AD-04, dan Flow UC-02) -> **Resolved pada 2026-09-08**:
  * **Verifikasi:** Diperbarui di direktori lokal Pengguna dan terkonfirmasi telah tercermin pada dokumen PDF kelompok (Skenario UC-04 Halaman 6–7 dan AD-04 Halaman 13).
