# Render, Performance, Storage, Stability, dan Release
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
---## 20. Render, Export, dan Hasil Akhir

| ID | Fitur | Persyaratan |
|---|---|---|
| EXP-01 | Export Settings | Pengaturan export lengkap. |
| EXP-02 | Resolution | Resolusi custom. |
| EXP-03 | Aspect Ratio | Rasio custom/preset. |
| EXP-04 | Frame Rate | FPS output. |
| EXP-05 | Video Codec | H.264, H.265, AV1, dan format lain sesuai dukungan platform. |
| EXP-06 | Audio Codec | AAC, PCM, dan codec lain yang didukung. |
| EXP-07 | Container | MP4, MOV, MKV, dan format sesuai dukungan. |
| EXP-08 | Bitrate Control | Target/max bitrate dan quality mode sesuai encoder. |
| EXP-09 | Quality Presets | Preset kualitas. |
| EXP-10 | Hardware Encoding | Hardware encoder bila tersedia. |
| EXP-11 | Software Encoding | CPU fallback. |
| EXP-12 | Render Queue | Antrean render. |
| EXP-13 | Batch Export | Banyak job/output. |
| EXP-14 | Export Range | Entire timeline, In/Out, selected range. |
| EXP-15 | Custom Presets | Menyimpan konfigurasi. |
| EXP-16 | Platform Presets | Preset output untuk kebutuhan publikasi umum. |
| EXP-17 | Background Rendering | Render tanpa membekukan UI. |
| EXP-18 | Progress | Progress, ETA bila dapat dihitung secara jujur, status. |
| EXP-19 | Cancel/Pause/Resume | Kontrol job sesuai kemampuan engine. |
| EXP-20 | Error Details | Alasan kegagalan yang dapat dipahami. |
| EXP-21 | Audio-only Export | Export hanya audio. |
| EXP-22 | Still Export | Export frame gambar. |
| EXP-23 | Multiple Resolutions | Beberapa output dari satu project/job. |
| EXP-24 | Cache Reuse | Menggunakan cache yang masih valid. |
| EXP-25 | Export Validation | Memeriksa setting dan media offline sebelum render. |
---

---

## 22. Performa dan Pengelolaan Sumber Daya

Target utama: pengalaman editing dasar yang responsif pada perangkat kelas menengah/rendah, termasuk konfigurasi sekitar 8 GB RAM dan integrated GPU. Ini target optimasi, bukan jaminan semua codec/effect/proyek berjalan real-time.

| ID | Fitur | Persyaratan |
|---|---|---|
| PERF-01 | Smart Proxy | Proxy untuk footage berat. |
| PERF-02 | Proxy Quality | Pilihan resolusi/codec proxy. |
| PERF-03 | Dynamic Preview Resolution | Full/Half/Quarter/Auto. |
| PERF-04 | Smart Render Cache | Cache hasil pemrosesan. |
| PERF-05 | Disk Cache | Cache di disk/SSD. |
| PERF-06 | Memory Cache | Cache RAM dengan batas terkontrol. |
| PERF-07 | Cache Invalidation | Hanya membangun ulang bagian yang berubah. |
| PERF-08 | GPU Acceleration | Jalankan efek GPU-friendly pada GPU. |
| PERF-09 | Hardware Decoding | Gunakan hardware decoder bila didukung. |
| PERF-10 | Hardware Encoding | Gunakan hardware encoder bila didukung. |
| PERF-11 | CPU Fallback | Jalur CPU jika hardware tidak cocok. |
| PERF-12 | Background Tasks | Thumbnail, waveform, proxy, dan tugas sekunder di background. |
| PERF-13 | Task Priority | Playback/interaksi mendapat prioritas yang sesuai. |
| PERF-14 | Performance Monitor | CPU, RAM, GPU, cache, decoder, dropped frames bila tersedia. |
| PERF-15 | Cache Manager | Melihat/membersihkan cache. |
| PERF-16 | Memory Budget | Mengontrol penggunaan RAM. |
| PERF-17 | GPU Memory Awareness | Mengingat iGPU memakai shared system memory. |
| PERF-18 | Zero-copy Pipeline | Mengurangi copy GPU↔RAM yang tidak perlu jika jalur hardware mendukung. |
| PERF-19 | Timeline Virtualization | UI timeline tidak merender seluruh item di luar viewport secara sia-sia. |
| PERF-20 | Background Thumbnail Generation | Thumbnail dibuat tanpa UI freeze. |
| PERF-21 | Adaptive Processing | Kualitas preview menyesuaikan performa. |
| PERF-22 | Cache Cleanup Policy | Cache lama dapat dihapus sesuai kebijakan pengguna. |
| PERF-23 | Long-session Stability | Uji memory leak dan resource accumulation pada sesi panjang. |

#### 22.1 Aturan Performa

- Jangan berasumsi Rust otomatis membuat playback lancar.
- Jangan mengasumsikan semua efek dapat berjalan real-time pada 8 GB RAM/iGPU.
- Jangan memindahkan setiap frame GPU→RAM→GPU tanpa alasan.
- Shared memory iGPU tetap dihitung sebagai bagian penggunaan memori sistem.
- Proxy dan cache tidak boleh mengubah kualitas export final.
- Playback boleh menurunkan kualitas preview; export tetap memakai source dan setting final.
- UI harus tetap responsif saat proxy, waveform, thumbnail, atau export berjalan.

#### 22.2 Skenario Benchmark

| Scenario | Tujuan |
|---|---|
| 1080p H.264 30 FPS, 1 track, tanpa effect | Baseline playback |
| 1080p beberapa track, efek dasar | Uji compositing/cache |
| 4K source menggunakan proxy | Uji proxy workflow |
| Edit parameter efek saat playback | Uji interactive responsiveness |
| Keyframe dan mask animation | Uji update parameter real-time |
| Warp/mesh deformation | Uji efek geometry |
| Export sambil tetap menggunakan UI | Uji background renderer |
| Project panjang dan banyak media | Uji RAM/cache stability |

Catat spesifikasi CPU/iGPU, driver, codec, resolusi, FPS, jumlah track, jenis effect, preview quality, RAM, dropped frames, dan durasi export. Jangan hanya menyebut “smooth” tanpa data.

---

---

## 23. Alur Kerja, Stabilitas, dan Produktivitas

| ID | Fitur | Persyaratan |
|---|---|---|
| SYS-01 | Undo | Undo actions. |
| SYS-02 | Redo | Redo actions. |
| SYS-03 | History Panel | Riwayat tindakan. |
| SYS-04 | Autosave | Simpan otomatis. |
| SYS-05 | Crash Recovery | Memulihkan sesi setelah crash. |
| SYS-06 | Project Backup | Backup project berkala. |
| SYS-07 | Project Versioning | Menyimpan versi. |
| SYS-08 | Custom Shortcuts | Shortcut custom. |
| SYS-09 | Shortcut Search | Cari shortcut. |
| SYS-10 | Command Search | Cari perintah aplikasi. |
| SYS-11 | Context Menus | Menu sesuai konteks. |
| SYS-12 | Tooltips/Shift Help | Bantuan kontrol. |
| SYS-13 | Preset Manager | Mengelola preset. |
| SYS-14 | Favorites | Favorite effects/tools/transitions. |
| SYS-15 | Copy Attributes | Menyalin parameter. |
| SYS-16 | Paste Attributes | Menerapkan parameter. |
| SYS-17 | Project Notes | Catatan project. |
| SYS-18 | Workspace Save | Simpan layout custom. |
| SYS-19 | Media Usage | Menemukan penggunaan aset. |
| SYS-20 | Project Validation | Memeriksa media/settings. |
| SYS-21 | Clear Error Messages | Error harus actionable. |
| SYS-22 | Crash-safe Save | Mencegah kerusakan project. |
| SYS-23 | Keyboard-first Workflow | Operasi utama dapat dilakukan dengan keyboard. |
| SYS-24 | Accessibility Basics | Fokus keyboard, contrast, label, scaling. |

---

---

## 26. File Proyek, Penyimpanan Persisten, dan Keamanan Data

- Project harus disimpan otomatis sesuai interval yang dapat dikonfigurasi.
- Autosave tidak boleh menimpa file utama secara tidak aman; gunakan temporary write/atomic replace bila memungkinkan.
- Buat backup atau recovery journal untuk mengurangi kehilangan pekerjaan.
- Project menyimpan referensi media, timeline, clip relationships, tracks, effects, masks, shapes, text, keyframes, workspaces, settings yang relevan, dan plugin dependencies.
- Project tidak boleh mengandalkan absolute paths saja; sediakan relink/recovery.
- Schema project memiliki version number.
- Setiap perubahan schema memerlukan migration atau penjelasan kompatibilitas.
- Project dari versi lebih baru tidak boleh dibuka lalu disimpan ulang secara diam-diam dengan hilangnya data.
- Export preset, effect preset, transition preset, plugin, dan workspace layout dikelola sebagai aset terpisah sesuai kebutuhan.
- Reset workspace tidak boleh menghapus data project atau user presets.

---

---

## 27. Repository GitHub, CI, ZIP, EXE, dan Sistem Pembaruan

Pengembangan dilakukan melalui repository GitHub. AI coding agent mengubah project, lalu GitHub Actions menjalankan pemeriksaan dan build. Setiap handoff yang diminta harus dapat diberikan sebagai ZIP, dan release Windows menghasilkan executable/package.

### 27.1 Hasil Build yang Diharapkan

1. **Source Project ZIP:** source code, dokumentasi, config, workflow GitHub Actions, dan file build yang dibutuhkan; tidak perlu menyertakan cache/build output besar yang bisa dihasilkan ulang.
2. **Windows Release ZIP:** aplikasi portable dan resource yang diperlukan, jika mode portable disediakan.
3. **Windows Installer `.exe`:** installer yang dibuat oleh CI/release workflow.
4. **Checksums:** SHA-256 untuk setiap asset rilis.
5. **Release Notes:** changelog dan instruksi upgrade.

Pastikan artefak ZIP tidak berisi secret, token, signing key, cache pribadi, file temporary, atau media pribadi.

### 27.2 Pipeline GitHub Actions

Workflow release yang disarankan:

1. Trigger pada push tag versi, misalnya `v0.1.0`, atau manual workflow dispatch untuk testing.
2. Checkout repository.
3. Setup toolchain Rust dan target Windows.
4. Restore dependency/cache build secara aman.
5. Run formatting checks.
6. Run lint/static checks.
7. Run unit tests.
8. Run integration tests yang dapat dijalankan di CI.
9. Build release executable.
10. Package runtime resources/dependencies.
11. Create portable ZIP dan/atau installer `.exe`.
12. Generate SHA-256 checksum.
13. Upload workflow artifacts.
14. Publish GitHub Release dan attach assets ketika tag merupakan release yang disetujui.
15. Simpan logs dan kegagalan build yang dapat ditelusuri.

Jangan menerbitkan release stabil dari commit yang gagal tes. Pre-release/beta harus diberi label dengan benar.

### 27.3 Pengelolaan Versi

Gunakan Semantic Versioning:

- `MAJOR`: perubahan besar yang tidak kompatibel.
- `MINOR`: fitur baru yang kompatibel.
- `PATCH`: bug fix/perbaikan kompatibel.

Tag release harus konsisten dengan version metadata aplikasi. Jangan membuat nomor versi berbeda antara UI aplikasi, Git tag, GitHub Release, installer, dan changelog.

### 27.4 Updater di Dalam Aplikasi

Aplikasi dapat memeriksa GitHub Releases untuk mengetahui versi terbaru.

| ID | Fitur | Persyaratan |
|---|---|---|
| UPD-01 | Check for Updates | Memeriksa release terbaru di repository resmi. |
| UPD-02 | Version Comparison | Membandingkan versi menggunakan SemVer, bukan string biasa. |
| UPD-03 | Update Channel | Stable secara default; beta/prerelease opsional dan dapat dipilih. |
| UPD-04 | Release Notes | Menampilkan catatan perubahan. |
| UPD-05 | Download Update | Mengunduh asset yang sesuai platform. |
| UPD-06 | Checksum Verification | Memverifikasi SHA-256 sebelum instalasi. |
| UPD-07 | Signature Verification | Mendukung verifikasi tanda tangan jika signing tersedia. |
| UPD-08 | User Consent | Pengguna menyetujui update sebelum pemasangan. |
| UPD-09 | Restart to Update | Update diterapkan saat aplikasi ditutup/restart. |
| UPD-10 | Backup/Rollback | Memiliki strategi pemulihan jika update gagal. |
| UPD-11 | Update Error Handling | Menjelaskan kegagalan jaringan/download/verification. |
| UPD-12 | Manual Check | Pengguna dapat mengecek update dari Settings/About. |
| UPD-13 | Offline Behavior | Jika GitHub tidak dapat diakses, aplikasi tetap berfungsi dan tidak memaksa update. |
| UPD-14 | Rate-limited Checks | Jangan melakukan request berlebihan; cache informasi release sesuai kebijakan. |
| UPD-15 | No Silent Install | Jangan menginstal update tanpa persetujuan pengguna. |

**Windows implementation note:** executable yang sedang berjalan tidak dapat selalu diganti secara langsung. Gunakan updater/helper process atau mekanisme restart yang aman. Update harus atomic atau memiliki rollback path.

**Security note:** jangan mengeksekusi file unduhan sebelum checksum/signature verification. URL, repo owner, dan repo name harus dikonfigurasi secara eksplisit dan tidak dapat diganti oleh metadata tidak tepercaya.

### 27.5 Metadata Repository

Buat dan rawat:

- `README.md`
- `LICENSE`
- `CONTRIBUTING.md`
- `SECURITY.md`
- `CHANGELOG.md`
- `.github/workflows/ci.yml`
- `.github/workflows/release-windows.yml`
- `.github/ISSUE_TEMPLATE/`
- `.github/PULL_REQUEST_TEMPLATE.md`

---
