# Keputusan Proyek

Keputusan produk yang sudah disetujui dan keputusan teknis yang masih terbuka. Jangan mengubah keputusan produk/scope tanpa persetujuan pemilik proyek.

## Keputusan produk

| ID | Keputusan | Implikasi |
|---|---|---|
| D-001 | Produk adalah editor video desktop profesional dengan UI/UX ramah pengguna. | Bukan hanya pemotong video sederhana. |
| D-002 | Rust menjadi fondasi utama aplikasi. | Komponen media eksternal matang tetap boleh digunakan. |
| D-003 | Windows desktop 64-bit adalah platform pertama. | Fokus desktop dahulu. |
| D-004 | Mobile merupakan roadmap masa depan. | Jangan implementasikan mobile sekarang. |
| D-005 | Tidak ada fitur AI internal. | Efek, subtitle, mask, audio, dan grading memakai pemrosesan manual/konvensional. |
| D-006 | Workflow UI divalidasi sebelum engine besar. | Hindari mengunci arsitektur tanpa uji. |
| D-007 | Workspace Tabs berada di bar paling atas seperti Blender. | Edit, Effects, Color, Audio, Titles & Motion, Export dan custom workspace. |
| D-008 | Setiap workspace menyimpan layout sendiri. | Reset satu workspace tidak mengubah workspace lain. |
| D-009 | Editor areas modular. | Resize, split, move, swap, tabbed, maximize, float, redock. |
| D-010 | Shift + Hover/Focus adalah contextual help utama. | Shift + Click bukan mekanisme utama. |
| D-011 | Media dapat dipreview sebelum masuk timeline. | Source Preview terpisah dari Program Viewer. |
| D-012 | Timeline menggunakan Universal Track. | Jangan membagi track permanen menjadi audio-only/video-only. |
| D-013 | Embedded audio/video dapat menjadi linked components. | Mendukung unlink/relink dan sinkronisasi. |
| D-014 | Group berbeda dari Compound Clip. | Group menjaga clip individual; Compound membungkus isi internal. |
| D-015 | Mask merupakan fitur inti. | Shape, Pen/Bezier, brush, feather, smoothing, expansion, keyframe. |
| D-016 | Corner Pin berbeda dari Mesh/Freeform Warp. | Empat corner versus mesh control points. |
| D-017 | Warp berlaku untuk objek visual kompatibel. | Bukan audio-only. |
| D-018 | Shape Creation mendukung keyframe parameter yang relevan. | Shape menjadi elemen animasi. |
| D-019 | Animatable capability ditentukan per parameter. | Tidak semua properti dipaksa punya keyframe. |
| D-020 | Keyframe Lane dan Graph Editor adalah alat berbeda. | Lane untuk editing cepat; Graph untuk curves/easing. |
| D-021 | Text animation mencakup Typewriter dan reveal/motion presets. | Tetap menyediakan kontrol manual. |
| D-022 | Pengguna dapat membuat custom effects/transitions di editor. | Builder berbasis stack/parameter, mask, shape, warp, keyframes; tanpa Fusion nodes. |
| D-023 | Plugin Manager, Creator/Developer Tools, dan SDK masuk roadmap. | Runtime/security/API belum final. |
| D-024 | Dokumentasi pengguna/developer adalah bagian produk. | Bukan pekerjaan opsional. |
| D-025 | Hindari batasan produk arbitrer. | Batas fisik hardware/format tetap berlaku. |
| D-026 | AI coding agent harus menjaga status/changelog. | Status berdasarkan bukti. |
| D-027 | Distribusi Windows direncanakan melalui ZIP dan installer EXE. | Build melalui GitHub Actions. |
| D-028 | Updater membaca rilis/tag GitHub. | Perlu verifikasi integritas/keaslian, persetujuan, rollback. |
| D-029 | Nama produk belum final. | Perlu pemeriksaan nama sebelum branding/rilis. |
| D-030 | Setiap Editor Area memiliki Editor Type Switcher ala Blender. | Switcher pada header mengganti jenis editor di area yang sama; berbeda dari Workspace Tabs dan tabbed-editor action. |

## Keputusan Teknis yang Belum Final dan Agenda Validasi

| ID | Area | Hal yang harus diuji sebelum diputuskan |
|---|---|---|
| T-001 / DEC-01 | Framework UI Rust | Docking, floating, accessibility, rendering, Windows stability, maintenance. |
| T-002 / DEC-02 | Media framework | FFmpeg/GStreamer/kombinasi; codec, lisensi, ukuran, seek, random access, dan sync. |
| T-003 / DEC-03 | GPU rendering | `wgpu`/native path, effects, transfer cost, fallback, interoperabilitas hardware decode. |
| T-004 / DEC-04 | Hardware decode/encode | Intel/AMD iGPU, driver/API, zero-copy feasibility, kualitas hasil, fallback. |
| T-005 | Audio engine | Sync, sample rate, routing, latency, DSP/plugin integration, voice-over recording. |
| T-006 / DEC-05 | Project file | Schema, references, cache, versioning, migration, recovery, forward compatibility. |
| T-007 / DEC-06 | Timeline internals | Universal Track, visual order, audio routing, linked/group/compound, timebase. |
| T-008 / DEC-07 | Plugin runtime | WASM/WASI atau alternatif; sandbox, permission, compatibility, ABI, failure isolation. |
| T-009 | Custom effect/transition builder | Declarative stack/parameters, masks, shapes, warp, keyframes; tanpa Fusion-style node compositor. |
| T-010 / DEC-09 | Warp engine | Homography, mesh interpolation, topology, keyframe behavior, performance. |
| T-011 / DEC-10 | Text rendering | Unicode, glyph shaping, fonts, layout, animation, cross-platform behavior. |
| T-012 / DEC-08 | Installer/updater | Signature, update helper, rollback, manifest, Windows compatibility, installer technology. |
| T-013 / DEC-11 | Mobile architecture | Shared project model/core boundaries dan UX research; bukan implementasi tahap desktop pertama. |

Setiap keputusan besar harus mencatat opsi, trade-off, risiko, biaya migrasi, bukti technical spike/benchmark, dan keputusan pemilik proyek. Jangan menganggap kandidat sebagai keputusan final hanya karena tercantum di sini.


## Analisis dan Status Nama Produk

### 1. Kriteria nama

Nama produk sebaiknya:

- Mudah dibaca dan diucapkan dalam Bahasa Indonesia maupun Inggris.
- Mudah dieja setelah didengar.
- Singkat dan kuat untuk ikon, splash screen, nama installer, repository, serta website.
- Tidak terlalu generik.
- Tidak mengunci aplikasi hanya pada cutting, color grading, atau motion graphics.
- Memiliki ruang untuk berkembang menjadi ekosistem plugin dan versi mobile.
- Tidak terlalu mirip nama editor atau merek yang sudah dikenal.

### 2. Hubungan dengan identitas pembuat

Nama produk dapat berdiri sendiri dengan penanda pembuat seperti `by AzkiVIP`. Nama aplikasi tidak harus sama dengan `AzkiVIP Studio`, karena identitas produk dan ekosistem komunitas dapat memiliki fungsi berbeda.

### 3. Kandidat yang pernah dibahas

`Kadrift Studio` pernah muncul sebagai kandidat brainstorming. Nama tersebut bisa terdengar cocok untuk aplikasi yang menggabungkan frame, editing, dan motion, tetapi belum dibuktikan ketersediaan merek maupun domainnya.

**Jangan menganggap Kadrift Studio sudah aman atau sudah dipilih.**

### 4. Kandidat yang sebaiknya dihindari dari screening awal

Pencarian awal yang pernah dilakukan menemukan penggunaan nama serupa/identik untuk produk atau layanan lain. Daftar ini bukan pemeriksaan merek dagang resmi:

| Nama | Catatan screening awal | Tindakan |
|---|---|---|
| FrameWeave | Ditemukan penggunaan nama pada aplikasi/produk lain. | Hindari sebagai kandidat utama. |
| Kineform | Ditemukan penggunaan terkait produk/studio kreatif. | Hindari. |
| Cutloom / CutLoom | Ditemukan beberapa penggunaan, termasuk konteks video. | Hindari. |
| Kadriva | Ditemukan penggunaan pada platform software. | Hindari. |
| FrameNest | Ditemukan penggunaan pada aplikasi/platform lain. | Hindari. |
| Moventra | Ditemukan penggunaan produk lain. | Hindari sampai ada pemeriksaan detail. |
| Kinetide | Ditemukan penggunaan brand lain. | Hindari. |
| Veyra | Ditemukan penggunaan software/editor. | Hindari. |

Pencarian web yang lama tidak menjamin kondisi nama saat ini. Lakukan pencarian ulang sebelum keputusan final.

### 5. Proses pemeriksaan sebelum memilih

1. Cari nama persis dan variasi ejaan di mesin pencari.
2. Cari repository GitHub dan nama organisasi.
3. Periksa domain yang relevan.
4. Periksa Microsoft Store dan platform distribusi software lain.
5. Cari merek dagang di WIPO Global Brand Database dan basis data merek Indonesia/DJKI.
6. Periksa kemiripan fonetik, bukan hanya ejaan persis.
7. Periksa kelas barang/jasa yang relevan dengan perangkat lunak.
8. Pastikan nama mudah diucapkan dan diingat melalui pengujian kecil.

### 6. Aturan sementara di source code

Sebelum nama final ditentukan:

- Gunakan nama repository yang netral, misalnya `video-editor` atau nama internal sementara.
- Pisahkan `product_id`, `display_name`, `publisher`, dan `repository_url` dalam konfigurasi.
- Hindari menyebarkan nama sementara ke banyak modul.
- Jangan memasukkan nama sementara ke format project yang tidak dapat diubah.
- Gunakan label internal sementara pada title bar dan package metadata.

### 7. Keputusan yang dibutuhkan nanti

Nama final harus disetujui pemilik proyek setelah daftar kandidat pendek, hasil pencarian, pemeriksaan merek, domain, dan pertimbangan branding tersedia.

## Format Catatan Keputusan Teknis

Gunakan format berikut hanya saat keputusan penting membutuhkan catatan yang dapat ditelusuri. Tidak perlu membuat ADR terpisah untuk setiap diskusi kecil atau ide yang belum siap diputuskan.

- **Status:** Diusulkan / Disetujui / Ditolak / Digantikan
- **Tanggal:** YYYY-MM-DD
- **Pemilik keputusan:** Pemilik proyek
- **Area:** UI / Media / Timeline / Render / Audio / Plugin / Rilis / lainnya

### 1. Konteks

Jelaskan masalah dan alasan keputusan perlu dibuat. Sertakan requirement yang terkait.

### 2. Pilihan yang dipertimbangkan

#### Pilihan A

- Kelebihan:
- Kekurangan:
- Risiko:
- Biaya implementasi/pemeliharaan:

#### Pilihan B

- Kelebihan:
- Kekurangan:
- Risiko:
- Biaya implementasi/pemeliharaan:

### 3. Keputusan

Tulis keputusan secara tegas. Jika belum disetujui, biarkan status `Diusulkan`.

### 4. Alasan

Jelaskan bukti, hasil technical spike, benchmark, kompatibilitas, lisensi, atau pertimbangan UX yang mendukung keputusan.

### 5. Konsekuensi

#### Positif

- ...

#### Negatif / trade-off

- ...

#### Dampak migrasi dan kompatibilitas

- ...

### 6. Rencana validasi

- Tes yang harus dilakukan:
- Acceptance criteria:
- Kondisi yang membuat keputusan perlu ditinjau ulang:

### 7. Referensi

- Tautan dokumentasi resmi atau hasil pengujian.
