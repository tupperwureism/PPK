# ADR 0001: Pemilihan Stack Laravel dan Laragon untuk Platform PPK 2026

* **Status:** Accepted
* **Tanggal:** 2026-09-08
* **Pembuat:** Tim Pengembang PPK 2026
* **Konteks:** Mata Kuliah Pengembangan Platform Khusus (PPK) 2026 - Web Platform Sebelum UTS

---

## 1. Konteks & Permasalahan

Ketentuan tugas proyek PPK 2026 menetapkan persyaratan teknis berikut:
1. Struktur kode wajib memisahkan minimal koneksi DB, tampilan (HTML), dan logika proses (arsitektur MVC).
2. Minimal terdapat pembagian direktori: /public, /app (model/controller), /views, /config.
3. Validasi data wajib dilakukan ganda: di sisi client dan sisi server untuk form-form penting.
4. Fitur otentikasi wajib mencakup registrasi mandiri, verifikasi admin, login, dan logout.
5. Menangani aturan bentrok jadwal ketat dan penanganan upload file bukti laporan kerusakan.

## 2. Keputusan Arsitektur

Kami memutuskan untuk mengadopsi stack berikut:
* **Framework Backend/Fullstack:** Laravel 11.x (PHP 8.2+)
* **Lingkungan Pengembangan Lokal:** Laragon (Windows) dengan Apache/Nginx, MySQL 8.0, dan PHP 8.2+
* **Database:** MySQL
* **Frontend View:** Laravel Blade Template dengan integrasi CSS Tailwind / Bootstrap untuk antarmuka responsif dan mudah digunakan.

## 3. Kesesuaian dengan Ketentuan Dosen

| Persyaratan Dosen | Implementasi Laravel |
| :--- | :--- |
| Struktur folder /public | Bawaan Laravel: public/index.php sebagai web document root |
| Struktur folder /app | Bawaan Laravel: pp/Models/ dan pp/Http/Controllers/ |
| Struktur folder /views | Bawaan Laravel: 
esources/views/ (didukung symlink / alias) |
| Struktur folder /config | Bawaan Laravel: direktori config/ dan file .env |
| Validasi Sisi Server | Laravel FormRequest class & validasi custom rule anti-bentrok |
| Validasi Sisi Client | HTML5 constraint validation & JavaScript inline |
| Upload Gambar Bukti | Laravel Storage::disk('public') dan symlink public/storage |
| Keamanan Autentikasi | Bcrypt password hashing, session management, CSRF protection bawaan |

## 4. Konsekuensi & Keuntungan

* **Keuntungan:**
  * Mengeliminasi *boilerplate* manual untuk router, koneksi PDO, session handling, dan migration schema.
  * Mempermudah kolaborasi Git dengan file migration dan database seeder dummy.
  * Menyederhanakan demonstrasi demo sistem dan pengemasan file zip untuk pengumpulan via Kulon.
* **Risiko & Mitigasi:**
  * Pastikan tim memahami konfigurasi virtual host Laragon (misal http://reservasi.test) atau menjalankan php artisan serve.
