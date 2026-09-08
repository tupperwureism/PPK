# PRD: Sistem Reservasi & Pelaporan Fasilitas Kampus

* **Dokumen:** Product Requirements Document (PRD) / Spesifikasi Sistem
* **Proyek:** Pengembangan Platform Khusus (PPK) 2026 – Web Platform Sebelum UTS
* **Target Pengumpulan:** Maksimal 11 Oktober 2026 pukul 12.00 WIB via Kulon
* **Stack:** Laravel & Laragon (MySQL)

---

## 1. Problem Statement

Pengelolaan fasilitas kampus (ruang kelas, aula, laboratorium, alat, dan lapangan) sering menghadapi kendala pencatatan jadwal bentrok (*double-booking*), ketiadaan informasi ketersediaan publik secara transparan, serta lambatnya respons terhadap sarana kampus yang rusak atau tidak layak pakai. Mahasiswa, dosen, dan staf membutuhkan satu portal terpadu untuk mengecek ketersediaan dan mengajukan reservasi secara mandiri, sekaligus melaporkan kerusakan fasilitas dengan bukti nyata agar petugas dan administrator kampus dapat mengelola operasional secara efisien dan akuntabel.

---

## 2. Solusi Produk

Membangun platform aplikasi web terpadu dengan arsitektur MVC (Laravel) yang memfasilitasi 4 aktor (*Pengunjung*, *Pengguna*, *Petugas*, dan *Admin*). Sistem menerapkan:
1. **Jadwal Transparan & Terstruktur:** Kalender ketersediaan per slot 30 menit (07.00–20.00 WIB) yang dapat diakses publik tanpa membocorkan identitas peminjam lain.
2. **Validasi Anti-Bentrok Ketat:** Logika server-side yang secara deterministik menolak persetujuan atau pengajuan peminjaman pada slot dan fasilitas yang bertabrakan.
3. **Pelaporan Insiden Terintegrasi:** Formulir pengaduan kerusakan terstandarisasi dengan unggah bukti foto, di mana penanganan laporan dapat langsung mengubah status ketersediaan fasilitas (*Dalam Perbaikan*).
4. **Alur Kerja Terpusat Petugas & Admin:** Dashboard antrean persetujuan/penolakan, pembatalan darurat dengan alasan resmi, verifikasi pendaftaran akun mandiri, serta ekspor rekapitulasi okupansi dan kerusakan fasilitas.

---

## 3. Matriks 17 User Story & Acceptance Criteria

### A. Pengunjung (Visitor - Tanpa Login)

#### User Story 1: Melihat Daftar Fasilitas & Ketersediaan Per Slot
* **Sebagai** Pengunjung atau Pengguna,
* **Saya ingin** melihat daftar fasilitas beserta status ketersediaannya per slot waktu (tersedia/tidak tersedia),
* **Sehingga** saya tahu jadwal kosong tanpa melihat detail identitas pemohon atau tujuan penggunaan pihak lain.
* **Acceptance Criteria:**
  * Menampilkan katalog fasilitas publik (nama, tipe, lokasi, kapasitas, deskripsi).
  * Matriks jadwal harian dibagi dalam slot tetap 30 menit mulai pukul 07.00 hingga 20.00 WIB.
  * Status slot hanya menampilkan indikator `Tersedia` atau `Tidak Tersedia`.
  * Detail nama pemesan, kontak, dan tujuan acara dirahasiakan dari tampilan publik.

#### User Story 2: Mencari & Memfilter Fasilitas
* **Sebagai** Pengunjung atau Pengguna,
* **Saya ingin** mencari fasilitas berdasarkan tipe, lokasi, atau kapasitas,
* **Sehingga** saya dapat menemukan ruangan/peralatan yang sesuai kebutuhan dengan cepat.
* **Acceptance Criteria:**
  * Tersedia form pencarian berbasis kata kunci (nama fasilitas).
  * Tersedia filter dropdown berdasarkan: Tipe Fasilitas (Ruang Kelas, Lab, Aula, Lapangan, Alat), Lokasi/Gedung, dan Minimal Kapasitas.
  * Hasil filter diperbarui secara dinamis atau via submission form.

---

### B. Pengguna (User - Mahasiswa/Dosen/Staf Login)

#### User Story 3: Mengajukan Reservasi Fasilitas
* **Sebagai** Pengguna (login),
* **Saya ingin** mengajukan reservasi fasilitas pada rentang waktu tertentu dengan mencantumkan tujuan penggunaan,
* **Sehingga** permohonan saya dapat diproses oleh petugas kampus.
* **Acceptance Criteria:**
  * Wajib memilih tanggal, `start_time`, dan `end_time` dalam jam operasional (07.00–20.00 WIB).
  * Jam mulai dan selesai wajib merupakan kelipatan slot 30 menit.
  * Wajib mengisi deskripsi tujuan peminjaman.
  * Validasi server-side memastikan tidak ada tumpang tindih dengan reservasi yang sudah disetujui (*Approved*).
  * Status awal reservasi setelah diajukan adalah `Pending`.

#### User Story 4: Membatalkan Reservasi Mandiri
* **Sebagai** Pengguna,
* **Saya ingin** membatalkan reservasi saya sendiri sebelum batas waktu toleransi tertentu,
* **Sehingga** slot waktu tersebut dapat dilepaskan untuk peminjam lain jika rencana saya berubah.
* **Acceptance Criteria:**
  * Tombol 'Batalkan' hanya aktif jika reservasi berstatus `Pending` atau `Approved`.
  * Sistem memvalidasi bahwa waktu pembatalan dilakukan minimal *Toleransi Waktu* (default: 2 jam) sebelum `start_time`.
  * Setelah dibatalkan, status berubah menjadi `Cancelled` dan slot waktu kembali terbuka (`Tersedia`).

#### User Story 5: Melihat Riwayat & Status Lengkap Reservasi
* **Sebagai** Pengguna,
* **Saya ingin** melihat riwayat dan status reservasi saya beserta detail lengkapnya,
* **Sehingga** saya dapat memantau apakah pengajuan saya disetujui, ditolak, atau dibatalkan.
* **Acceptance Criteria:**
  * Halaman daftar riwayat menampilkan kode reservasi, nama fasilitas, tanggal, jam, dan badge status (`Pending`, `Approved`, `Rejected`, `Cancelled`).
  * Modal atau halaman detail menampilkan rincian alasan penolakan/pembatalan jika ada.

#### User Story 6: Melaporkan Kerusakan Fasilitas
* **Sebagai** Pengguna,
* **Saya ingin** melaporkan kerusakan/masalah pada fasilitas tertentu dengan memilih kategori, deskripsi, dan melampirkan foto bukti,
* **Sehingga** pengelola fasilitas segera mengambil tindakan perbaikan.
* **Acceptance Criteria:**
  * Form laporan memuat pilihan fasilitas terkait, kategori kerusakan (Kelistrikan, Kebocoran/Struktur, Komputer/Lab, Sanitasi, Perangkat/Alat), dan textarea deskripsi detail.
  * Wajib mengunggah file foto bukti (validasi: JPG/PNG, maks 2MB) baik di client maupun server-side.
  * Status tiket laporan awal adalah `Baru`.

#### User Story 7: Melihat Status & Riwayat Laporan Kerusakan
* **Sebagai** Pengguna,
* **Saya ingin** melihat daftar laporan kerusakan yang pernah saya buat beserta status terkininya,
* **Sehingga** saya mengetahui perkembangan tindak lanjut dari laporan saya.
* **Acceptance Criteria:**
  * Menampilkan daftar tiket pengaduan dengan status: `Baru`, `Diproses`, `Selesai`, atau `Ditolak`.
  * Menampilkan catatan resolusi dari petugas jika laporan sudah ditandai `Selesai` atau `Ditolak`.

---

### C. Petugas (Officer)

#### User Story 8: Dashboard Antrean Reservasi & Laporan
* **Sebagai** Petugas,
* **Saya ingin** melihat dashboard/antrean reservasi dan laporan kerusakan yang masih menunggu diproses,
* **Sehingga** tidak ada pengajuan peminjaman atau aduan fasilitas yang terlewat.
* **Acceptance Criteria:**
  * Tampilan ringkasan metrik: Jumlah reservasi pending, jumlah laporan baru, jumlah fasilitas dalam perbaikan.
  * Tabel antrean reservasi masuk terurut berdasarkan tanggal pengajuan terlama (*FIFO*).

#### User Story 9: Menyetujui atau Menolak Reservasi (Anti-Bentrok Otomatis)
* **Sebagai** Petugas,
* **Saya ingin** menyetujui atau menolak reservasi yang masuk secara manual dengan jaminan sistem mencegah persetujuan jadwal bentrok,
* **Sehingga** penggunaan ruangan tertib dan tidak terjadi perselisihan pemakaian.
* **Acceptance Criteria:**
  * Tombol 'Setujui' dan 'Tolak' tersedia pada setiap reservasi berstatus `Pending`.
  * Jika tombol 'Setujui' ditekan, sistem menjalankan query pengecekan bentrok jadwal di server.
  * Jika ditemukan jadwal bentrok yang sudah disetujui sebelumnya, aksi persetujuan dibatalkan dan sistem memunculkan pesan error jelas.
  * Saat menolak reservasi, petugas dapat menyematkan catatan/alasan penolakan.

#### User Story 10: Pembatalan Reservasi Darurat oleh Petugas
* **Sebagai** Petugas,
* **Saya ingin** membatalkan reservasi yang sudah disetujui dalam kondisi mendesak dengan mencantumkan alasan pembatalan,
* **Sehingga** pengguna mendapat kejelasan informasi ketika fasilitas mendadak tidak dapat digunakan (misal: perbaikan darurat atau kerusakan fasilitas).
* **Acceptance Criteria:**
  * Petugas dapat melakukan aksi 'Batal Darurat' pada reservasi yang berstatus `Approved`.
  * Wajib mengisi formulir input 'Alasan Pembatalan Darurat'.
  * Status reservasi berubah menjadi `Cancelled` dan notifikasi/alasan tersimpan di riwayat pengguna.

#### User Story 11: Memperbarui Status & Resolusi Laporan Kerusakan
* **Sebagai** Petugas,
* **Saya ingin** mengubah status laporan (baru/diproses/selesai/ditolak) beserta catatan resolusi saat laporan ditutup,
* **Sehingga** riwayat penyelesaian tercatat rapi dan transparan bagi pelapor.
* **Acceptance Criteria:**
  * Petugas dapat mengubah status tiket: `Baru` -> `Diproses` -> `Selesai` / `Ditolak`.
  * Saat menandai status `Selesai` atau `Ditolak`, kolom 'Catatan Resolusi' wajib diisi.

#### User Story 12: Menandai Status Fasilitas 'Dalam Perbaikan'
* **Sebagai** Petugas,
* **Saya ingin** menandai fasilitas berstatus 'Dalam Perbaikan' terkait laporan kerusakan dan mengembalikannya ke status aktif setelah selesai,
* **Sehingga** sistem mencegah adanya reservasi baru pada fasilitas yang sedang rusak.
* **Acceptance Criteria:**
  * Tombol toggle/aksi status ketersediaan pada fasilitas: `Aktif` <-> `Dalam Perbaikan`.
  * Fasilitas yang berstatus `Dalam Perbaikan` tidak dapat dipilih pada form reservasi dan bertanda khusus di katalog publik.

---

### D. Administrator (Admin)

#### User Story 13: Mendaftarkan Akun Petugas Langsung
* **Sebagai** Admin,
* **Saya ingin** mendaftarkan akun petugas secara langsung,
* **Sehingga** hak akses operasional petugas tetap terkontrol ketat (petugas tidak melakukan registrasi mandiri dalam kondisi apa pun).
* **Acceptance Criteria:**
  * Form pembuatan petugas hanya dapat diakses oleh Admin.
  * Input: Nama, Email, Password, NIP/Identitas Staf.
  * Role otomatis diset sebagai `officer`.

#### User Story 14: Mendaftarkan Akun Pengguna Langsung
* **Sebagai** Admin,
* **Saya ingin** mendaftarkan akun pengguna (mahasiswa/dosen/staf) secara langsung tanpa melalui form registrasi mandiri,
* **Sehingga** mempermudah onboarding akun khusus atau pengguna yang membutuhkan penanganan manual.
* **Acceptance Criteria:**
  * Form pendaftaran pengguna oleh admin dengan pilihan peran / kategori pengguna.

#### User Story 15: Verifikasi Akun Pengguna Hasil Registrasi Mandiri
* **Sebagai** Admin,
* **Saya ingin** memverifikasi atau menolak akun pengguna hasil registrasi mandiri sebelum akun tersebut dapat digunakan untuk login,
* **Sehingga** mencegah pendaftaran akun palsu atau penyalahgunaan platform kampus.
* **Acceptance Criteria:**
  * Tabel antrean akun pendaftar mandiri dengan status `Belum Diverifikasi`.
  * Admin dapat meninjau data pendaftar dan menekan 'Setujui Verifikasi' atau 'Tolak'.
  * Pengguna yang belum diverifikasi tidak dapat melakukan login (pesan: Akun menunggu persetujuan admin).

#### User Story 16: Mengelola Data Master Fasilitas (CRUD)
* **Sebagai** Admin,
* **Saya ingin** mengelola data fasilitas (tambah, edit, nonaktifkan),
* **Sehingga** direktori fasilitas kampus selalu akurat sesuai kondisi fisik terkini.
* **Acceptance Criteria:**
  * Form input fasilitas: Nama Fasilitas, Tipe, Lokasi/Gedung, Kapasitas, Deskripsi, Foto Fasilitas.
  * Fitur 'Nonaktifkan' (*Soft Delete*) untuk menyembunyikan fasilitas dari katalog tanpa merusak relasi data riwayat reservasi lama.

#### User Story 17: Rekapitulasi & Ekspor Laporan Okupansi dan Kerusakan
* **Sebagai** Admin,
* **Saya ingin** melihat dan mengekspor rekapitulasi okupansi fasilitas dan frekuensi kerusakan per fasilitas/lokasi (format CSV/Excel/PDF),
* **Sehingga** pimpinan kampus memiliki data evaluasi pemanfaatan sarana dan perawatan infrastruktur.
* **Acceptance Criteria:**
  * Filter rentang tanggal laporan rekap.
  * Ringkasan: Tingkat keterpakaian fasilitas (okupansi) dan jumlah insiden kerusakan.
  * Tombol ekspor file ke format CSV atau PDF yang dapat diunduh.

---

## 4. Persyaratan Teknis & Batasan Arsitektural

1. **Pola MVC Laravel:**
   * Model: `User`, `Facility`, `Reservation`, `Report`.
   * Controller: `FacilityController`, `ReservationController`, `ReportController`, `AdminController`, `OfficerController`, `AuthController`.
   * Request Validator: `StoreReservationRequest`, `StoreReportRequest`, `RegisterUserRequest`.
2. **Validasi Ganda (Client & Server):**
   * Client-side: Atribut HTML5 (`required`, `type`, `accept="image/*"`), Javascript/AlpineJS untuk pembatasan pilihan slot 30 menit.
   * Server-side: Laravel Validator rules (`numeric`, `date_format:H:i`, `mimes:jpeg,png,jpg|max:2048`, closure custom validation untuk anti-bentrok).
3. **Penyimpanan Berkas (Uploads):**
   * Direktori: `storage/app/public/reports/` dengan symlink ke `public/storage/reports/`.

---

## 5. Kebutuhan Non-Fungsional & Rubrik UTS PPK 2026

1. **Ketentuan File Word Laporan UTS:**
   * Memuat: Nama & NIM anggota tim, pembagian tugas jelas, link Google Drive (source code, SQL dump, dokumentasi), panduan setting environment, kredensial login akun demo tiap aktor, dan tangkapan layar fitur antarmuka.
2. **Alur Presentasi UTS (10 Menit Presentasi + 10-15 Menit Tanya Jawab):**
   * Slide & materi mencakup: Latar belakang, demonstrasi fitur utama 4 aktor, arsitektur sistem, dan kendala/solusi yang dihadapi.
