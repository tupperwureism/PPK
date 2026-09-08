---
name: deliberate-execution
description: Enforce deliberate, step-by-step execution with self-reflective cross-file consistency audits and hierarchical batch chunking. Use when making multi-file modifications, executing large or complex tasks, when updating shared domain entities across diagrams/docs, or when the user asks to be thorough, reflect, check across instances, or avoid rushing.
---

# Deliberate Execution & Multi-Instance Consistency Guard (Flash Governor)

This skill provides a cognitive and operational governor to counter the natural tendency of high-speed models to rush (*premature completion*) and focus on single files (*tunnel vision*). It enforces a rigorous two-tier verification workflow: **Atomic Reflection Loop (Behavior A)** and **Recursive Chunking & Global Convergence (Behavior B)**.

---

## 1. Perilaku A: The Atomic Reflection Loop (Perilaku Pertama & Utama)

**Kapan Digunakan:** Setiap kali selesai melakukan satu perubahan, penambahan, atau perbaikan pada suatu berkas atau entitas sistem (misal: use case, status enum, aturan waktu, relasi data).

### Langkah-Langkah Operasional:
1. **Multi-Instance Discovery (Pemindaian Menyeluruh):**
   * Dilarang berhenti setelah mengedit satu berkas saja.
   * Lakukan pemindaian aktif (menggunakan search/grep) ke seluruh repositori untuk mendata SEMUA berkas yang merepresentasikan konsep yang sama.
   * Contoh pada proyek web/UML: jika memperbarui sebuah use case/fitur, periksa:
     * Diagram alur aktivitas (`activity_diagram/*.puml`)
     * Diagram alur layar (`userflow/*.puml`)
     * Dokumen skenario HTML (`docs/*.html`)
     * Dokumen pengolah kata Word (`docs/*.docx`)
     * Dokumen ringkasan/spesifikasi (`ringkasan.md`, `CONTEXT.md`, `PRD.md`)

2. **The Self-Grill & Reflection Gate (Merenung & Uji Kritis):**
   * Sebelum menyatakan selesai, ajukan 5 pertanyaan uji kritis kepada diri sendiri:
     1. *Apakah saya hanya mengedit 1 format dokumen dan melupakan format kembarannya?*
     2. *Apakah setiap string status enum, kode error, dan parameter data identik karakter-demi-karakter di semua berkas terdampak?*
     3. *Apakah perubahan ini memicu efek samping (downstream impact) pada logika aktor lain?*
     4. *Apakah saya sudah membaca fisik isi berkas setelah diedit untuk memastikan tidak ada syntax/rendering error?*
     5. *Apakah ada asumsi terselubung yang belum divalidasi?*

3. **Dokumentasi Bukti (`progress.md`):**
   * Buat atau perbarui berkas pelacak `progress.md` di workspace yang memuat bukti tabel matriks sinkronisasi (*Proof of Cross-Instance Consistency*).
   * Hanya laporkan ke pengguna jika seluruh instance sudah diverifikasi 100% konsisten.

---

## 2. Perilaku B: Recursive Chunking & Global Convergence (Untuk Tugas Besar / Batch)

**Kapan Digunakan:** Saat menerima instruksi yang berskala besar, multi-langkah, atau kompleks (misal: "Selesaikan Batch A", "Rancang seluruh diagram aktor X", "Refaktor modul Y").

### Langkah-Langkah Operasional:
1. **Dekomposisi Atomik (A -> A1, A2, A3):**
   * Bedah tugas besar (Batch A) menjadi unit-unit atomik yang dapat diselesaikan dan diuji secara independen.
   * Buat rencana dekomposisi yang eksplisit di awal:
     * Sub-tugas A1: Unit terkecil pertama.
     * Sub-tugas A2: Unit kedua yang bergantung pada A1.
     * Sub-tugas A3: Unit ketiga, dst.

2. **Eksekusi Sekuensial Berpagar (Gated Sequential Execution):**
   * Kerjakan sub-tugas A1.
   * Terapkan **Perilaku A (Atomic Reflection Loop)** pada A1. Kunci keabsahannya sampai benar-benar yakin dan terbukti di `progress.md`.
   * **Dilarang menyentuh A2 sebelum A1 dinyatakan 100% valid.**
   * Ulangi proses yang sama untuk A2, A3, dst.

3. **Gerbang Konsistensi Global Antar-Bagian (Inter-Atom Harmony Check):**
   * Setelah seluruh sub-tugas (A1, A2, A3) selesai secara lokal, lakukan audit hubungan silang:
     * Apakah definisi data di A1 tidak bertentangan dengan asumsi di A3?
     * Apakah transisi alur dari A1 menuju A2 berjalan mulus tanpa celah logika?

4. **Sapuan Regresi Menyeluruh (Whole-System Final Sweep):**
   * Jalankan **Perilaku A** sekali lagi pada skala keseluruhan proyek (seluruh direktori dan berkas terdampak).
   * Verifikasi bahwa tidak ada berkas usang (*stale artifacts*) yang tertinggal.
   * Perbarui status akhir Batch A menjadi `Completed` di `progress.md` sebelum memberikan laporan akhir kepada pengguna.

---

## 3. Format Pelacak Kemajuan (`progress.md` Template)

Gunakan struktur standar berikut saat memperbarui `progress.md`:

```markdown
# Laporan Pelacakan Kemajuan & Konsistensi (progress.md)

## 1. Status Batch / Sub-Tugas
- [x] Sub-tugas A1: [Deskripsi] -> Verified (Tier A Passed)
- [ ] Sub-tugas A2: [Deskripsi] -> In Progress
- [ ] Sub-tugas A3: [Deskripsi] -> Pending

## 2. Matriks Verifikasi Multi-Instance (Tier A Proof)
| Entitas / Fitur | Berkas Representasi | Status Sinkronisasi | Bukti Verifikasi |
| :--- | :--- | :---: | :--- |
| UC-xx | activity_diagram/*.puml | Verified | Decision diamond sesuai |
| UC-xx | userflow/*.puml | Verified | Screen flow match |
| UC-xx | docs/*.html | Verified | Teks skenario sinkron |
| UC-xx | docs/*.docx | Verified | Tabel docx terupdate |

## 3. Gerbang Konsistensi Global (Tier B Gate)
- Inter-atom compatibility: [PASSED / PENDING]
- Whole-system regression sweep: [PASSED / PENDING]
```
