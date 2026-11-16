# Spesifikasi Utama — Produk dan Pengembangan Editor Video

> **Status:** Draf 0.2  
> **Peran dokumen:** Sumber kebenaran utama (SSOT) untuk cakupan produk, UI/UX, interaksi, fitur, batasan, dan aturan kerja AI coding agent.  
> **Platform awal:** Windows desktop 64-bit  
> **Teknologi inti yang direncanakan:** Rust  
> **Mobile:** Direncanakan untuk tahap mendatang, bukan cakupan rilis desktop pertama.  
> **AI di dalam aplikasi:** Tidak ada fitur AI. AI hanya boleh digunakan sebagai alat bantu pengembangan oleh pemilik proyek.

---

## 0. Cara Membaca dan Menggunakan Spesifikasi Ini

Dokumen ini adalah **sumber kebenaran seluruh requirement produk**. Semua fitur, perilaku, batasan, dan kriteria yang sudah disetujui dipertahankan di sini. Jangan membaca seluruh dokumen secara otomatis untuk setiap task.

Urutan kerja AI: baca `AGENTS.md` → periksa `PROJECT_STATUS.md` → temukan bagian task pada indeks berikut → baca bagian terkait saja. Baca `ARCHITECTURE.md` atau `DECISIONS.md` jika task menyentuh desain teknis atau keputusan yang belum final. Gunakan skill dan dokumentasi resmi hanya jika diperlukan oleh task.

Jika ada konflik, jangan menghapus requirement berdasarkan asumsi. Ikuti keputusan pemilik proyek yang paling baru dan tercatat. Untuk konflik yang belum terselesaikan, jelaskan dan minta keputusan sebelum membuat perubahan berdampak besar.

### 0.1 Prioritas Sumber Kebenaran

1. Keputusan terbaru yang secara eksplisit disetujui pemilik proyek dan tercatat di `DECISIONS.md`.
2. Spesifikasi produk dalam dokumen ini.
3. Kontrak teknis yang sudah disetujui dalam `ARCHITECTURE.md`.
4. `PROJECT_STATUS.md` untuk status implementasi dan task aktif, bukan untuk mengubah requirement.
5. Source code dan hasil tes untuk kondisi implementasi aktual.
6. Percakapan lama, asumsi, mockup, atau hasil AI yang belum disetujui bukan sumber requirement.

### 0.2 Indeks Cepat

| Task | Bagian |
|---|---:|
| Visi, platform, istilah, UI/UX | 1–2, 25 |
| Workspace, Editor Areas, Editor Type Switcher, docking | 3–4 |
| Shift Help | 5 |
| Project, media, Viewer | 6–7 |
| Timeline dan operasi clip | 8–9 |
| Inspector, effects, mask, warp, shapes | 10–13 |
| Keyframe dan Graph Editor | 14 |
| Color, audio, text/motion | 15–17 |
| Custom effects dan transitions | 18–19 |
| Export | 20 |
| Plugin | 21 |
| Performance dan stabilitas | 22–23, 26 |
| Dokumentasi produk | 24 |
| Repository, CI, release, updater | 27 |
| Alur kerja AI | `AGENTS.md` dan bagian 28 |
| Acceptance dan strategi testing | 29 |
| Roadmap | 30 |
| Penamaan dan keputusan teknis | `DECISIONS.md` |
| Referensi eksternal dan ringkasan | 33–34 |

Untuk task lintas-domain, baca hanya bagian yang berdampak langsung dan perluas pembacaan bila ditemukan dependensi nyata.

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
| Fusion / node compositor | Tidak termasuk scope saat ini |
| AI features | Tidak ada |
| User-facing limits | Hindari batas arbitrer yang tidak perlu; tetap hormati batas hardware, format, memori, dan stabilitas |

**Catatan penting:** “tanpa batasan” berarti jangan membuat pembatasan produk secara arbitrer—misalnya batas track kecil atau batas preset yang tidak perlu. Ini bukan berarti hardware, memori, format codec, atau performa tidak memiliki batas fisik.

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
| Playhead | Garis/indikator posisi waktu pemutaran di timeline. |
| Ruler | Penggaris waktu di bagian atas timeline. |
| Keyframe Lane | Baris keyframe yang ditampilkan pada/di bawah clip atau parameter di timeline. |
| Graph Editor | Editor terpisah untuk mengatur kurva interpolasi dan nilai animasi. |

---

# 3. Tab Workspace — Bar Paling Atas

**Ini adalah navigasi workspace utama**, bukan sekadar tab Media/Effects di panel samping. Posisi default ada di sisi atas aplikasi, terinspirasi dari workspace tabs Blender.

## 3.1 Workspace Bawaan

| Workspace | Tujuan | Susunan awal yang disarankan |
|---|---|---|
| Edit | Editing sehari-hari | Media Browser, Source Monitor/Viewer, Program Viewer, Inspector, Timeline |
| Effects | Mencari, menyusun, mengatur, dan membuat effects/transitions | Effects Browser, Viewer, Effect Stack/Inspector, Timeline, Keyframe Lane |
| Color | Color correction dan grading | Viewer, Color Controls, Scopes, Timeline, Color Inspector |
| Audio | Editing dan mixing audio | Timeline, Audio Mixer, Waveform/Source Monitor, Audio Inspector |
| Titles & Motion | Text, shapes, graphic elements, dan animasi | Titles/Shapes Library, Viewer, Inspector, Timeline, Keyframe/Graph Editor |
| Export | Setup output dan render jobs | Export Settings, Render Queue, Preview, Job Details |

`Plugin Development` dapat disediakan sebagai workspace tambahan/opsional untuk developer, bukan workspace utama untuk editor biasa.

## 3.2 Pengelolaan Workspace

| ID | Fitur | Persyaratan |
|---|---|---|
| WS-01 | Workspace Tabs Top Bar | Semua workspace utama tampil sebagai tab di bagian atas. |
| WS-02 | Default Workspaces | Edit, Effects, Color, Audio, Titles & Motion, Export. |
| WS-03 | Add Workspace | Pengguna dapat menambahkan workspace baru. |
| WS-04 | Rename Workspace | Pengguna dapat mengganti nama workspace custom. |
| WS-05 | Duplicate Workspace | Menggandakan workspace termasuk susunan area. |
| WS-06 | Delete Custom Workspace | Menghapus workspace custom dengan konfirmasi. |
| WS-07 | Reorder Tabs | Mengubah urutan workspace tabs. |
| WS-08 | Save Per-Workspace Layout | Setiap tab menyimpan layout independen. |
| WS-09 | Reset Current Workspace | Mengembalikan hanya workspace/tab aktif ke layout default-nya. |
| WS-10 | Reset One Workspace | Memilih satu workspace untuk di-reset meskipun tidak sedang aktif. |
| WS-11 | Reset All Workspaces | Mengembalikan semua workspace ke layout default. Wajib ada konfirmasi. |
| WS-12 | Workspace Presets | Menyediakan layout standar dan custom presets. |
| WS-13 | Workspace Import/Export | Menyimpan dan memindahkan layout custom. |
| WS-14 | Remember Last Workspace | Membuka workspace terakhir yang digunakan. |
| WS-15 | Workspace Recovery | Memulihkan workspace/layout jika aplikasi crash. |
| WS-16 | Workspace Search | Mencari editor/panel dan menempatkannya di workspace. |

Reset layout tidak boleh menghapus project, media, effect presets, plugin, atau user data lainnya.

---

# 4. Area Editor Modular dan Sistem Docking

Setiap workspace memiliki editor areas yang dapat diubah susunannya.

| ID | Fitur | Persyaratan |
|---|---|---|
| UI-01 | Resizable Areas | Area dapat diperbesar/diperkecil dengan drag divider. |
| UI-02 | Horizontal Split | Membagi area secara horizontal. |
| UI-03 | Vertical Split | Membagi area secara vertikal. |
| UI-04 | Move Area | Memindahkan area ke posisi lain. |
| UI-05 | Swap Editors | Menukar isi dua area. |
| UI-06 | Tabbed Editors | Beberapa editor dapat berada pada area yang sama dalam bentuk tab. |
| UI-07 | Floating Editor Window | Editor dapat dilepas menjadi window terpisah. |
| UI-08 | Redock | Floating window dapat digabungkan kembali. |
| UI-09 | Dock Preview | Menampilkan target docking sebelum dilepas. |
| UI-10 | Maximize Area | Memperbesar satu area memenuhi ruang workspace. |
| UI-11 | Restore Area | Mengembalikan ukuran dan susunan sebelumnya. |
| UI-12 | Multi-monitor | Floating editor dapat dipindah ke monitor lain. |
| UI-13 | Fullscreen Editor | Editor dapat memenuhi layar. |
| UI-14 | Hide/Show Area | Menyembunyikan dan menampilkan area. |
| UI-15 | Save Layout | Menyimpan layout untuk workspace aktif. |
| UI-16 | Reset Layout | Reset layout tanpa mengubah data project. |
| UI-17 | Responsive Areas | Area menyesuaikan ukuran jendela dan ruang tersedia. |
| UI-18 | UI Scale | Mengubah skala interface. |
| UI-19 | Focus Area | Area aktif jelas dan menerima shortcut yang sesuai. |
| UI-20 | Keyboard Navigation | Area dan tab dapat diakses melalui keyboard. |

## 4.1 Jenis editor yang dapat ditempatkan di area

- Media Browser / Project Bin
- Source Monitor
- Program Viewer / Video Preview
- Timeline
- Inspector
- Effects Browser
- Transitions Browser
- Titles & Shapes Browser
- Keyframe Timeline / Keyframe Lane
- Graph Editor
- Color Editor
- Video Scopes
- Audio Mixer
- Audio Waveform Editor
- Render Queue
- Performance Monitor
- Plugin Manager
- Plugin Development Tools
- Documentation / Help Browser
- Project Settings
- History / Undo History

## 4.2 Default layout pada workspace Edit

Default awal (bukan susunan permanen):

- **Left area:** Media, Effects, Transitions, Titles/Shapes dapat dibuka sebagai tab atau editor yang ditukar.
- **Center area:** Viewer/Preview utama.
- **Right area:** Inspector yang mengikuti objek terpilih.
- **Bottom area:** Timeline.
- **Opsional:** Source Monitor dapat berbagi tab dengan Viewer atau ditempatkan di area terpisah.

Susunan harus bisa di-customize tanpa memindahkan project data.

---


## 4.3 Editor Type Switcher — Requirement Wajib

Setiap **Editor Area** memiliki kontrol **Editor Type Switcher** pada header area, mengikuti pola interaksi Blender. Switcher mengganti jenis editor di area yang sama; bukan Workspace Tab dan bukan aksi menambahkan editor sebagai tab.

- Menampilkan ikon dan nama jenis editor aktif.
- Membuka daftar editor yang tersedia; daftar panjang sebaiknya dapat dicari dan dikelompokkan.
- Memilih jenis editor mengganti konten area yang sama tanpa membuat area baru atau berpindah workspace.
- Contoh: Program Viewer → Graph Editor; Timeline → Audio Mixer; Media Browser → Effects Browser.
- Jika area berisi beberapa editor bertab, switcher mengganti tipe editor yang aktif. Menambah editor sebagai tab tetap menjadi aksi terpisah.
- Pergantian editor tidak menghapus project data; layout tetap tersimpan per workspace.
- Dapat dioperasikan dengan mouse dan keyboard serta memiliki tooltip, focus state, dan Shift Help.

| ID | Fitur | Persyaratan |
|---|---|---|
| UI-21 | Editor Type Switcher | Mengganti jenis editor di area aktif yang sama; berbeda dari Workspace Tabs dan Tabbed Editors. |

# 5. Bantuan Kontekstual Shift dan Kemudahan Menemukan Fitur

Pengguna meminta cara menahan Shift pada fitur/tombol untuk mengetahui penjelasan lebih lanjut.

| ID | Fitur | Persyaratan |
|---|---|---|
| HELP-01 | Shift + Hover | Ketika Shift ditahan dan pointer berada di kontrol, tampilkan bantuan kontekstual. |
| HELP-02 | Short Description | Penjelasan singkat fungsi kontrol. |
| HELP-03 | Detailed Help | Cara penggunaan dan contoh. |
| HELP-04 | Parameter Explanation | Arti parameter, satuan, dan efek perubahan nilai. |
| HELP-05 | Shortcut Hint | Shortcut yang berlaku. |
| HELP-06 | Related Controls | Tautan ke fitur terkait. |
| HELP-07 | Pinned Help | Bantuan dapat dibuka di panel Help terpisah. |
| HELP-08 | Keyboard Accessibility | Help dapat dipanggil saat kontrol difokuskan melalui keyboard. |
| HELP-09 | Touch Alternative | Tombol/info icon alternatif untuk perangkat tanpa keyboard. |

Aturan interaksi:

- Shift + Hover/Focus adalah perilaku utama.
- Jangan gunakan Shift + Click sebagai mekanisme utama karena Shift + Click biasa dipakai untuk multi-selection.
- Bantuan tidak boleh menutupi kontrol secara permanen atau menghalangi drag.
- Tooltip biasa tetap boleh tampil untuk label singkat.

---

# 6. Pengelolaan Proyek dan Media

| ID | Fitur | Persyaratan |
|---|---|---|
| MED-01 | Project Manager | Membuat, membuka, menyalin, mengganti nama, dan mengelola project. |
| MED-02 | New Project | Membuat project baru. |
| MED-03 | Project Settings | Resolusi, aspect ratio, frame rate, timebase, audio sample rate, color management. |
| MED-04 | Import Media | Mengimpor video, audio, gambar, image sequence, dan aset pendukung yang didukung. |
| MED-05 | Drag & Drop Import | Mengimpor melalui drag and drop. |
| MED-06 | Media Browser | Menampilkan semua media project. |
| MED-07 | Media Bins | Mengelompokkan media dalam folder. |
| MED-08 | Nested Bins | Folder bersarang. |
| MED-09 | Search Media | Pencarian nama, metadata, dan tag. |
| MED-10 | Filter Media | Filter berdasarkan video/audio/image/sequence/jenis lain. |
| MED-11 | Sort Media | Urut nama, tanggal, durasi, ukuran, jenis. |
| MED-12 | Thumbnail Generator | Membuat thumbnail otomatis. |
| MED-13 | Hover Scrubbing | Scrub preview dengan menggerakkan pointer di thumbnail. |
| MED-14 | Metadata | Codec, resolusi, FPS, durasi, ukuran, channel audio, color information jika tersedia. |
| MED-15 | Media Preview | Preview media sebelum masuk timeline. |
| MED-16 | Source Monitor | Memutar sumber terpisah dari hasil timeline. |
| MED-17 | Source In/Out | Memilih bagian sumber yang akan digunakan. |
| MED-18 | Audio Waveform Generation | Membuat waveform. |
| MED-19 | Relink Media | Menghubungkan ulang media yang hilang atau dipindah. |
| MED-20 | Offline Media Indicator | Menandai media yang tidak ditemukan. |
| MED-21 | Proxy Generation | Membuat media proxy beresolusi/bitrate lebih ringan. |
| MED-22 | Proxy Toggle | Mengaktifkan/menonaktifkan proxy. |
| MED-23 | Proxy Quality | Memilih kualitas proxy. |
| MED-24 | Proxy Batch Creation | Membuat proxy beberapa file sekaligus. |
| MED-25 | Replace Media | Mengganti sumber dengan mempertahankan edit yang kompatibel. |
| MED-26 | Media Usage | Menemukan semua penggunaan media dalam project/timeline. |
| MED-27 | Image Sequence Import | Mengimpor image sequence sebagai footage. |
| MED-28 | Media Labels/Tags | Label dan tag custom untuk aset. |

### 6.1 Persyaratan Preview Media

- Pengguna dapat memilih media di browser dan memutarnya sebelum menaruhnya di timeline.
- Preview sumber tidak mengubah playhead timeline utama kecuali pengguna memilih aksi khusus untuk itu.
- Pengguna dapat menetapkan In/Out pada source monitor.
- Drag dari media browser ke timeline harus membuat clip dari sumber yang dipilih.
- Video yang memiliki audio dapat ditambahkan sebagai linked video/audio components sesuai pengaturan insert.

---

# 7. Viewer / Pratinjau Video

| ID | Fitur | Persyaratan |
|---|---|---|
| PRE-01 | Program Preview | Menampilkan hasil akhir timeline pada posisi playhead. |
| PRE-02 | Play/Pause | Kontrol playback. |
| PRE-03 | Frame Step | Maju/mundur satu frame. |
| PRE-04 | Scrubbing | Menggeser playhead. |
| PRE-05 | Timecode | Menampilkan posisi waktu/frame. |
| PRE-06 | Preview Resolution | Full, Half, Quarter, Auto atau tingkat sepadan. |
| PRE-07 | Fullscreen Preview | Viewer fullscreen. |
| PRE-08 | Viewer Zoom | Zoom in/out. |
| PRE-09 | Fit to Viewer | Menyesuaikan gambar dengan ruang viewer. |
| PRE-10 | Safe Areas | Action/title safe guides. |
| PRE-11 | Grid and Guides | Grid, ruler, guide lines. |
| PRE-12 | Before/After | Perbandingan sebelum/sesudah efek atau grading. |
| PRE-13 | Interactive Overlays | Handle transform, mask, shape, corner pin, mesh warp, text. |
| PRE-14 | Loop Playback | Mengulang rentang tertentu. |
| PRE-15 | Dropped Frame Indicator | Menunjukkan playback frame drop. |
| PRE-16 | Screenshot/Still | Mengambil still frame dari viewer. |
| PRE-17 | Viewer Color Management | Memperlihatkan preview dengan transformasi warna yang sesuai setting. |
| PRE-18 | Overlay Visibility | Show/hide handles, guides, mask overlays. |

---

# 8. Timeline dan Sistem Track Universal

## 8.1 Track Universal

Tidak ada kategori track permanen yang hanya menerima video atau audio. Setiap track adalah lane yang dapat menerima item yang kompatibel.

**Tipe item yang dapat ditempatkan:**

- Video clip
- Audio clip
- Still image
- Text/title
- Shape/vector graphic
- Compound clip
- Nested sequence/timeline ketika fitur itu sudah tersedia
- Adjustment/effect layer jika implementasinya mendukung

Audio dan visual tetap diproses oleh jalur engine masing-masing walaupun berbagi konsep track yang universal. Urutan visual track menentukan urutan compositing; seluruh item audio masuk ke audio mixer sesuai routing-nya.

| ID | Fitur | Persyaratan |
|---|---|---|
| TL-01 | Universal Track | Track menerima jenis item yang kompatibel, bukan dikunci sebagai audio/video-only. |
| TL-02 | No Arbitrary Track Cap | Tidak menetapkan batas jumlah track kecil secara arbitrer; performa tetap mengikuti hardware. |
| TL-03 | Add/Delete Track | Menambah dan menghapus track. |
| TL-04 | Rename Track | Mengganti nama. |
| TL-05 | Reorder Track | Mengubah urutan. |
| TL-06 | Resize Track | Mengatur tinggi track. |
| TL-07 | Collapse Track | Mengecilkan track. |
| TL-08 | Lock Track | Mencegah perubahan pada track. |
| TL-09 | Hide Visual Output | Menyembunyikan keluaran visual track. |
| TL-10 | Mute Audio | Mute semua audio pada track. |
| TL-11 | Solo Track | Isolasi keluaran audio/visual yang sesuai. |
| TL-12 | Track Selection | Memilih satu atau banyak track. |
| TL-13 | Track Color Labels | Warna label. |
| TL-14 | Track Compositing | Mode compositing/opacity track bila relevan. |
| TL-15 | Track Effects | Efek pada seluruh konten track. |
| TL-16 | Track Height Presets | Tinggi track compact/normal/expanded. |
| TL-17 | Track Grouping | Mengelompokkan track untuk manajemen. |

## 8.2 Komponen Audio-Video yang Terhubung

Saat video dengan audio ditambahkan, aplikasi harus dapat membuat dua komponen terpisah di timeline—video component dan audio component—yang saling terhubung.

| ID | Fitur | Persyaratan |
|---|---|---|
| TL-18 | Auto A/V Split | Memisahkan video dan embedded audio menjadi dua komponen ketika mode insert yang sesuai digunakan. |
| TL-19 | Linked Indicator | Menunjukkan kedua komponen masih terhubung. |
| TL-20 | Linked Movement | Menggeser satu komponen ikut menggeser pasangannya. |
| TL-21 | Linked Selection | Memilih pasangan bersama-sama. |
| TL-22 | Linked Trim | Trim/split yang dipilih dapat diterapkan sinkron sesuai operasi. |
| TL-23 | Unlink | Memutus hubungan agar audio dan video dapat diubah terpisah. |
| TL-24 | Relink | Menyambungkan kembali komponen. |
| TL-25 | Independent Editing | Setelah unlink, komponen bisa dipindah, dipotong, dan diedit sendiri. |
| TL-26 | Insert Placement | Pengguna bebas menempatkan komponen ke track universal mana pun. |
| TL-27 | A/V Sync Protection | Memberi peringatan atau tindakan pemulihan saat operasi dapat menyebabkan desync yang tidak disengaja. |

Audio dan video yang ditambahkan secara terpisah tidak boleh otomatis ditautkan kecuali pengguna memilih Link atau operasi sinkronisasi.

## 8.3 Navigasi Timeline

| ID | Fitur | Persyaratan |
|---|---|---|
| TL-28 | Timeline Ruler | Penggaris waktu. |
| TL-29 | Playhead | Garis/indikator posisi pemutaran. |
| TL-30 | Timecode | Waktu dan frame yang akurat. |
| TL-31 | Frame Navigation | Bergerak frame per frame. |
| TL-32 | Scrubbing | Mencari posisi dengan drag playhead. |
| TL-33 | Timeline Zoom | Zoom horizontal. |
| TL-34 | Horizontal Scroll | Scroll timeline secara horizontal. |
| TL-35 | Vertical Scroll | Scroll antar track. |
| TL-36 | Fit Timeline | Fit seluruh konten ke area timeline. |
| TL-37 | Center Playhead | Menjaga playhead di tengah jika diaktifkan. |
| TL-38 | Loop Region | Loop rentang tertentu. |
| TL-39 | Markers | Marker di timeline. |
| TL-40 | Marker Notes | Catatan pada marker. |
| TL-41 | In/Out Range | Rentang kerja/render. |
| TL-42 | Timeline Selection | Seleksi clip, gap, range, dan track. |

## 8.4 Snap / Penempelan Otomatis

| ID | Fitur | Persyaratan |
|---|---|---|
| TL-43 | Snap Toggle | Tombol magnet untuk aktif/nonaktif. |
| TL-44 | Snap to Clip Edge | Snap pada tepi clip. |
| TL-45 | Snap to Playhead | Snap pada playhead. |
| TL-46 | Snap to Marker | Snap pada marker. |
| TL-47 | Snap to Keyframe | Snap pada keyframe. |
| TL-48 | Snap to Frame | Penempatan mengikuti grid frame/timebase. |
| TL-49 | Temporary Snap Modifier | Alt menyalakan snap sementara saat di-drag apabila snap global mati; saat Alt dilepas, kembali ke kondisi global sebelumnya. |

Tidak boleh terjadi perilaku membingungkan ketika Alt ditekan saat snap global sudah aktif. Aturan state harus dijelaskan melalui tooltip.

## 8.5 Alat Editing

| ID | Tool/Feature | Requirement |
|---|---|---|
| TL-50 | Selection Tool | Memilih, memindah, dan resize clip. |
| TL-51 | Razor/Blade Tool | Klik clip untuk memotong pada posisi pointer. |
| TL-52 | Split at Playhead | Memotong pada posisi playhead. |
| TL-53 | Split Selected Clips | Memotong seluruh clip terpilih. |
| TL-54 | Split Linked Components | Menjaga perilaku linked A/V saat split sesuai opsi. |
| TL-55 | Multi-track Razor | Memotong beberapa track pada posisi yang sama. |
| TL-56 | Ripple Edit | Mengubah durasi dan menggeser konten setelahnya. |
| TL-57 | Roll Edit | Mengubah edit point dua clip yang bersebelahan. |
| TL-58 | Slip Edit | Mengubah bagian sumber tanpa memindahkan posisi clip. |
| TL-59 | Slide Edit | Memindahkan clip dengan menyesuaikan clip tetangganya. |
| TL-60 | Spacer Tool | Menggeser konten untuk mengatur ruang. |
| TL-61 | Gap Detection | Mendeteksi ruang kosong. |
| TL-62 | Close Gap | Menutup gap. |
| TL-63 | Ripple Delete | Hapus clip dan tutup gap. |
| TL-64 | Normal Delete | Hapus tanpa menggeser item lain. |
| TL-65 | Insert Mode | Menyisipkan clip dan menggeser konten setelahnya. |
| TL-66 | Overwrite Mode | Menimpa bagian timeline yang dituju. |
| TL-67 | Tool Exit | ESC keluar dari tool sementara dan kembali ke Selection Tool jika aman. |
| TL-68 | Tool Shortcuts | Shortcut dapat dikustomisasi. |

**Cut Tool behavior:** pilih Razor/Blade → klik pada clip di posisi target → clip terbelah. ESC keluar dari tool dan kembali ke Selection Tool. Split at Playhead juga tersedia tanpa mengaktifkan Razor.

---

# 9. Operasi Clip, Grouping, dan Compound Clip

Group dan Compound Clip adalah dua fitur berbeda dan harus dipertahankan terpisah.

| ID | Fitur | Persyaratan |
|---|---|---|
| CLP-01 | Group Clips | Mengelompokkan beberapa clip agar dapat dipilih/dipindahkan bersama, tetapi tetap individual. |
| CLP-02 | Ungroup | Memisahkan group tanpa menghapus clip. |
| CLP-03 | Create Compound Clip | Membungkus item terpilih menjadi satu item timeline. |
| CLP-04 | Open Compound Clip | Membuka isi compound clip untuk diedit. |
| CLP-05 | Decompose/Unpack | Mengembalikan isi compound ke timeline sebagai item individual dengan posisi dan timing terjaga. |
| CLP-06 | Nested Timeline | Timeline di dalam timeline, fase lanjutan. |
| CLP-07 | Copy/Paste | Salin dan tempel clip. |
| CLP-08 | Duplicate | Menggandakan clip. |
| CLP-09 | Delete | Menghapus clip. |
| CLP-10 | Disable | Menonaktifkan clip sementara tanpa menghapusnya. |
| CLP-11 | Rename | Mengganti nama clip. |
| CLP-12 | Clip Color | Warna identifikasi. |
| CLP-13 | Copy/Paste Attributes | Menyalin pengaturan antar clip. |
| CLP-14 | Replace Clip | Mengganti media source dengan mempertahankan edit kompatibel. |
| CLP-15 | Reverse | Memutar terbalik. |
| CLP-16 | Freeze Frame | Membuat frame diam. |
| CLP-17 | Speed Control | Mengubah kecepatan. |
| CLP-18 | Speed Ramping | Mengubah kecepatan sepanjang clip. |
| CLP-19 | Duration Control | Mengatur durasi. |
| CLP-20 | Frame Hold | Menahan frame selama interval. |
| CLP-21 | Clip Markers | Marker internal clip. |
| CLP-22 | Clip Notes | Catatan pada clip. |

---

# 10. Inspector, Parameter, dan Tumpukan Efek

Inspector berada di area yang bisa dipindahkan/diganti, bukan area permanen yang tidak dapat dikustomisasi. Kontennya berubah berdasarkan objek/parameter yang dipilih.

## 10.1 Kategori Inspector

- Transform
- Compositing / Opacity / Blend
- Adjustments
- Effects
- Mask
- Corners / Warp
- Speed / Time Remapping
- Color
- Audio
- Text
- Shape
- Keyframes
- Metadata / Clip Properties

## 10.2 Transformasi dan Penyesuaian Dasar

| ID | Fitur | Parameter |
|---|---|---|
| TRF-01 | Position | X, Y |
| TRF-02 | Scale | X, Y, uniform/non-uniform |
| TRF-03 | Rotation | Derajat |
| TRF-04 | Anchor/Pivot | Titik pusat transformasi |
| TRF-05 | Crop | Left, right, top, bottom |
| TRF-06 | Flip | Horizontal, vertical |
| TRF-07 | Opacity | 0–100% atau ekuivalen |
| TRF-08 | Blend Modes | Normal, Multiply, Screen, Overlay dan mode standar lain yang diimplementasikan |
| TRF-09 | Perspective | Perspektif dasar |
| TRF-10 | Reset Transform | Reset parameter transform |
| TRF-11 | Transform Keyframes | Keyframe pada parameter yang dapat dianimasikan |

## 10.3 Sistem Efek

| ID | Fitur | Persyaratan |
|---|---|---|
| EFX-01 | Effects Browser | Perpustakaan efek. |
| EFX-02 | Effect Categories | Kategori dan subkategori. |
| EFX-03 | Search Effects | Pencarian cepat. |
| EFX-04 | Favorites | Menandai efek favorit. |
| EFX-05 | Drag & Drop | Drag efek ke clip/track/layer yang kompatibel. |
| EFX-06 | Effect Stack | Menampilkan efek yang diterapkan dan urutannya. |
| EFX-07 | Reorder Stack | Mengubah urutan efek. Urutan harus memengaruhi hasil sesuai pipeline. |
| EFX-08 | Enable/Disable | Toggle efek. |
| EFX-09 | Remove Effect | Menghapus efek. |
| EFX-10 | Presets | Menyimpan kombinasi parameter sebagai preset. |
| EFX-11 | Copy/Paste Effects | Menyalin efek dan setting. |
| EFX-12 | Effect Keyframes | Menganimasikan parameter yang mendukung keyframe. |
| EFX-13 | Adjustment Layer | Menerapkan efek pada item visual di bawahnya dalam rentang yang ditentukan. |
| EFX-14 | Bypass/Before-After | Membandingkan dengan/ tanpa efek. |
| EFX-15 | Clip/Track/Master Scope | Efek dapat diterapkan pada clip, track, atau master/output bila relevan. |
| EFX-16 | Effect Zones | Efek dapat dibatasi pada rentang waktu tertentu. |
| EFX-17 | Effect Documentation | Setiap efek memiliki penjelasan parameter melalui Shift Help dan Help Docs. |

## 10.4 Daftar Efek Video Bawaan

Daftar berikut adalah baseline library, bukan batas maksimum jumlah efek.

| Kategori | Efek |
|---|---|
| Blur & Sharpen | Gaussian Blur, Directional Blur, Motion Blur, Sharpen |
| Light | Glow, Bloom, Lens Flare, Vignette |
| Image Texture | Film Grain, Noise |
| Distortion | Chromatic Aberration, Lens Distortion, Displacement, Wave/Distort |
| Pixel | Pixelate, Mosaic, Posterize |
| Keying | Chroma Key, Luma Key |
| Shadow/Edge | Shadow, Outline |
| Color | Colorize, Invert, Saturation/Contrast/Exposure adjustments |
| Stabilization | Video Stabilization |
| Compositing | Alpha, opacity, blend/composite modes |
| Geometry | Transform, Crop, Corner Pin, Mesh/Freeform Warp |

Efek berat boleh memiliki preview proxy/cache quality, tetapi parameter tetap dapat diatur dan hasil export harus menggunakan pipeline output yang ditentukan.

---

# 11. Sistem Mask

Mask adalah fitur inti, bukan fitur tambahan. Pengguna dapat menggambar langsung pada Viewer dan mengatur parameter melalui Inspector.

## 11.1 Alat Pembuatan Mask

| ID | Fitur | Persyaratan |
|---|---|---|
| MSK-01 | Rectangle Mask | Mask persegi. |
| MSK-02 | Ellipse Mask | Mask lingkaran/oval. |
| MSK-03 | Polygon Mask | Mask dari titik polygon. |
| MSK-04 | Pen Tool | Path custom. |
| MSK-05 | Bezier Mask | Kurva dengan control handles. |
| MSK-06 | Freehand Brush Mask | Menggambar mask dengan brush. |
| MSK-07 | Eraser Brush | Menghapus area sapuan. |
| MSK-08 | Add/Remove Points | Menambah atau menghapus titik path. |
| MSK-09 | Invert Mask | Membalik area terpilih. |
| MSK-10 | Multiple Masks | Banyak mask pada satu item. |
| MSK-11 | Add/Subtract/Intersect | Menggabungkan operasi mask. |
| MSK-12 | Mask on Visual Objects | Mask dapat dipakai pada video, image, text, shape, dan elemen visual yang kompatibel. |
| MSK-13 | Mask Effects | Membatasi effect ke area mask. |

## 11.2 Parameter Mask

| ID | Parameter | Requirement |
|---|---|---|
| MSK-14 | Feather | Menghaluskan tepi mask. |
| MSK-15 | Feather Radius | Mengatur lebar area feather. |
| MSK-16 | Expansion | Memperluas mask. |
| MSK-17 | Contraction | Memperkecil mask. |
| MSK-18 | Opacity | Mengatur kekuatan mask. |
| MSK-19 | Edge Smoothness | Kontrol kualitas/kehalusan tepi. |
| MSK-20 | Brush Size | Ukuran brush. |
| MSK-21 | Brush Hardness | Ketegasan tepi brush. |
| MSK-22 | Brush Flow | Intensitas sapuan. |
| MSK-23 | Brush Opacity | Opacity brush. |
| MSK-24 | Mask Overlay | Overlay warna di Viewer. |
| MSK-25 | Mask Preview | Menampilkan bentuk/alpha mask. |
| MSK-26 | Mask Presets | Menyimpan preset. |

## 11.3 Keyframe Mask

- Position
- Scale
- Rotation
- Path/control-point positions
- Feather
- Expansion/contraction
- Opacity
- Brush/path animation yang didukung

Semua keyframe mask harus dapat dipilih, digeser, diduplikasi, dihapus, dan diedit dari Keyframe Lane. Graph Editor menampilkan channel terpilih bila parameter mendukung kurva.

---

# 12. Corner Pin, Corners, dan Mesh / Freeform Warp

**Requirement tambahan wajib.** Istilah “Corners” mengacu pada efek four-corner geometry/corner pin seperti yang ada di Kdenlive. Namun kebutuhan pengguna juga mencakup penambahan control points, sehingga fitur harus memiliki mode lebih luas.

## 12.1 Mode yang Harus Tersedia

| Mode | Deskripsi |
|---|---|
| Four-Corner / Corner Pin | Empat sudut independen: top-left, top-right, bottom-right, bottom-left. Dapat membentuk perspektif, skew, dan jajar genjang. |
| Mesh Warp | Mesh/grid yang bisa ditambah subdivisinya; titik sudut, tepi, dan titik interior bisa digeser. |
| Freeform Warp | Menambahkan/menghapus titik kontrol secara bebas pada area gambar. |
| Curve Warp (advanced) | Deformasi menggunakan points dan handles/kurva bila implementasi sudah matang. |

## 12.2 Persyaratan Corners / Corner Pin

| ID | Fitur | Persyaratan |
|---|---|---|
| WRP-01 | Four Corner Handles | Empat sudut memiliki handle independen. |
| WRP-02 | Direct Viewer Editing | Handle dapat digeser langsung di Viewer. |
| WRP-03 | Numeric Coordinates | Posisi X/Y dapat dimasukkan secara numerik. |
| WRP-04 | Perspective Mapping | Mendukung perspektif/keystone. |
| WRP-05 | Bilinear Mapping | Mode interpolasi bilinear. |
| WRP-06 | Transparent Exposed Area | Area kosong dapat transparan atau menggunakan opsi border behavior. |
| WRP-07 | Corner Reset | Reset empat titik ke nilai awal. |
| WRP-08 | Keyframes | Setiap corner/control coordinate yang didukung dapat di-keyframe. |

## 12.3 Persyaratan Mesh / Freeform Warp

| ID | Fitur | Persyaratan |
|---|---|---|
| WRP-09 | Add Control Point | Pengguna dapat menambah titik kontrol. |
| WRP-10 | Remove Control Point | Pengguna dapat menghapus titik kontrol. |
| WRP-11 | Move Independent Point | Setiap titik dapat dipindahkan tanpa otomatis menggeser titik lain, kecuali mode soft influence dipilih. |
| WRP-12 | Add Mesh Subdivision | Menambah baris/kolom subdivisi mesh. |
| WRP-13 | Move Edge Nodes | Titik di bagian tepi dapat digerakkan. |
| WRP-14 | Move Interior Nodes | Titik di dalam mesh dapat digerakkan. |
| WRP-15 | Influence Radius | Mengatur area pengaruh titik bila mode soft warp digunakan. |
| WRP-16 | Interpolation Modes | Pilihan interpolasi seperti bilinear/bicubic/smooth yang didukung engine. |
| WRP-17 | Mesh Overlay | Menampilkan/menyembunyikan grid dan handles. |
| WRP-18 | Soft/Sharp Warp | Mengatur karakter pengaruh warp bila tersedia. |
| WRP-19 | Edge Handling | Menentukan perlakuan area yang keluar dari batas gambar. |
| WRP-20 | Reset Mesh | Mengembalikan mesh. |
| WRP-21 | Save Warp Preset | Menyimpan geometri sebagai preset bila sesuai. |
| WRP-22 | Warp Keyframes | Keyframe titik dan parameter deformasi. |

## 12.4 Kompatibilitas Objek

Corner Pin dan Warp dapat diterapkan pada elemen visual yang kompatibel:

- Video clips
- Still images
- Text/title layers
- Shapes/vector graphics
- Graphic overlays
- Compound visual compositions
- Mask paths bila model path mendukung transformasi tersebut

Warp tidak diterapkan langsung pada audio-only item karena tidak memiliki geometri visual. Audio tetap berjalan normal jika merupakan komponen pasangan video yang di-warp.

## 12.5 Aturan Topologi Keyframe

- Perpindahan titik warp dapat dianimasikan.
- Untuk interpolasi antar-keyframe yang stabil, struktur mesh/topology sebaiknya konsisten.
- Jika jumlah titik/struktur mesh berubah setelah ada keyframe, aplikasi harus memberi aturan yang jelas: propagate perubahan ke keyframe terkait, membuat keyframe baru yang kompatibel, atau memperingatkan bahwa interpolasi tidak tersedia.
- Jangan membuat interpolasi bentuk yang tidak valid secara diam-diam.

---

# 13. Sistem Pembuatan Shape

Shape adalah objek visual tersendiri dan dapat ditempatkan di timeline, diberi effect, mask, transform, warp, dan keyframe.

| ID | Fitur | Persyaratan |
|---|---|---|
| SHP-01 | Rectangle | Bentuk persegi. |
| SHP-02 | Rounded Rectangle | Persegi sudut membulat. |
| SHP-03 | Ellipse/Circle | Lingkaran/oval. |
| SHP-04 | Line | Garis. |
| SHP-05 | Polygon | Polygon. |
| SHP-06 | Star | Bintang. |
| SHP-07 | Arrow | Panah. |
| SHP-08 | Freehand Path | Gambar bebas. |
| SHP-09 | Pen Tool | Path custom. |
| SHP-10 | Bezier Curves | Control handles untuk kurva. |
| SHP-11 | Edit Path Points | Edit node/path. |
| SHP-12 | Add/Remove Nodes | Tambah/hapus node. |
| SHP-13 | Fill Color | Warna isi. |
| SHP-14 | Gradient Fill | Gradient. |
| SHP-15 | Stroke | Garis tepi. |
| SHP-16 | Stroke Width | Ketebalan. |
| SHP-17 | Stroke Style | Solid, dashed, dan variasi lain yang didukung. |
| SHP-18 | Corner Radius | Kelengkungan sudut. |
| SHP-19 | Shadow | Bayangan. |
| SHP-20 | Opacity | Transparansi. |
| SHP-21 | Shape Presets | Menyimpan dan menggunakan preset. |
| SHP-22 | Group Shapes | Group beberapa shape. |
| SHP-23 | Transform | Position, scale, rotation, anchor. |
| SHP-24 | Shape Warp | Corner Pin/Mesh Warp pada shape. |

## 13.1 Keyframe Shape

Shape creation **wajib mendukung keyframes** untuk parameter yang relevan:

- Position, scale, rotation, anchor/pivot
- Opacity
- Fill color/gradient stops bila didukung
- Stroke color/width/opacity
- Corner radius
- Path control-point positions
- Shape-specific parameters (misalnya jumlah titik star bila modelnya mendukung)

Animasi perubahan path harus memperhatikan kesesuaian jumlah dan urutan nodes antar-keyframe. Bila struktur tidak kompatibel, jangan menghasilkan interpolasi yang rusak tanpa peringatan.

---

# 14. Sistem Keyframe dan Graph Editor

Sebagian besar parameter yang bermakna untuk animasi harus mendukung keyframe, tetapi **tidak semua properti wajib keyframable**. Setiap parameter harus mendeklarasikan capability-nya secara eksplisit.

## 14.1 Sistem yang Mendukung Keyframe

- Transform: position, scale, rotation, anchor, opacity.
- Effects: parameter efek yang cocok untuk animasi.
- Color: parameter koreksi/grading yang dipilih untuk keyframing.
- Mask: posisi, path, feather, expansion, opacity, dan parameter relevan.
- Corner Pin/Mesh Warp: corner/control-point positions dan parameter warp.
- Shapes: transform, opacity, path, fill/stroke dan parameter bentuk yang mendukung.
- Text: transform, opacity, text-animation parameters, character/word reveal progress.
- Speed/time remapping: speed curve dan retiming points.
- Audio: volume, pan, effect parameters, automation.
- Transitions: progress/duration/parameter transisi.

## 14.2 Kontrol Keyframe

| ID | Fitur | Persyaratan |
|---|---|---|
| KF-01 | Keyframe Toggle | Mengaktifkan animasi pada parameter. |
| KF-02 | Add Keyframe | Menambah keyframe di posisi playhead. |
| KF-03 | Delete Keyframe | Menghapus keyframe terpilih. |
| KF-04 | Move Keyframe | Memindahkan waktu/nilai keyframe. |
| KF-05 | Duplicate Keyframe | Menggandakan keyframe. |
| KF-06 | Copy/Paste | Salin dan tempel keyframe. |
| KF-07 | Multi-select | Memilih banyak keyframe. |
| KF-08 | Multi-move | Memindahkan banyak keyframe. |
| KF-09 | Previous/Next Keyframe | Berpindah antar keyframe. |
| KF-10 | Channel Lanes | Menampilkan per-channel. |
| KF-11 | Keyframe Lane | Menampilkan marker keyframe di timeline/di bawah clip. |
| KF-12 | Show/Hide Lanes | Mengatur visibilitas keyframe lanes, dengan shortcut customizable. |
| KF-13 | Snap to Keyframes | Snap saat mengedit keyframe. |
| KF-14 | Keyframe Value Editing | Mengubah nilai numerik. |
| KF-15 | Interpolation Type | Mengubah interpolasi. |

## 14.3 Graph Editor

Graph Editor adalah **editor terpisah**, bukan harus selalu tampil di dalam setiap clip.

| ID | Fitur | Persyaratan |
|---|---|---|
| KF-16 | Separate Graph Editor | Dapat ditempatkan pada editor area mana pun. |
| KF-17 | Selected Channel Only | Default hanya menampilkan parameter/channel yang dipilih. |
| KF-18 | Multiple Channels Optional | Pengguna dapat menambahkan channel lain secara manual. |
| KF-19 | Graph Point Editing | Menggeser titik graph. |
| KF-20 | Bezier Handles | Mengatur handle kurva. |
| KF-21 | Linear | Interpolasi linear. |
| KF-22 | Constant/Hold | Menahan nilai hingga keyframe berikutnya. |
| KF-23 | Ease In | Ease in preset. |
| KF-24 | Ease Out | Ease out preset. |
| KF-25 | Ease In & Out | Ease in/out. |
| KF-26 | Smooth | Preset smooth. |
| KF-27 | Auto Bezier | Handle otomatis. |
| KF-28 | Custom Bezier | Kurva manual. |
| KF-29 | Graph Zoom/Pan | Zoom dan pan. |
| KF-30 | Graph Presets | Menyimpan preset kurva/interpolasi. |
| KF-31 | Graph Reset | Reset kurva ke nilai bawaan. |
| KF-32 | Show/Hide Graph Editor | Dapat ditampilkan/disembunyikan lewat shortcut customizable. |
| KF-33 | Graph Value Scale | Skala waktu dan nilai yang jelas. |
| KF-34 | Keyframe Tangents | Pengaturan tangent. |

**Keputusan UX:**

- Keyframe Lane tersedia langsung di timeline untuk manipulasi cepat.
- Pengguna dapat memilih, menghapus, menggandakan, dan memindahkan keyframe dari lane.
- Graph Editor dibuka terpisah untuk pengeditan kurva yang detail.
- Graph Editor otomatis fokus pada parameter yang dipilih.
- Tidak memaksa semua graph tampil sekaligus.
- Graph Editor dapat di-dock, di-float, di-resize, dan dipindah seperti editor lain.
- Shortcut show/hide harus bisa dikustomisasi.

---

# 15. Koreksi Warna dan Color Grading

Color workspace merupakan workspace bawaan tersendiri di top bar.

## 15.1 Penyesuaian Dasar

| ID | Adjustment |
|---|---|
| COL-01 | Exposure |
| COL-02 | Brightness |
| COL-03 | Contrast |
| COL-04 | Pivot |
| COL-05 | Highlights |
| COL-06 | Shadows |
| COL-07 | Whites |
| COL-08 | Blacks |
| COL-09 | Saturation |
| COL-10 | Temperature |
| COL-11 | Tint |
| COL-12 | Vibrance |
| COL-13 | Midtone Detail |
| COL-14 | Color Boost |
| COL-15 | Reset Color |

## 15.2 Color Wheels

- Lift
- Gamma
- Gain
- Offset
- Log Wheels
- HDR Wheels
- Luminance controls
- Saturation controls
- Numeric controls
- Keyframe pada parameter yang mendukung animasi

## 15.3 Curves

- Custom RGB Curves
- Red Curve
- Green Curve
- Blue Curve
- Luma Curve
- Hue vs Hue
- Hue vs Saturation
- Hue vs Luminance
- Luminance vs Saturation
- Saturation vs Saturation
- Add/delete curve points
- Reset selected curve
- Keyframe pada parameter kurva yang dirancang untuk animasi

## 15.4 Alat Warna Lanjutan

- RGB Mixer
- HSL Qualifier
- Color Isolation
- Color Warper
- Color Windows / Power Windows
- Color Tracking (advanced)
- LUT Import / Apply / Strength
- Color Presets
- Copy/Paste Grade
- Before/After
- Color Management / Color Space transforms
- HDR grading controls

## 15.5 Video Scopes

- Waveform
- RGB Parade
- Vectorscope
- Histogram
- Scope zoom
- Multi-scope layout
- Scope visibility and dockability

---

# 16. Editing dan Mixing Audio

| ID | Fitur | Persyaratan |
|---|---|---|
| AUD-01 | Audio Preview | Preview audio sumber. |
| AUD-02 | Waveform | Menampilkan waveform. |
| AUD-03 | Clip Volume | Volume per clip. |
| AUD-04 | Volume Keyframes | Automasi volume. |
| AUD-05 | Pan | Posisi stereo. |
| AUD-06 | Fade In/Out | Audio fades. |
| AUD-07 | Trim/Split | Potong audio. |
| AUD-08 | Audio Mixer | Mixing multi-track. |
| AUD-09 | Track Fader | Volume per track. |
| AUD-10 | Master Bus | Master output. |
| AUD-11 | EQ | Equalizer. |
| AUD-12 | Compressor | Dynamic range compression. |
| AUD-13 | Limiter | Peak limiting. |
| AUD-14 | Noise Gate | Threshold gate. |
| AUD-15 | DSP Noise Reduction | Noise reduction non-AI. |
| AUD-16 | Reverb | Reverb. |
| AUD-17 | Delay | Delay. |
| AUD-18 | De-esser | Mengurangi sibilance. |
| AUD-19 | Pitch | Pitch adjustment. |
| AUD-20 | Stereo Enhancement | Mengatur stereo width. |
| AUD-21 | Audio Bus/Submix | Routing dan grouping audio. |
| AUD-22 | Loudness Meter | Pengukuran loudness. |
| AUD-23 | Peak Meter | Peak monitoring. |
| AUD-24 | Manual Ducking | Ducking melalui automation/keyframes, bukan AI. |
| AUD-25 | Channel Control | Mono/stereo/multichannel handling. |
| AUD-26 | Voice-over Recording | Recording voice-over. |
| AUD-27 | Audio Plugin Support | Dukungan plugin audio yang kompatibel. |

---

# 17. Title, Teks, dan Animasi Gerak

Text harus menjadi objek timeline yang dapat diatur seperti objek visual lain.

## 17.1 Kontrol Teks

- Text editor
- Font family dan custom fonts
- Font size, weight, style
- Letter spacing dan line spacing
- Alignment
- Text color dan gradient
- Outline/stroke
- Shadow
- Background/box
- Padding dan corner radius untuk text box
- Position, scale, rotation, anchor
- Opacity dan blend mode
- Text presets
- Lower thirds
- Title templates
- Custom title presets
- Text clipping/mask/shape interaction
- Corner Pin dan Mesh Warp untuk text

## 17.2 Sistem Animasi Teks

| Animation | Perilaku |
|---|---|
| Typewriter | Huruf muncul satu per satu seperti sedang diketik. |
| Character Reveal | Reveal per karakter. |
| Word Reveal | Reveal per kata. |
| Line Reveal | Reveal per baris. |
| Fade In/Out | Opacity transition. |
| Slide In/Out | Bergerak dari arah tertentu. |
| Scale/Pop | Perubahan skala/pop. |
| Tracking Animation | Perubahan letter spacing. |
| Blur Reveal | Blur berkurang saat teks muncul. |
| Mask/Wipe Reveal | Teks muncul melalui mask/wipe. |
| Bounce | Gerakan pantul dengan preset kurva. |
| Rotate | Animasi rotasi. |
| Stagger | Delay antar karakter/kata/baris. |

Pengaturan Typewriter dan reveal:

- Kecepatan/durasi
- Character/word/line mode
- Start/end range
- Direction/reverse
- Delay/stagger
- Optional cursor/caret untuk gaya typewriter
- Easing/interpolation
- Keyframe progress dan parameter animasi
- Save as animation preset

Semua animasi text harus tetap dapat diedit. Preset tidak boleh mengunci teks menjadi video hasil render kecuali pengguna memilih bake/render option.

## 17.3 Subtitle

- Subtitle Editor
- Manual subtitle creation
- SRT import/export
- VTT import/export
- ASS/SSA support bila kompatibel
- Subtitle timing
- Font, size, color, outline, background, placement
- Subtitle track/timeline objects
- Burn-in subtitles on export

**Tidak ada AI auto-caption atau speech-to-text.**

---

# 18. Pembuat Efek dan Transisi Kustom

Requirement penting: pengguna tidak boleh hanya memakai daftar efek/transisi bawaan. Mereka perlu dapat merakit, mengubah, menyimpan, dan membagikan efek/transisi sendiri langsung dari editor.

Ini **bukan Fusion** dan tidak memerlukan node compositor pada tahap ini. Sistem awal berbasis effect stack, parameter, layer, mask, shape, warp, dan keyframe.

## 18.1 Pembuat Efek Kustom

- Membuat custom effect dari rangkaian built-in effects.
- Menambah/menghapus/reorder efek dalam stack.
- Mengatur parameter setiap efek.
- Menggunakan mask untuk membatasi efek.
- Menggunakan blend/compositing modes.
- Mengatur keyframe dan easing.
- Menyimpan kombinasi menjadi custom effect preset.
- Menamai, memberi deskripsi, kategori, thumbnail, dan tags.
- Mengatur default values dan exposed controls.
- Preview pada sample clip dan clip project.
- Mengubah preset tanpa merusak efek bawaan.
- Export/import preset.

## 18.2 Pembuat Transisi Kustom

- Membuat transisi dari dua keadaan visual/clip.
- Mengatur durasi dan alignment.
- Menggunakan opacity, transform, mask, shape, blur, color, warp, dan built-in effects.
- Mengatur keyframe start/end dan kurva.
- Mengatur arah, timing, easing, dan overlap.
- Mendukung audio crossfade untuk pasangan audio yang relevan.
- Preview A/B pada dua clip.
- Menyimpan menjadi custom transition preset.
- Import/export preset.

## 18.3 Pustaka Preset Kustom

- Kategori, pencarian, favorit, tags.
- Duplicate, rename, delete, export, import.
- Versioning preset jika parameter berubah.
- Dependency check untuk preset yang memakai plugin.
- Pesan yang jelas jika plugin/efek dependency tidak tersedia.

---

# 19. Pustaka Transisi

Baseline transition set:

- Cross Dissolve
- Fade In
- Fade Out
- Dip to Black
- Dip to White
- Dip to Color
- Wipe
- Slide
- Push
- Zoom
- Blur
- Iris
- Audio Crossfade
- Custom Transition

Setiap transition mendukung durasi, alignment, parameter, preview, preset, dan keyframe bila parameter tersebut mendukung animasi.

---

# 20. Render, Export, dan Hasil Akhir

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

# 21. Sistem Plugin dan Pembuatan Plugin

Aplikasi perlu mendukung penggunaan plugin dan menyediakan workflow untuk membuat plugin. Plugin system adalah extensibility layer, bukan sekadar halaman daftar plugin.

## 21.1 Jenis Plugin

- Video effects
- Transitions
- Audio effects/processors
- Titles/motion presets
- Generators (misalnya shape/pattern/graphic generator)
- Importers/exporters
- Custom panels/editors/workspace tools
- Custom masks/tools bila API mendukung
- Metadata/utility tools

## 21.2 Pengelola Plugin

- Browse installed plugins
- Install/uninstall
- Enable/disable
- Plugin version and author
- Compatibility status
- Dependency detection
- Update notification
- Plugin permissions/capabilities
- Plugin logs/errors
- Safe mode (start app with third-party plugins disabled)
- Load/reload plugin while developing when supported

## 21.3 Pembuat Plugin / Alat Developer

- New Plugin wizard
- Plugin template selection by plugin type
- Plugin ID/name/version/author/description/license
- SDK/API reference
- Parameter declaration and UI controls
- Preview/test harness with sample media
- Development reload/hot reload where technically safe
- Debug logs and validation
- Build/package plugin
- Compatibility validation
- Example plugins and starter templates
- Plugin documentation generator or template

## 21.4 SDK Plugin dan Kompatibilitas

- Define a stable, versioned Plugin API.
- Prefer a sandboxed/cross-platform interface such as WebAssembly/WIT for suitable extensions, subject to technical validation.
- GPU custom effects may later expose a controlled shader interface such as WGSL, with validation and resource limits.
- Native plugins require strict ABI/version strategy; do not casually expose unstable internal Rust ABI.
- Third-party plugins must not be allowed to corrupt project files or access unrestricted resources without clear permission.
- Plugins must declare required app/API version and supported platform.
- Plugin failure should not crash the entire editor where isolation is possible.
- Document performance expectations and plugin lifecycle.

**Keputusan arsitektur plugin harus diuji melalui technical spike sebelum dijadikan kontrak permanen.** Jangan mengklaim plugin cross-platform atau sandbox aman sebelum diuji.

---

# 22. Performa dan Pengelolaan Sumber Daya

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

### 22.1 Aturan Performa

- Jangan berasumsi Rust otomatis membuat playback lancar.
- Jangan mengasumsikan semua efek dapat berjalan real-time pada 8 GB RAM/iGPU.
- Jangan memindahkan setiap frame GPU→RAM→GPU tanpa alasan.
- Shared memory iGPU tetap dihitung sebagai bagian penggunaan memori sistem.
- Proxy dan cache tidak boleh mengubah kualitas export final.
- Playback boleh menurunkan kualitas preview; export tetap memakai source dan setting final.
- UI harus tetap responsif saat proxy, waveform, thumbnail, atau export berjalan.

### 22.2 Skenario Benchmark

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

# 23. Alur Kerja, Stabilitas, dan Produktivitas

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
- Daftar efek/transisi bawaan.
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



**Arahan visual tambahan:** Dark Theme menggunakan dasar charcoal/graphite netral dengan beberapa tingkat surface. Warna aksen final belum diputuskan; gunakan aksen secara terarah dan jangan menjadikan neon teal/cyan atau glow berlebihan sebagai dominasi tanpa persetujuan. Viewer dan Timeline tetap menjadi fokus hierarki visual. Ikon harus dirender sebagai ikon/SVG sebenarnya; tooltip dan Shift Help tidak boleh menampilkan markup SVG mentah.

# 26. File Proyek, Penyimpanan Persisten, dan Keamanan Data

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

# 27. Repository GitHub, CI, ZIP, EXE, dan Sistem Pembaruan

Pengembangan dilakukan melalui repository GitHub. AI coding agent mengubah project, lalu GitHub Actions menjalankan pemeriksaan dan build. Setiap handoff yang diminta harus dapat diberikan sebagai ZIP, dan release Windows menghasilkan executable/package.

## 27.1 Hasil Build yang Diharapkan

1. **Source Project ZIP:** source code, dokumentasi, config, workflow GitHub Actions, dan file build yang dibutuhkan; tidak perlu menyertakan cache/build output besar yang bisa dihasilkan ulang.
2. **Windows Release ZIP:** aplikasi portable dan resource yang diperlukan, jika mode portable disediakan.
3. **Windows Installer `.exe`:** installer yang dibuat oleh CI/release workflow.
4. **Checksums:** SHA-256 untuk setiap asset rilis.
5. **Release Notes:** changelog dan instruksi upgrade.

Pastikan artefak ZIP tidak berisi secret, token, signing key, cache pribadi, file temporary, atau media pribadi.

## 27.2 Pipeline GitHub Actions

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

## 27.3 Pengelolaan Versi

Gunakan Semantic Versioning:

- `MAJOR`: perubahan besar yang tidak kompatibel.
- `MINOR`: fitur baru yang kompatibel.
- `PATCH`: bug fix/perbaikan kompatibel.

Tag release harus konsisten dengan version metadata aplikasi. Jangan membuat nomor versi berbeda antara UI aplikasi, Git tag, GitHub Release, installer, dan changelog.

## 27.4 Updater di Dalam Aplikasi

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

## 27.5 Metadata Repository

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

# 28. Aturan Pengembangan dengan AI

AI digunakan sebagai alat bantu pengembangan, bukan fitur di dalam aplikasi. Aturan operasional coding agent berada di `AGENTS.md` agar instruksi kerja hanya memiliki satu sumber.

Batasan produk yang tetap berlaku:

- Jangan menambah fitur AI internal.
- Jangan mengurangi atau menghapus requirement tanpa persetujuan pemilik proyek.
- Jangan memperlakukan prototype/mock sebagai bukti engine produksi.
- Jangan mengklaim codec, GPU acceleration, playback, render/export, plugin sandbox, atau updater berhasil tanpa pengujian nyata.
- Fusion/node compositor dan 3D compositing tetap di luar scope saat ini.

# 29. Strategi Pengujian dan Kriteria Penerimaan

## 29.1 Kriteria Penerimaan Prototipe UI Interaktif

Prototype UI harus interaktif. Setidaknya:

1. Workspace tabs di atas dapat dipindah/dipilih.
2. Workspace dapat ditambah, di-duplicate, di-rename, di-reset satu per satu, dan reset semua.
3. Editor area dapat resize, split, swap, tab, maximize, float, dan redock.
4. Layout berubah responsive saat ukuran jendela berubah.
5. Shift + Hover menampilkan contextual help.
6. Media dapat di-import atau menggunakan sample assets yang diberi label.
7. Media dapat di-preview sebelum masuk timeline.
8. Drag media ke timeline membuat clip.
9. Universal tracks menerima video/audio/image/text/shape item sesuai jenis.
10. Video dengan audio dapat dipisah sebagai linked components.
11. Linked movement dan unlink/relink dapat diuji.
12. Group/Ungroup dan Compound Clip/Open/Decompose dapat diuji sebagai operasi berbeda.
13. Playhead, ruler, snap, Alt temporary snap, Razor, Split at Playhead, dan ESC dapat diuji.
14. Inspector berubah sesuai item yang dipilih.
15. Mask dapat dibuat/diubah di Viewer.
16. Corner Pin memiliki empat handle yang dapat digeser.
17. Warp prototype menyediakan titik tambahan atau mesh nodes yang bisa dipindah.
18. Shapes dapat dibuat, diubah, dan memiliki keyframe controls.
19. Text dapat dibuat dan memilih Typewriter/animation preset.
20. Keyframe dapat ditambah, dihapus, dipindah, dan dipilih.
21. Graph Editor menampilkan channel yang dipilih dan preset easing dapat mengubah kurva.
22. Color controls dan scopes memiliki interaksi UI.
23. Effect stack dapat menambah, menghapus, reorder dan edit parameter secara interaktif.
24. Custom Effect/Transition Builder dapat menyusun preset dari elemen yang tersedia.
25. Plugin Creator dapat membuat sample plugin project/manifest atau wizard prototype.
26. Export settings dan render queue dapat diuji sebagai UI.
27. Dark/Light theme berfungsi.
28. Tidak ada kontrol yang tampak aktif tetapi tidak melakukan apa pun tanpa status/penjelasan.

**Prototype limitation:** UI prototype boleh memakai playback sample terbatas atau simulasi untuk menguji interaksi. Jangan menyebutnya sebagai engine editing/rendering final. Kinerja codec, playback semua format, GPU acceleration, export, dan plugin execution harus diuji pada engine sesungguhnya.

## 29.2 Kriteria Penerimaan Editor Inti

- Editing operasi dasar frame-accurate.
- Audio-video sync tidak berubah secara tidak sengaja.
- Group dan Compound Clip memberi hasil yang berbeda sesuai definisi.
- Undo/redo mencakup semua operasi timeline yang didukung.
- Keyframe value dan interpolation konsisten.
- Mask, corner pin, mesh warp, shapes, dan text dapat dipreview.
- Export output dapat diputar dan sesuai resolusi/FPS/codec yang dipilih.
- Crash recovery memulihkan project sejauh mungkin tanpa merusak source project.
- Project schema migration diuji dengan fixture versi sebelumnya.

## 29.3 Kriteria Penerimaan Rilis

- CI lulus.
- Windows build berhasil.
- ZIP dan `.exe` tersedia.
- SHA-256 dibuat dan diverifikasi.
- Git tag, app version, GitHub Release, dan changelog konsisten.
- Updater membaca latest release dan membandingkan SemVer.
- Update gagal tidak membuat aplikasi rusak.
- Dokumentasi versi sesuai release.

---



## 29.4 Kriteria Penerimaan UI Desktop Lengkap

Mempertahankan seluruh 56 kriteria penerimaan UI desktop. ID menggunakan namespace `UI-AC` agar tidak bertabrakan dengan workflow prototype.

| ID | Workflow | Kriteria penerimaan |
|---|---|---|
| UI-AC-01 | Workspace tabs | Edit, Effects, Color, Audio, Titles & Motion, Export dapat dipilih; tab aktif jelas. |
| UI-AC-02 | Add workspace | Workspace custom dapat dibuat dan diberi nama. |
| UI-AC-03 | Rename/duplicate/reorder | Workspace custom dapat diubah nama, digandakan, dan diurutkan. |
| UI-AC-04 | Delete workspace | Workspace custom dihapus dengan konfirmasi; project data tidak berubah. |
| UI-AC-05 | Layout per workspace | Layout independen tersimpan; reset satu workspace tidak mengubah lainnya. |
| UI-AC-06 | Reset all | Semua workspace kembali default setelah konfirmasi; project/media tetap aman. |
| UI-AC-07 | Editor Type Switcher | Switcher di header mengganti jenis editor pada area yang sama. |
| UI-AC-08 | Bedakan switcher/workspace | Editor switcher tidak berpindah workspace; Workspace Tab tidak mengganti editor area. |
| UI-AC-09 | Bedakan switcher/tabbed editor | Ganti editor dan menambah editor sebagai tab adalah aksi berbeda. |
| UI-AC-10 | Split/resize | Area dapat di-split horizontal/vertikal dan divider di-resize. |
| UI-AC-11 | Move/swap/tabbed | Editor dapat dipindah, ditukar, dan dikelompokkan sebagai tab. |
| UI-AC-12 | Maximize/restore | Area dapat dimaksimalkan dan dikembalikan. |
| UI-AC-13 | Float/redock | Editor dapat menjadi floating window dan kembali dock tanpa kehilangan state. |
| UI-AC-14 | Multi-monitor | Floating editor dapat dipulihkan bila monitor target tidak tersedia. |
| UI-AC-15 | Responsive | Kontrol penting tetap terbaca/terjangkau saat jendela berubah ukuran. |
| UI-AC-16 | Keyboard navigation | Tabs, switcher, fokus area dan shortcut dapat digunakan dengan keyboard. |
| UI-AC-17 | Shift Help | Shift + Hover/Focus menampilkan fungsi, penggunaan, parameter, shortcut. |
| UI-AC-18 | Icon rendering | Ikon dirender benar; markup SVG mentah tidak pernah tampil sebagai teks. |
| UI-AC-19 | Import media | Video, audio, gambar, image sequence dikenali sesuai dukungan. |
| UI-AC-20 | Bins/search/filter | Bin, nested bin, search, filter dan sort berfungsi. |
| UI-AC-21 | Source preview | Media dipreview tanpa mengubah Program Viewer playhead. |
| UI-AC-22 | Source In/Out | In/Out ditetapkan dan bagian terpilih digunakan saat insert. |
| UI-AC-23 | Drag to timeline | Drag membuat clip pada track/posisi target. |
| UI-AC-24 | Relink/offline | Media hilang ditandai dan dapat direlink. |
| UI-AC-25 | Proxy | Proxy status, toggle, quality, batch workflow berfungsi sesuai implementasi. |
| UI-AC-26 | Universal Track | Item kompatibel dapat ditempatkan pada track universal. |
| UI-AC-27 | Linked A/V | Embedded video/audio tampil linked dan bergerak sinkron. |
| UI-AC-28 | Unlink/relink | Komponen dapat diedit terpisah dan disambungkan kembali. |
| UI-AC-29 | Group/ungroup | Group menjaga clip individual, posisi dan isi. |
| UI-AC-30 | Compound clip | Create/open/edit/decompose menjaga timing, track, isi. |
| UI-AC-31 | Playhead/scrub | Timecode dan preview mengikuti playhead. |
| UI-AC-32 | Snap | Snap global dan target yang ditentukan berfungsi. |
| UI-AC-33 | Alt temporary snap | Alt mengaktifkan snap sementara ketika global snap nonaktif. |
| UI-AC-34 | Razor/Split | Razor memotong di pointer; Split at Playhead di playhead. |
| UI-AC-35 | ESC | ESC keluar dari active tool ke Selection tanpa membatalkan edit. |
| UI-AC-36 | Ripple/roll/slip/slide | Tool berbeda dan sesuai perilaku yang ditetapkan. |
| UI-AC-37 | Undo/redo | Operasi timeline dapat dibatalkan/dipulihkan. |
| UI-AC-38 | Inspector context | Inspector berubah sesuai video/audio/text/shape terpilih. |
| UI-AC-39 | Effect stack | Add, reorder, bypass, enable/disable, remove berfungsi. |
| UI-AC-40 | Mask creation | Rectangle, ellipse, polygon, Pen/Bezier, brush, eraser dapat digunakan. |
| UI-AC-41 | Mask parameters | Feather, expansion, smoothing, opacity, brush size/hardness/flow memengaruhi mask. |
| UI-AC-42 | Corner Pin | Empat corner dapat digeser secara independen. |
| UI-AC-43 | Mesh Warp | Titik tambahan dapat dibuat, dihapus, dan digeser secara independen. |
| UI-AC-44 | Warp compatibility | Warp berlaku pada objek visual kompatibel, bukan audio-only. |
| UI-AC-45 | Shape creation | Primitive dan custom path dapat dibuat serta diedit. |
| UI-AC-46 | Keyframe lane | Keyframe dapat ditambah, dipilih, digeser, disalin, dihapus. |
| UI-AC-47 | Graph Editor | Channel terpilih, curve, points, easing dan handles dapat diedit. |
| UI-AC-48 | Color controls | Adjustment, wheels, curves, qualifiers/windows, LUT, scopes dapat diuji. |
| UI-AC-49 | Text animation | Typewriter/reveal/motion dapat dipreview dan dikontrol. |
| UI-AC-50 | Audio editing | Waveform, trim, fade, volume, pan, keyframe, mixer berfungsi. |
| UI-AC-51 | Audio processing | EQ, dynamics, DSP effects memiliki status/aksi nyata. |
| UI-AC-52 | Export settings | Resolution, FPS, codec/container, quality, range dapat disetel. |
| UI-AC-53 | Render queue | Add/reorder/cancel/progress/error state sesuai implementasi. |
| UI-AC-54 | Theme | Dark/Light konsisten pada editor, menu, popup, tooltip dan overlay. |
| UI-AC-55 | Autosave/recovery | Project dan layout dipulihkan sesuai kebijakan penyimpanan. |
| UI-AC-56 | Performance feedback | Dropped frames/resource state hanya ditampilkan jika datanya aktual. |

### Data uji

Sediakan satu video dengan embedded audio, satu audio terpisah, dua gambar, satu title/text, beberapa shape, clip untuk Group/Compound Clip, dan keyframe Position/Opacity/Mask/Shape/Warp. Asset harus tersedia lokal dan legal.

### Kriteria keluar

Jalankan workflow prioritas, catat hasil, pastikan kontrol inti tidak inert, layout tersimpan/dipulihkan, simulasi diberi label, dan temuan UX diselesaikan atau dicatat. Pemilihan framework final dilakukan berdasarkan uji nyata dan dicatat di `DECISIONS.md`.

## 29.5 Workflow Prototipe UI Interaktif

Mempertahankan 44 workflow prototype, data uji, batas simulasi, prinsip interaksi, serta kriteria keluar. ID menggunakan namespace `UI-PROT`.

### 1. Batasan prototipe

Prototipe harus interaktif. Pengguna dapat mengklik, memilih, menggeser, resize, drag-and-drop, membuat titik, mengubah parameter, dan berpindah workspace.

Prototipe boleh memakai:

- Media contoh yang disertakan.
- Data timeline simulasi.
- State lokal sementara.
- Preview media melalui komponen yang didukung framework prototype.
- Efek visual sederhana untuk menunjukkan hasil UI.

Prototipe **tidak boleh mengaku** sudah memiliki decoding semua codec, GPU acceleration, export produksi, render cache produksi, audio engine final, atau plugin runtime yang aman. Semua simulasi harus diberi label di dokumentasi dan laporan.

### 2. Struktur tampilan

#### Bar paling atas

- Project name / menu project.
- Workspace tabs: Edit, Effects, Color, Audio, Titles & Motion, Export.
- Tombol tambah workspace.
- Pengaturan workspace dan Reset.
- Area status proyek dan pencarian perintah.

#### Ruang kerja

- Default Edit: Media/Effects/Transitions di kiri, Viewer di tengah, Inspector di kanan, Timeline di bawah.
- Setiap bagian adalah editor area yang dapat diganti, split, resize, dipindah, dijadikan tab, maximize, floating, dan redock.
- Setiap workspace menyimpan layout independen.

#### Tema dan responsivitas

- Dark dan Light theme.
- Layout harus diuji pada 1366×768, 1920×1080, dan 2560×1440.
- Ketika area terlalu sempit, tampilkan mode compact, tab, atau drawer. Jangan sekadar mengecilkan semua kontrol.

### 3. Alur pengujian wajib

| ID | Alur | Langkah uji | Kriteria lulus |
|---|---|---|---|
| UI-PROT-01 | Workspace tabs | Berpindah Edit, Effects, Color, Audio, Titles & Motion, Export. | Tab aktif jelas dan layout masing-masing dipulihkan. |
| UI-PROT-02 | Tambah workspace | Buat workspace baru dan beri nama. | Workspace muncul di bar atas dan tersimpan. |
| UI-PROT-03 | Reset satu workspace | Ubah layout Edit, pindah ke Color, reset Edit. | Hanya layout Edit yang kembali default. |
| UI-PROT-04 | Reset semua | Ubah beberapa workspace lalu reset seluruhnya. | Semua layout kembali default setelah konfirmasi; project tidak berubah. |
| UI-PROT-05 | Split editor area | Split area horizontal dan vertikal. | Dua area aktif dapat digunakan dan ukurannya bisa diubah. |
| UI-PROT-06 | Ganti jenis editor | Ubah sebuah area menjadi Media, Timeline, Graph Editor, atau Audio Mixer. | Isi area berganti tanpa mengubah data project. |
| UI-PROT-07 | Tabbed editors | Letakkan beberapa editor dalam satu area. | Tab editor bisa dipilih dan dipindah. |
| UI-PROT-08 | Floating editor | Lepaskan Viewer/Graph Editor ke window terpisah. | Window dapat dipindah dan tidak kehilangan state. |
| UI-PROT-09 | Redock | Kembalikan floating window ke area target. | Editor kembali bergabung tanpa kehilangan konfigurasi. |
| UI-PROT-10 | Maximize/restore | Maksimalkan satu area dan pulihkan. | Layout sebelumnya tetap utuh. |
| UI-PROT-11 | Responsive | Ubah ukuran jendela dari lebar ke sempit. | Panel beradaptasi dan kontrol penting tetap dapat dijangkau. |
| UI-PROT-12 | Shift Help | Tahan Shift pada tombol/parameter. | Penjelasan fungsi, parameter, dan shortcut tampil. |
| UI-PROT-13 | Import media | Tambahkan file video, audio, dan gambar contoh. | Aset terlihat dengan tipe dan status yang benar. |
| UI-PROT-14 | Preview sebelum timeline | Pilih media dari browser dan putar. | Preview sumber tidak mengubah timeline utama. |
| UI-PROT-15 | Drag ke timeline | Seret media dari browser ke track. | Clip dibuat pada posisi/track target. |
| UI-PROT-16 | Linked A/V | Tambahkan video yang memiliki audio. | Komponen video dan audio muncul terpisah tetapi linked. |
| UI-PROT-17 | Unlink/relink | Putus dan sambungkan ulang link. | Setelah unlink, komponen dapat dipindah terpisah. |
| UI-PROT-18 | Group/Ungroup | Pilih beberapa clip lalu group dan ungroup. | Clip tetap individual dan posisinya tidak berubah. |
| UI-PROT-19 | Compound Clip | Buat, buka, edit isi, lalu decompose. | Isi dan timing kembali tanpa kehilangan item. |
| UI-PROT-20 | Playhead/scrub | Geser playhead dan scrub timeline. | Frame/timecode mengikuti posisi. |
| UI-PROT-21 | Snap | Aktifkan/nonaktifkan magnet dan pindahkan clip dekat tepi clip. | Snap hanya bekerja saat aktif. |
| UI-PROT-22 | Alt temporary snap | Saat snap global nonaktif, tahan Alt saat drag. | Snap aktif sementara dan kembali nonaktif saat Alt dilepas. |
| UI-PROT-23 | Razor | Aktifkan Razor dan klik clip. | Clip terbelah di lokasi pointer. |
| UI-PROT-24 | Split at Playhead | Tempatkan playhead dan jalankan perintah Split. | Clip yang sesuai terpotong di playhead. |
| UI-PROT-25 | ESC | Aktifkan Razor lalu tekan ESC. | Kembali ke Selection Tool tanpa menghapus hasil edit. |
| UI-PROT-26 | Inspector context | Pilih video, audio, teks, dan shape. | Inspector menampilkan kategori yang relevan. |
| UI-PROT-27 | Effect stack | Tambah, susun ulang, bypass, dan hapus efek. | Urutan dan status efek berubah secara visual. |
| UI-PROT-28 | Mask shape | Buat rectangle/ellipse/polygon mask di Viewer. | Handle dapat dipilih dan digeser. |
| UI-PROT-29 | Mask pen/Bezier | Tambah node dan handle kurva. | Path dapat diubah. |
| UI-PROT-30 | Mask brush | Gambar dan hapus sapuan. | Brush size/hardness/flow memengaruhi overlay mask. |
| UI-PROT-31 | Mask feather | Ubah feather dan edge smoothing. | Tepi mask berubah pada preview. |
| UI-PROT-32 | Corner Pin | Geser empat corner secara independen. | Bentuk perspektif berubah dan setiap corner dapat dikontrol. |
| UI-PROT-33 | Mesh Warp | Tambah/hapus subdivisi atau titik. | Titik baru dapat digeser secara independen. |
| UI-PROT-34 | Warp objek | Terapkan warp pada video, image, text, dan shape. | Semua objek visual uji dapat berubah bentuk. |
| UI-PROT-35 | Shape creation | Buat rectangle, ellipse, star, arrow, dan path custom. | Bentuk muncul sebagai objek visual dan dapat diubah. |
| UI-PROT-36 | Shape keyframes | Atur dua keyframe position/path/opacity. | Nilai antara dapat dipreview atau disimulasikan secara konsisten. |
| UI-PROT-37 | Keyframe lane | Tambah, pilih, geser, salin, dan hapus keyframe. | Posisi dan nilai tampil akurat. |
| UI-PROT-38 | Graph Editor | Pilih channel dari Inspector/clip. | Hanya channel terpilih tampil secara default. |
| UI-PROT-39 | Easing | Terapkan Linear, Ease In, Ease Out, Smooth, dan Bezier. | Bentuk kurva berubah dan pilihan terbaca. |
| UI-PROT-40 | Color controls | Ubah exposure, wheels, curves, dan LUT placeholder. | Kontrol mengubah parameter dan preview demo. |
| UI-PROT-41 | Text animation | Tambahkan teks dan pilih Typewriter/reveal. | Preview menunjukkan tahapan animasi atau progresnya. |
| UI-PROT-42 | Export UI | Ubah resolusi, FPS, codec pilihan, dan rentang. | Ringkasan export diperbarui. |
| UI-PROT-43 | Render queue | Tambah job, ubah urutan, cancel job simulasi. | Status dan progres simulasi konsisten, diberi label demo. |
| UI-PROT-44 | Tema | Berpindah Dark/Light. | Semua editor dan popup mengikuti tema. |

### 4. Data contoh

Sediakan project demo dengan:

- Satu video dengan audio embedded.
- Satu file audio terpisah.
- Dua gambar.
- Satu title/text.
- Beberapa shape.
- Beberapa clip yang bisa digunakan untuk group dan compound clip.
- Keyframe contoh pada Position, Opacity, Mask, Shape, dan Warp.

Jangan bergantung pada file sample eksternal yang tidak disertakan atau tidak memiliki lisensi penggunaan yang jelas.

### 5. Prinsip interaksi

- Drag-and-drop harus memiliki indikator target yang jelas.
- Operasi berbahaya seperti menghapus workspace, menghapus media dari project, atau Reset All harus memerlukan konfirmasi yang tepat.
- Undo/Redo harus tersedia untuk operasi yang mengubah data project.
- Selection, focus, hover, active, disabled, loading, dan error state harus berbeda secara visual.
- Shortcut tidak boleh bentrok secara diam-diam. Tampilkan shortcut di menu dan sediakan editor shortcut.
- Klik kanan membuka context menu sesuai objek.
- Help Shift tidak boleh mengganggu drag atau painting.

### 6. Kriteria keluar dari tahap prototype

- Semua alur P0 pada tabel sudah dijalankan.
- Tidak ada kontrol inti yang tidak memiliki respons.
- Data layout dan state project dapat disimpan dan dipulihkan.
- Semua fitur yang masih simulasi terdokumentasi.
- Temuan usability dicatat dan diselesaikan atau dibuat sebagai keputusan terbuka.
- Framework UI dipilih melalui uji nyata dan keputusannya dicatat di `DECISIONS.md`.

## 29.6 Strategi Pengujian Teknis

Mempertahankan strategi unit, integration, media, render, performance, compatibility, security, dan CI testing.

### 1. Tujuan

Mencegah aplikasi terlihat berfungsi padahal workflow inti tidak benar, serta mencegah regresi pada timeline, layout, keyframe, media, dan project file.

### 2. Lapisan pengujian

| Lapisan | Tujuan | Contoh |
|---|---|---|
| Unit test | Menguji logika kecil dan deterministik. | Timebase, snap calculation, interpolation, bezier, warp geometry. |
| Integration test | Menguji beberapa modul bersama. | Linked A/V move/split, project save/load, cache invalidation. |
| UI interaction test | Menguji interaksi pengguna. | Workspace reset, docking, drag clip, Razor, Inspector context. |
| Media test | Menguji sumber media dan decoding. | Seek, frame timing, audio sync, missing media, proxy. |
| Render test | Menguji hasil output. | Durasi, resolusi, frame count, audio track, color metadata. |
| Regression test | Mencegah bug yang telah diperbaiki muncul kembali. | Satu tes otomatis untuk setiap bug penting. |
| Performance test | Mengukur penggunaan resource dan latency. | Playback FPS, dropped frames, RAM, GPU, cache, export speed. |
| Compatibility test | Menguji lingkungan dan format berbeda. | Windows versions, iGPU, codec/container, high-DPI, multi-monitor. |
| Security test | Menguji plugin, updater, dan file project. | Plugin permission, signature, malformed project, path traversal. |

### 3. Kriteria UI prototype

Kriteria interaktif dijabarkan di Bagian 29.5 dalam spesifikasi ini. Setidaknya uji:

- Workspace tabs, add/remove/reorder, reset satu dan semua.
- Area resize, split, tab, move, swap, float, redock, maximize/restore.
- Shift Help pada hover/focus.
- Media preview sebelum masuk timeline.
- Universal Track dan linked A/V.
- Group/ungroup dan compound/open/decompose.
- Playhead, snap, Alt temporary snap, Razor, Split at Playhead, ESC.
- Mask, Corner Pin, Mesh Warp, Shape, keyframe lane, Graph Editor.
- Color controls, text animation, export settings, responsive layout.

UI prototype dapat memakai data simulasi, tetapi label simulasi harus jelas dan tidak boleh dipakai sebagai bukti engine produksi.

### 4. Skenario uji media dan playback

Buat kumpulan media uji yang legal dan dapat didistribusikan. Catat codec, container, resolusi, frame rate, durasi, bit depth, audio codec, sample rate, dan ukuran berkas.

Skenario minimum:

1. 1080p H.264, 30 FPS, satu track, tanpa efek.
2. 1080p dengan beberapa track dan efek dasar.
3. Video 4K menggunakan proxy.
4. Perubahan parameter efek saat playback.
5. Scrubbing acak dan seek berulang.
6. Embedded audio plus audio terpisah.
7. Playback sambil membuat thumbnail/proxy di background.
8. Playback sambil membuka/menutup editor area dan mengubah layout.
9. Export berjalan sambil UI tetap responsif.
10. File offline, corrupt, unsupported, atau metadata tidak lengkap.

Jangan menjanjikan angka FPS universal sebelum ada benchmark perangkat tertentu. Catat perangkat, driver, versi build, media, setting, dan hasil aktual.

### 5. Target perangkat rendah

Target validasi awal adalah sistem Windows dengan RAM 8 GB dan integrated GPU, tetapi model CPU/iGPU perlu ditentukan sebelum angka performa menjadi acceptance threshold.

Wajib dicatat:

- RAM idle dan peak.
- Memori bersama iGPU jika dapat dibaca.
- CPU utilization.
- GPU utilization/engine jika dapat dibaca.
- Preview resolution.
- Codec dan hardware decode path.
- FPS playback aktual.
- Dropped frames.
- Cache size dan hit/miss jika tersedia.
- Waktu seek dan waktu memulai playback.
- Performa UI saat background tasks berjalan.

### 6. Kriteria stabilitas

- Project autosave tidak merusak project.
- Crash recovery hanya memulihkan data yang valid.
- Media asli tidak dihapus saat cache dibersihkan.
- Tidak ada kehilangan clip saat undo/redo.
- Tidak ada desync A/V tanpa peringatan atau recovery.
- Layout restore tidak menghapus project data.
- Gagal decode/export memberikan pesan yang dapat ditindaklanjuti.
- Pembaruan aplikasi dapat dipulihkan jika instalasi gagal.

### 7. Bukti pengujian

Setiap tes penerimaan perlu menyimpan:

- ID requirement.
- Versi/commit yang diuji.
- Sistem operasi dan hardware.
- Langkah reproduksi.
- Hasil yang diharapkan.
- Hasil aktual.
- Screenshot/log/video jika relevan.
- Status: lulus, gagal, diblokir, atau belum dijalankan.

### 8. Aturan CI

Pipeline minimal dapat menjalankan format check, lint, unit test, integration test, dan build pada target yang ditentukan. Tes yang memerlukan hardware tertentu dijalankan sebagai tes manual/benchmark terpisah, bukan dipalsukan menjadi tes CI universal.

# 30. Tahapan Pengembangan

Urutan berikut memisahkan validasi UI dari pengembangan engine. Fase dapat disesuaikan berdasarkan hasil pengujian, tetapi jangan melewati prasyarat teknis tanpa alasan.

| Phase | Fokus | Hasil yang harus tersedia |
|---|---|---|
| P0 | Product/UI validation | UI prototype interaktif; workspace tabs, docking, timeline interaction, inspector, mask/warp/shape/keyframe prototype, responsive. |
| P1 | Desktop shell & project model | App shell Rust, workspace persistence, project save/load, media browser, source preview. |
| P2 | Core media & timeline | Decode/playback, universal track model, clip operations, linked A/V, group/compound, audio sync. |
| P3 | Render pipeline & performance | Proxy, cache, preview quality, render queue, export, hardware paths. |
| P4 | Effects & geometry | Effect stack, built-in effects, mask, Corner Pin, Mesh/Freeform Warp, adjustment layer. |
| P5 | Animation & color | Keyframe, Graph Editor, color correction/grading, scopes, shapes, text animations. |
| P6 | Audio & custom creation | Audio mixing, custom effect builder, custom transition builder, preset library. |
| P7 | Plugin ecosystem & documentation | Plugin API/SDK, plugin creator, docs, sample plugins. |
| P8 | Hardening & release | Stability, regression tests, Windows packaging, GitHub Actions, updater, release notes. |
| P9 | Future mobile exploration | Shared core/API review and platform-specific UX research. Not part of first desktop release. |

---

# 31. Analisis Penamaan

Status nama produk, kandidat, screening awal, serta proses pemeriksaannya dipelihara di `DECISIONS.md`. Nama belum final dan setiap screening lama harus diverifikasi ulang sebelum branding atau distribusi publik.

# 32. Keputusan Teknis yang Belum Final

Satu-satunya daftar keputusan produk dan pertanyaan teknis terbuka dipelihara di `DECISIONS.md`. Jangan menyalin daftar keputusan ke file lain karena dapat menimbulkan versi yang berbeda.

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

**Dokumen ini adalah baseline scope. Perubahan selanjutnya harus dicatat, bukan hanya disebut di percakapan.**
