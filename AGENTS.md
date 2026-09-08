# Pedoman & Aturan Perilaku Proyek (Sistem Reservasi & Pelaporan Fasilitas Kampus)

Dokumen ini adalah aturan wajib (*Workspace Rules*) yang harus dipatuhi oleh AI Agent dalam menangani analisis, pemodelan, dan pengembangan pada proyek ini.

---

## 1. Disiplin SDLC Bertahap (Anti-Jumping Mandate)
* **Dilarang Terburu-buru:** Jalankan proses perancangan sistem langkah demi langkah (*step-by-step*) secara bersih, terukur, dan disiplin.
* **Urutan Tahapan Analisis & Desain:**
  1. *Fase 1: Kebutuhan & Konteks:* Use Case Diagram, Skenario Use Case, dan Kamus Domain (CONTEXT.md).
  2. *Fase 2: Pemodelan Proses & Alur Antarmuka:* Activity Diagram swimlane dan User Flow layar.
  3. *Fase 3: Pemodelan Struktural & Data:* Class Diagram, Data Dictionary, dan ERD (Konseptual & Fisik).
  4. *Fase 4: Pemodelan Interaksi Teknis:* Sequence Diagram (hanya disusun setelah Class Diagram dan ERD selesai).
  5. *Fase 5: Implementasi Kode & Pengujian.*
* Jangan pernah menyusun diagram interaksi (Sequence Diagram) atau menulis kode sebelum tahapan struktural di atasnya disetujui.

---

## 2. Batas Tanggung Jawab Tim & Protokol Technical Debt
* **Pemisahan Wewenang Modul:** Proyek ini adalah tugas kelompok di mana aktor dibagi antar anggota. Modul utama yang dikelola di repositori ini adalah aktor **Pengguna**.
* **Larangan Perubahan Sepihak:** Jangan pernah memodifikasi berkas atau diagram yang menjadi ranah tanggung jawab anggota tim lain (misal: bagian Pengunjung tim, Petugas, atau Admin) tanpa persetujuan eksplisit.
* **Pencatatan Utang Teknis:** Jika menemukan inkonsistensi, celah logika, atau kesalahan redaksional pada dokumen rekan tim, biarkan berkas asli mereka tetap apa adanya, dan catat temuan secara formal ke dalam TECHNICAL_DEBT.md sebagai bahan diskusi konsolidasi kelompok.

---

## 3. Integritas Sinkronisasi Multi-Format Dokumen
* Ketika melakukan perbaikan atau penyesuaian pada suatu use case/fitur, periksa dan perbarui **seluruh format representasi yang ada secara serempak**:
  * Diagram UML PlantUML (.puml di ctivity_diagram/ dan userflow/).
  * Dokumen skenario versi web/HTML (docs/use_case_scenarios.html).
  * Dokumen skenario versi pengolah kata Word (docs/Skenario_Use_Case_Pengguna_PPK2026.docx).
* Dilarang memperbarui salah satu format dan membiarkan format lainnya usang (*stale*).

---

## 4. Mindset Full-System Dual-Lens
* **Fokus Eksekusi:** Berikan kedalaman analisis maksimal pada aktor Pengguna.
* **Kewaspadaan Sistem Utuh:** Selalu pantau dan pahami arsitektur sistem secara keseluruhan (Petugas dan Admin). Antisipasi titik integrasi silang (*cross-actor seams*), seperti:
  * Penguncian slot reservasi saat fasilitas diubah menjadi *Dalam Perbaikan*.
  * Hubungan pendaftaran mandiri pengguna dengan persetujuan verifikasi akun admin.
  * Pelepasan slot jadwal saat reservasi dibatalkan darurat oleh petugas.
