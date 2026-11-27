# Spesifikasi Utama — Produk dan Pengembangan Editor Video

> **Status:** Draf 0.2  
> **Peran dokumen:** Sumber kebenaran utama (SSOT) untuk cakupan produk, UI/UX, interaksi, fitur, batasan, dan aturan kerja AI coding agent.  
> **Platform awal:** Windows desktop 64-bit  
> **Teknologi inti yang direncanakan:** Rust  
> **Mobile:** Direncanakan untuk tahap mendatang, bukan cakupan rilis desktop pertama.  
> **AI di dalam aplikasi:** Tidak ada fitur AI. AI hanya boleh digunakan sebagai alat bantu pengembangan oleh pemilik proyek.

---

## 0. Cara Membaca dan Menggunakan Spesifikasi Ini

Dokumen ini menjadi **indeks dan sumber aturan global**. Detail spesifikasi fitur telah dipisahkan ke dokumen domain agar AI/developer dapat membaca hanya bagian yang relevan. Gabungan Master Spec dan dokumen domain adalah sumber kebenaran requirement produk.

Alur baca: `AGENTS.md` → periksa `PROJECT_STATUS.md` → gunakan indeks domain di bawah → baca dokumen fitur yang relevan. Gunakan `ARCHITECTURE.md` dan `DECISIONS.md` bila task menyentuh desain teknis atau keputusan yang belum final. Skill dan dokumentasi resmi dibaca selektif sesuai task.

Jika ada konflik, jangan menghapus requirement berdasarkan asumsi. Ikuti keputusan pemilik proyek yang paling baru dan tercatat. Untuk konflik yang belum terselesaikan, jelaskan dan minta keputusan sebelum membuat perubahan berdampak besar.

### 0.1 Prioritas Sumber Kebenaran

1. Keputusan terbaru yang secara eksplisit disetujui pemilik proyek dan tercatat di `DECISIONS.md`.
2. Visi, batas produk, istilah global, standar dokumentasi, dan aturan UI/UX di Master Spec.
3. Requirement detail pada dokumen domain di `docs/features/` dan acceptance/testing di `docs/quality/`.
4. Kontrak teknis yang sudah disetujui dalam `ARCHITECTURE.md`.
5. `PROJECT_STATUS.md` untuk status implementasi dan task aktif, bukan untuk mengubah requirement.
6. Source code dan hasil tes untuk kondisi implementasi aktual.
7. Percakapan lama, asumsi, mockup, atau hasil AI yang belum disetujui bukan sumber requirement.

### 0.2 Indeks Dokumen Domain

| Domain | Bagian asli yang dipertahankan | Dokumen sumber |
|---|---:|---|
| Workspace, Editor Areas, docking, Shift Help | 3–5 | [WORKSPACE_AND_EDITOR_AREAS.md](docs/features/WORKSPACE_AND_EDITOR_AREAS.md) |
| Project, media, Viewer, Timeline, clip operations | 6–9 | [CORE_EDITING.md](docs/features/CORE_EDITING.md) |
| Inspector, effect stack, built-in effects, generators, mask, warp, shape, keyframe, 2D/2.5D VFX, Custom Effect Builder | 10–14, 18.1 | [EFFECTS_AND_2D_VFX.md](docs/features/EFFECTS_AND_2D_VFX.md) |
| Custom Transition Builder dan transition library | 18.2, 19 | [TRANSITIONS.md](docs/features/TRANSITIONS.md) |
| Color correction dan grading | 15 | [COLOR_GRADING.md](docs/features/COLOR_GRADING.md) |
| Audio editing dan mixing | 16 | [AUDIO.md](docs/features/AUDIO.md) |
| Title, text, subtitle, motion | 17 | [TITLES_AND_MOTION.md](docs/features/TITLES_AND_MOTION.md) |
| Universal Preset Library lintas fitur | 18.3 | [PRESETS.md](docs/features/PRESETS.md) |
| Plugin Manager, Creator, SDK, compatibility | 21 | [PLUGIN_SYSTEM.md](docs/features/PLUGIN_SYSTEM.md) |
| Render/export, performance, stability, project storage, CI/release/updater | 20, 22–23, 26–27 | [RENDER_AND_PERFORMANCE.md](docs/features/RENDER_AND_PERFORMANCE.md) |
| Acceptance criteria dan strategi pengujian | 29 | [ACCEPTANCE_AND_TESTING.md](docs/quality/ACCEPTANCE_AND_TESTING.md) |

Nomor bagian asli sengaja dipertahankan di setiap dokumen agar ID requirement, riwayat diskusi, dan referensi lama tetap dapat dilacak. Istilah global—termasuk Workspace Tab, Workspace, Editor Area, Editor, dan Panel—tetap didefinisikan di Bagian 2 dokumen ini.

### 0.3 Cara Memilih Dokumen

- Task pada satu fitur: baca dokumen domain fitur tersebut dan bagian testing yang relevan.
- Task lintas fitur: baca dokumen domain yang benar-benar terdampak; jangan memuat semua dokumen secara otomatis.
- Task teknis: baca domain produk bersama bagian terkait di `ARCHITECTURE.md` dan catatan di `DECISIONS.md`.
- Task UI/UX: gunakan desain/wireframe/HTML prototype dari pemilik proyek sebagai acuan visual; spesifikasi menjelaskan perilaku dan acceptance criteria, bukan mendesain ulang tampilan.

---

# 1. Visi Produk

Membangun **professional non-linear video editor desktop** berbasis Rust yang menawarkan workflow editing, effects, color grading, audio, title/motion graphics, masking, keyframes, plugin system, serta workspace modular yang sangat dapat dikustomisasi.

Aplikasi harus:

- Memiliki kemampuan editing profesional dan tidak sengaja membatasi pengguna hanya pada workflow sederhana.
- Memiliki UI/UX yang lebih mudah dipelajari dibandingkan editor profesional yang sangat kompleks.
- Menggunakan sistem workspace modular yang terinspirasi dari Blender.
- Menempatkan workspace tabs di bagian paling atas aplikasi; setiap workspace memiliki layout dan editor areas sendiri.
- Memungkinkan setiap editor area dipindah, di-resize, di-split, di-swap, di-tab-kan, di-maximize, di-float ke window baru, dan di-dock kembali.
- Menyediakan video preview dari media sebelum media dimasukkan ke timeline.
- Menggunakan universal tracks: track bukan khusus video atau audio.
- Menyediakan linked audio-video components yang otomatis mengikuti ketika dipindahkan, dengan opsi unlink/relink.
- Memiliki Group/Ungroup serta Compound Clip/Open/Decompose sebagai dua konsep terpisah.
- Memiliki mask editor yang dapat digambar langsung di Viewer dengan shape, pen/Bezier, dan freehand brush.
- Memiliki Corner Pin dan Freeform/Mesh Warp dengan titik kontrol yang dapat ditambahkan, dihapus, digeser, dan di-keyframe.
- Mendukung keyframe pada sebagian besar parameter yang memang dapat dianimasikan, termasuk transform, effects, mask, shapes, text animation, warp, speed, dan audio automation.
- Menyediakan Graph Editor terpisah yang hanya menampilkan channel/parameter terpilih.
- Memungkinkan pengguna membuat custom effects dan custom transitions langsung dari editor tanpa harus membangun Fusion/node compositor.
- Menyediakan 2D Visual Composition untuk merangkai shape, vector path, text, image, video, dan generator menjadi komposisi motion graphics berlapis yang tetap editable.
- Menyediakan procedural visual generators, termasuk pola, tekstur, dan 2D particle emitter yang dapat dianimasikan.
- Menyediakan 2.5D motion graphics untuk depth, extrusion/bevel text dan shape, perspektif kamera, serta lighting sederhana; full 3D scene compositing tetap di luar scope.
- Menyediakan Preset Library lintas kategori agar efek, transisi, generator, shape, komposisi, animasi, color, audio, dan pengaturan lain dapat disimpan serta digunakan ulang.
- Memiliki text animation presets seperti Typewriter serta kontrol manual.
- Memiliki plugin authoring/development workflow dan Plugin SDK.
- Memiliki dokumentasi pengguna, referensi fitur, shortcut, dan dokumentasi developer/API yang lengkap.
- Mengutamakan efisiensi resource, proxy, cache, hardware acceleration, dan UI yang tidak freeze.
- Tidak menyertakan fitur AI di aplikasi.

## 1.1 Sasaran Pengguna

- Pemula yang membutuhkan interface mudah dipahami.
- Content creator yang membuat video panjang maupun pendek.
- Editor yang membutuhkan multi-track editing, color, audio, keyframe, mask, titles, dan efek.
- Pengguna advanced yang ingin mengatur workspace, parameter, kurva, preset, dan plugin sendiri.

## 1.2 Platform dan Batas Lingkup

| Area | Keputusan |
|---|---|
| Platform pertama | Windows desktop 64-bit |
| Paket aplikasi | `.exe` installer dan/atau portable package; release disertai ZIP |
| Bahasa inti | Rust |
| UI | Framework desktop Rust dipilih setelah technical spike; jangan mengunci framework hanya berdasarkan selera AI |
| Media engine | Evaluasi FFmpeg dan/atau GStreamer; gunakan komponen matang, jangan menulis codec dari nol |
| GPU | Abstraksi lintas vendor; evaluasi `wgpu` dan jalur native hardware decoding sesuai GPU/driver |
| Mobile | Roadmap masa depan; desain core agar tidak terlalu bergantung pada Windows UI |
| Fusion / node compositor / full 3D scene compositing | Tidak termasuk scope saat ini; lihat batas 2.5D pada Bagian 13.3 |
| 2.5D motion graphics | Termasuk scope terbatas sesuai Bagian 13.3; bukan full 3D scene editor |
| AI features | Tidak ada |
| User-facing limits | Hindari batas arbitrer yang tidak perlu; tetap hormati batas hardware, format, memori, dan stabilitas |

**Catatan penting:** “tanpa batasan” berarti jangan membuat pembatasan produk secara arbitrer—misalnya batas track kecil atau batas preset yang tidak perlu. Ini bukan berarti hardware, memori, format codec, atau performa tidak memiliki batas fisik.

---

---

# 2. Istilah Antarmuka

| Istilah | Arti |
|---|---|
| Workspace Tab | Tab tingkat atas di bar paling atas, misalnya Edit, Effects, Color, Audio. Setiap tab menyimpan konfigurasi layout-nya sendiri. |
| Workspace | Satu lingkungan kerja lengkap beserta layout, editor, dan panel yang dipakai. |
| Editor Area | Wilayah besar di dalam workspace yang menampilkan satu editor atau editor bertab. |
| Editor | Modul yang ditampilkan di area, misalnya Timeline, Viewer, Media Browser, Graph Editor, Scopes. |
| Panel | Kontrol di dalam editor, misalnya Transform, Mask, Color Wheels, atau Effect Parameters. |
| Docking | Memasang editor/window kembali ke area workspace. |
| Floating Window | Editor yang dilepas dari layout utama menjadi jendela OS terpisah. |
| Split Area | Membagi satu area menjadi dua atau lebih area. |
| Universal Track | Track timeline yang menerima berbagai jenis item, bukan track yang dibatasi khusus audio/video. |
| Linked Components | Dua atau lebih komponen yang tetap terhubung dan mengikuti operasi tertentu bersama-sama. |
| Group | Kumpulan clip yang tetap menjadi clip individual tetapi dapat dipilih/dioperasikan bersama. |
| Compound Clip | Beberapa item timeline dibungkus menjadi satu item yang dapat dibuka untuk mengedit isi internalnya. |
| Corner Pin | Distorsi perspektif memakai empat titik sudut. |
| Mesh/Freeform Warp | Deformasi gambar menggunakan mesh atau sejumlah control points yang dapat ditambah/dikurangi. |
| Generator | Sumber visual yang dibuat dari parameter, bukan dari footage yang sudah ada; misalnya gradient, pattern, noise, atau particle emitter. |
| 2D Visual Composition | Komposisi beberapa visual layer (shape, text, image, video, generator) yang dapat diedit sebagai satu elemen. |
| 2.5D Motion Graphics | Sistem visual dengan sumbu Z, perspektif, extrusion, kamera, dan lighting sederhana di dalam komposisi video 2D; bukan editor scene 3D umum. |
| Preset | Kumpulan parameter, stack, objek, atau template yang dapat disimpan dan diterapkan ulang tanpa membekukan hasil menjadi video. |
| Playhead | Garis/indikator posisi waktu pemutaran di timeline. |
| Ruler | Penggaris waktu di bagian atas timeline. |
| Keyframe Lane | Baris keyframe yang ditampilkan pada/di bawah clip atau parameter di timeline. |
| Graph Editor | Editor terpisah untuk mengatur kurva interpolasi dan nilai animasi. |

---

---

# 24. Dokumentasi Aplikasi

Dokumentasi adalah deliverable produk, bukan tugas opsional.

## 24.1 Dokumentasi Pengguna

- Quick Start
- Interface overview
- Workspace tabs dan cara custom/reset
- Editor areas, docking, split, floating windows
- Project creation/settings
- Import dan media browser
- Media preview/source monitor
- Timeline dan universal tracks
- Linked audio-video, unlink/relink
- Group vs Compound Clip
- Selection, Razor, Split at Playhead, snapping, Alt behavior
- Inspector dan Transform
- Effects dan effect stack
- Custom Effect Builder
- Transitions dan Custom Transition Builder
- Mask editor dan brush/feather
- Corners/Corner Pin dan Mesh/Freeform Warp
- Shape creation dan path editing
- 2D Visual Composition dan procedural generators
- 2.5D motion graphics
- Universal Preset Library dan semua jenis preset
- Keyframe lanes dan Graph Editor
- Color correction/grading dan scopes
- Audio editing/mixer
- Text, titles, Typewriter dan text animation
- Export/render queue
- Proxy/cache/performance settings
- Plugin Manager dan Plugin Creator
- Troubleshooting dan recovery

## 24.2 Dokumentasi Referensi

- Semua parameter dan rentang nilai.
- Daftar shortcut default.
- Daftar format/codec yang didukung per platform.
- Daftar efek, transisi, generator, dan kategori visual bawaan.
- Daftar jenis preset, metadata, format import/export, dan dependency.
- Plugin compatibility matrix.
- Project file schema/version.
- Update/release notes.
- Known limitations.

## 24.3 Dokumentasi Developer

- Architecture overview
- Build instructions
- Module boundaries
- Media pipeline
- Timeline data model
- Render graph/pipeline (bukan UI Fusion node editor)
- Cache/memory model
- GPU abstraction
- Plugin SDK/API
- Plugin packaging/signing/permissions
- Testing strategy
- Release workflow
- Update system
- Contribution guidelines

## 24.4 Dokumentasi yang Selalu Sinkron

Setiap fitur baru wajib memperbarui dokumentasi terkait. Dokumen harus diberi versi yang sesuai dengan rilis aplikasi. Fitur eksperimental harus diberi label experimental; fitur yang belum ada tidak boleh didokumentasikan seolah sudah tersedia.

---



### 24.5 Standar Penulisan dan Bantuan

Dokumentasi merupakan bagian produk. Dokumentasi harus mengikuti perilaku aplikasi yang benar-benar tersedia, bukan fitur yang baru direncanakan.

#### Standar Bantuan Kontekstual Shift

Setiap kontrol penting harus mempunyai dokumentasi singkat yang bisa dibuka dengan Shift + Hover/Focus.

Konten minimal:

- Nama fitur.
- Ringkasan fungsi.
- Kapan digunakan.
- Cara penggunaan.
- Penjelasan setiap parameter.
- Satuan dan rentang nilai bila ada.
- Shortcut terkait.
- Keterbatasan/format yang didukung.
- Tautan ke panduan lengkap.

Bantuan kontekstual tidak boleh menjanjikan kemampuan yang belum tersedia pada versi aplikasi pengguna.

#### Standar Penulisan

- Tulis dalam Bahasa Indonesia yang jelas dan konsisten.
- Istilah teknis yang sudah umum boleh dipertahankan dalam bahasa Inggris.
- Jelaskan istilah teknis saat pertama kali digunakan.
- Gunakan langkah bernomor untuk prosedur.
- Sertakan gambar/screenshot yang sesuai versi aplikasi.
- Sertakan contoh input/output bila membantu.
- Catat platform, versi, dan batasan yang relevan.
- Jangan menulis “akan” pada panduan pengguna seolah fitur sudah tersedia.
- Tandai fitur eksperimental/beta dengan jelas.

#### Dokumentasi di Dalam Aplikasi

Aplikasi sebaiknya memiliki pusat bantuan yang dapat dicari. Hasil pencarian harus dapat membuka topik yang tepat. Bila pengguna memilih kontrol lalu menekan Shift, bantuan harus mengarah ke dokumentasi kontrol tersebut, bukan hanya halaman umum.

#### Sinkronisasi Dokumentasi dengan Produk

- Perubahan UI harus memperbarui panduan terkait.
- Perubahan shortcut harus memperbarui referensi shortcut.
- Perubahan parameter efek harus memperbarui Shift Help dan referensi efek.
- Perubahan format project harus memperbarui dokumentasi migrasi.
- Perubahan API plugin harus memperbarui versi SDK dan migration guide.
- Setiap rilis harus memiliki release notes.

---

# 25. Aturan Kualitas UI/UX

1. Professional, clean, modern, and discoverable. Hindari UI berantakan yang menampilkan semua kontrol sekaligus.
2. Fitur advanced tersedia, tetapi dikelompokkan secara logis dan dapat dilipat.
3. Jangan mengurangi jumlah fitur hanya demi UI tampak sederhana. Sederhanakan akses, bukan kapabilitas.
4. Gunakan istilah yang konsisten; jangan mengganti nama perintah yang sama antar-workspace.
5. Kontrol penting harus mempunyai tooltip dan Shift Help.
6. Tidak boleh ada tombol dekoratif yang tidak berfungsi.
7. Setiap operasi destruktif harus dapat di-undo; operasi berisiko tinggi memerlukan konfirmasi yang proporsional.
8. Jangan mengunci pengguna pada satu layout.
9. Jangan membuat panel penting hilang tanpa cara untuk menemukannya kembali.
10. Semua editor harus punya cara untuk close, hide, restore, atau reset.
11. Responsive layout harus menjaga usability dan tidak hanya mengecilkan semua panel.
12. Dark dan Light theme harus sama-sama terbaca dan konsisten.
13. Jangan menggunakan warna status sebagai satu-satunya penanda; tambahkan label/icon.
14. Gunakan virtualization/efficient rendering untuk timeline dan list besar.
15. Jangan mengorbankan frame accuracy untuk animasi UI dekoratif.

---



### 25.1 Acuan Desain Visual

Desain visual, layout, komponen, spacing, warna, ikon, dan detail tampilannya mengikuti desain/wireframe/HTML prototype yang disediakan atau disetujui pemilik proyek. Jangan mengarang arah visual baru, melakukan redesign tanpa diminta, atau menulis spesifikasi estetika panjang yang menduplikasi desain tersebut. Master Spec tetap mendefinisikan perilaku, state, interaksi, constraint, aksesibilitas, dan acceptance criteria. Jika desain belum tersedia atau ada bagian yang ambigu dan memengaruhi implementasi, tandai secara spesifik dan minta arahan; jangan mengurangi fitur. Ikon wajib dirender sebagai aset ikon/SVG yang benar; markup SVG mentah tidak boleh tampil sebagai teks di UI, tooltip, atau Shift Help.

---

# 28. Aturan Pengembangan dengan AI

AI digunakan sebagai alat bantu pengembangan, bukan fitur di dalam aplikasi. Aturan operasional coding agent berada di `AGENTS.md` agar instruksi kerja hanya memiliki satu sumber.

Batasan produk yang tetap berlaku:

- Jangan menambah fitur AI internal.
- Jangan mengurangi atau menghapus requirement tanpa persetujuan pemilik proyek.
- Jangan memperlakukan prototype/mock sebagai bukti engine produksi.
- Jangan mengklaim codec, GPU acceleration, playback, render/export, plugin sandbox, atau updater berhasil tanpa pengujian nyata.
- Fusion/node compositor dan full 3D scene compositing tetap di luar scope awal. Implementasi 2.5D motion graphics yang dibatasi secara eksplisit di Bagian 13.3 termasuk scope.

---

# 30. Tahapan Pengembangan

Urutan berikut memisahkan validasi UI dari pengembangan engine. Fase dapat disesuaikan berdasarkan hasil pengujian, tetapi jangan melewati prasyarat teknis tanpa alasan.

| Phase | Fokus | Hasil yang harus tersedia |
|---|---|---|
| P0 | Product/UI validation | UI prototype interaktif; workspace tabs, docking, timeline interaction, inspector, mask/warp/shape/keyframe prototype, responsive. |
| P1 | Desktop shell & project model | App shell Rust, workspace persistence, project save/load, media browser, source preview. |
| P2 | Core media & timeline | Decode/playback, universal track model, clip operations, linked A/V, group/compound, audio sync. |
| P3 | Render pipeline & performance | Proxy, cache, preview quality, render queue, export, hardware paths. |
| P4 | Effects, generators & 2D VFX | Effect stack, built-in effects, mask, Corner Pin, Mesh/Freeform Warp, adjustment layer, procedural generators, 2D Visual Composition. |
| P5 | Animation, color & 2.5D | Keyframe, Graph Editor, color correction/grading, scopes, shapes, text animations, 2.5D motion graphics. |
| P6 | Audio, custom creation & presets | Audio mixing, custom effect builder, custom transition builder, generator presets, composition templates, universal preset library. |
| P7 | Plugin ecosystem & documentation | Plugin API/SDK, plugin creator, docs, sample plugins. |
| P8 | Hardening & release | Stability, regression tests, Windows packaging, GitHub Actions, updater, release notes. |
| P9 | Future mobile exploration | Shared core/API review and platform-specific UX research. Not part of first desktop release. |

---

---

# 31. Analisis Penamaan

Status nama produk, kandidat, screening awal, serta proses pemeriksaannya dipelihara di `DECISIONS.md`. Nama belum final dan setiap screening lama harus diverifikasi ulang sebelum branding atau distribusi publik.

---

# 32. Keputusan Teknis yang Belum Final

Satu-satunya daftar keputusan produk dan pertanyaan teknis terbuka dipelihara di `DECISIONS.md`. Jangan menyalin daftar keputusan ke file lain karena dapat menimbulkan versi yang berbeda.

---

# 33. Tautan Sumber dan Referensi

Referensi ini untuk memahami pola fitur dan perilakunya; bukan instruksi untuk menyalin kode atau branding produk lain.

- Blender Manual — Workspaces: https://docs.blender.org/manual/en/latest/interface/window_system/workspaces.html
- Blender Manual — Areas: https://docs.blender.org/manual/en/latest/interface/window_system/areas.html
- Kdenlive Manual — Corners effect: https://docs.kdenlive.org/en/effects_and_filters/video_effects/transform_distort_perspective/corners.html
- Kdenlive Manual — Transform/Distort/Perspective category: https://docs.kdenlive.org/en/effects_and_filters/video_effects/transform_distort_perspective.html
- Kdenlive Manual — Effects and Filters: https://docs.kdenlive.org/en/effects_and_filters.html
- DaVinci Resolve 20 New Features Guide — Resolve FX Warper: https://documents.blackmagicdesign.com/SupportNotes/DaVinci_Resolve_20_New_Features_Guide.pdf
- DaVinci Resolve manual and product overview: https://www.blackmagicdesign.com/products/davinciresolve
- FFmpeg documentation: https://ffmpeg.org/ffmpeg-all.html
- GStreamer Rust bindings: https://gstreamer.freedesktop.org/documentation/rust/stable/latest/docs/gstreamer/
- wgpu documentation: https://wgpu.rs/
- GitHub Actions documentation: https://docs.github.com/en/actions
- GitHub Releases documentation: https://docs.github.com/en/repositories/releasing-projects-on-github

---

---

# 34. Aturan Produk Final — Ringkasan yang Tidak Boleh Dilupakan

1. Workspace tabs ada di bar paling atas: Edit, Effects, Color, Audio, Titles & Motion, Export, dan dapat ditambah/custom.
2. Setiap workspace menyimpan layout sendiri dan dapat di-reset satu tab atau semuanya.
3. Semua editor areas modular: resize, split, move, swap, tabs, maximize, floating windows, redock.
4. Shift + Hover/Focus memberikan penjelasan fitur secara kontekstual.
5. Media dapat dipreview sebelum masuk timeline.
6. Track bersifat universal; jangan mengunci track sebagai audio-only/video-only.
7. Video+audio dapat dibuat sebagai komponen terpisah yang linked; bisa unlink dan relink.
8. Group/Ungroup berbeda dari Compound Clip/Open/Decompose.
9. Timeline memiliki playhead, snap toggle, Alt temporary snap, Razor click-to-cut, Split at Playhead, ESC keluar dari tool.
10. Inspector menyediakan Transform, Adjustments, Effects, Mask, Speed, Color, Audio, Text, Shape, dan Warp sesuai objek.
11. Mask wajib mendukung shape, pen/Bezier, freehand brush, feather, smooth edge, expansion/contraction, dan keyframes.
12. Corners/Corner Pin wajib memiliki empat handle independen; Mesh/Freeform Warp harus bisa menambah/menghapus dan menggerakkan titik tambahan.
13. Warp dapat diterapkan pada video, image, text, shapes, dan visual objects lain yang kompatibel—bukan audio-only.
14. Shape creation memiliki primitive dan custom paths; shape juga mendukung keyframes.
15. Sebagian besar parameter yang relevan harus keyframable, tetapi capability ditentukan per parameter, bukan dipaksakan pada semua properti.
16. Keyframe dapat diedit di timeline; Graph Editor terpisah menampilkan channel yang dipilih dan mempunyai easing presets serta custom curves.
17. Color workspace mencakup adjustments, wheels, curves, qualifiers/windows, LUT, dan scopes.
18. Text memiliki animation presets seperti Typewriter, reveal, fade, slide, pop, dan kontrol manual.
19. Pengguna dapat membuat custom effects/transitions melalui builder berbasis stack, parameters, masks, shapes, warp, dan keyframes. Fusion/node compositor belum masuk scope.
20. Plugin Manager, Plugin SDK, Plugin Creator, validation, compatibility, dan dokumentasi developer termasuk roadmap produk.
21. Dokumentasi lengkap adalah requirement, bukan tambahan opsional.
22. AI tidak menjadi fitur aplikasi.
23. Optimasi meliputi proxy, adaptive preview, cache, hardware decode/encode, GPU processing, zero-copy bila didukung, resource budgets, dan background tasks.
24. Desktop Windows lebih dahulu; mobile merupakan rencana kemudian.
25. GitHub Actions membangun dan menguji executable; ZIP dan installer `.exe` dibuat sebagai release assets.
26. Updater membaca GitHub Releases/tag, membandingkan SemVer, menampilkan release notes, memverifikasi checksum/signature, meminta persetujuan, dan punya rollback strategy.
27. AI coding agent harus membaca SSOT dan file status, menjaga requirement, menulis tests/docs, dan melaporkan hasil secara jujur.
28. Tidak ada klaim fitur selesai, playback lancar, plugin aman, codec didukung, atau update berhasil tanpa pengujian nyata.
29. Effects, Transitions, Generators, Shapes/Graphics, 2D Visual Composition, dan Presets memiliki fungsi serta browser/kategori yang jelas; jangan menggabungkannya menjadi satu kategori yang membingungkan.
30. 2D Visual Composition dan 2D procedural generators termasuk scope; 2.5D motion graphics mendukung XYZ/depth, extrusion text/shape, perspective camera, basic materials/light/shadow, dan keyframes. Full 3D scene compositing/modeling tetap di luar scope awal.
31. Universal Preset Library mendukung efek tunggal/stack, transisi, generator, shape/style, komposisi 2D, 2.5D, text/style/animation, mask, warp, color, audio chain, keyframe curve, export profile, dan workspace layout; preset tetap editable serta memiliki dependency/version handling.
32. Desain visual dan HTML prototype yang disediakan pemilik proyek menjadi acuan tampilan. Master Spec menjelaskan perilaku dan acceptance, bukan narasi estetika panjang atau redesign mandiri.

**Dokumen ini adalah baseline scope. Perubahan selanjutnya harus dicatat, bukan hanya disebut di percakapan.**
