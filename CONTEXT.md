# Context: Sistem Reservasi & Pelaporan Fasilitas Kampus (PPK 2026)

Dokumen ini mendefinisikan domain model, *Ubiquitous Language* (kamus istilah resmi), aturan bisnis global, dan mesin status (*state machines*) untuk platform web **Sistem Reservasi & Pelaporan Fasilitas Kampus** berbasis **Laravel & Laragon**.

---

## 1. Ubiquitous Language (Kamus Istilah Domain)

Setiap pengembang, dokumen spesifikasi, antarmuka, dan kode sumber wajib menggunakan istilah resmi berikut secara konsisten:

| Istilah Domain | Padanan Kode | Definisi & Batasan |
| :--- | :--- | :--- |
| **Pengunjung** | Visitor | Aktor publik tanpa login yang hanya dapat melihat daftar fasilitas dan matriks ketersediaan jadwal tanpa identitas pemesan lain. |
| **Pengguna** | User (Role: user) | Mahasiswa, Dosen, atau Staf terotentikasi yang berhak memesan fasilitas dan melaporkan kerusakan. |
| **Petugas** | Officer (Role: officer) | Staf operasional kampus yang memproses persetujuan reservasi, pembatalan darurat, dan menangani tiket laporan kerusakan. Akun dibuat langsung oleh Admin (tanpa registrasi mandiri). |
| **Admin** | Admin (Role: dmin) | Pengelola data master fasilitas, verifikator pendaftaran akun mandiri, pembuat akun petugas/pengguna langsung, dan pengelola ekspor rekapitulasi. |
| **Fasilitas** | Facility | Entitas sarana/prasarana kampus (ruang kelas, aula, laboratorium, lapangan, peralatan). Memiliki kapasitas, lokasi, tipe, dan status ketersediaan. |
| **Slot Waktu** | TimeSlot | Satuan waktu reservasi dengan durasi tetap kelipatan **30 menit** dalam rentang jam operasional **07.00 – 20.00 WIB**. |
| **Reservasi** | Reservation | Permohonan peminjaman fasilitas oleh Pengguna untuk rentang slot waktu tertentu dengan tujuan penggunaan tertentu. |
| **Bentrok Jadwal** | ScheduleConflict | Kondisi tumpang tindih waktu pada fasilitas yang sama. Wajib divalidasi dan dicegah secara mutlak di sisi server (*server-side*). |
| **Batas Pembatalan** | CancellationDeadline | Aturan toleransi batas waktu pembatalan mandiri oleh Pengguna sebelum waktu mulai reservasi. |
| **Laporan Kerusakan** | Report / IncidentReport | Tiket pengaduan kerusakan/masalah fasilitas kampus yang memuat kategori, deskripsi detail, dan bukti foto. |
| **Catatan Resolusi** | ResolutionNote | Catatan penjelasan dari Petugas saat menyelesaikan atau menolak tiket laporan kerusakan. |
| **Dalam Perbaikan** | UnderMaintenance | Status fasilitas yang sedang tidak dapat dipinjam karena perbaikan terkait laporan kerusakan. |

---

## 2. Aturan Bisnis Global (Core Business Rules)

1. **Jam Operasional Fasilitas:**
   * Sistem hanya menerima reservasi pada rentang waktu **07.00 s/d 20.00 WIB**.
   * Pemesanan di luar jam ini wajib ditolak oleh validasi server.
2. **Kaidah Slot Waktu (Granularitas 30 Menit):**
   * Durasi reservasi wajib kelipatan 30 menit (contoh: 07.00–07.30, 07.30–09.00).
   * Nilai start_time dan end_time tidak boleh bernilai acak (misal 07.15 dilarang).
3. **Pencegahan Bentrok Jadwal (Anti-Collision Server-Side):**
   * Validasi bentrok dilakukan di level server (FormRequest / database query) sebelum insert/update status reservasi.
   * Dua reservasi disetujui (*Approved*) tidak boleh memiliki irisan waktu:
     StartBaru < EndAda DAN EndBaru > StartAda.
4. **Privasi Jadwal Pengunjung:**
   * Pada tampilan publik/pengunjung, sistem hanya menampilkan status Tersedia atau Tidak Tersedia.
   * Informasi nama pemesan, kontak, dan tujuan peminjaman **tidak boleh** bocor ke publik.
5. **Kebijakan Pembatalan:**
   * **Pengguna:** Hanya dapat membatalkan reservasi mandiri jika masih berstatus Pending atau Approved sebelum batas *deadline* (misal minimal 2 jam sebelum start_time).
   * **Petugas:** Dapat membatalkan reservasi Approved secara sepihak dalam kondisi darurat (*emergency cancel*), namun wajib menyertakan alasan pembatalan tertulis.
6. **Integritas Pelaporan Kerusakan:**
   * Setiap laporan kerusakan wajib melampirkan foto bukti (format image valid: JPG, JPEG, PNG, maks 2MB).
   * Petugas yang menangani laporan berwenang mengubah status fasilitas menjadi Dalam Perbaikan, yang secara otomatis memblokir reservasi baru pada fasilitas tersebut.

---

## 3. Mesin Status Siklus Hidup (Lifecycle State Machines)

### A. Status Reservasi (ReservationStatus)
- Pending -> Approved (oleh Petugas)
- Pending -> Rejected (oleh Petugas)
- Pending -> Cancelled (oleh Pengguna)
- Approved -> Cancelled (oleh Pengguna sebelum deadline, atau oleh Petugas darurat dengan alasan)

### B. Status Laporan Kerusakan (ReportStatus)
- Baru -> Diproses (oleh Petugas)
- Diproses -> Selesai (oleh Petugas dengan Catatan Resolusi)
- Diproses -> Ditolak (oleh Petugas dengan Alasan)
- Baru -> Ditolak (oleh Petugas)

### C. Status Ketersediaan Fasilitas (FacilityStatus)
- Aktif <-> Dalam Perbaikan (terkait tiket kerusakan)
- Aktif <-> Nonaktif (diatur Admin)
