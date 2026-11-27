# Acceptance Criteria dan Strategi Testing
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
> **Terkait:** Spesifikasi tiap domain berada di `docs/features/`.
---## 29. Strategi Pengujian dan Kriteria Penerimaan

### 29.1 Kriteria Penerimaan Prototipe UI Interaktif

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

### 29.2 Kriteria Penerimaan Editor Inti

- Editing operasi dasar frame-accurate.
- Audio-video sync tidak berubah secara tidak sengaja.
- Group dan Compound Clip memberi hasil yang berbeda sesuai definisi.
- Undo/redo mencakup semua operasi timeline yang didukung.
- Keyframe value dan interpolation konsisten.
- Mask, corner pin, mesh warp, shapes, dan text dapat dipreview.
- Export output dapat diputar dan sesuai resolusi/FPS/codec yang dipilih.
- Crash recovery memulihkan project sejauh mungkin tanpa merusak source project.
- Project schema migration diuji dengan fixture versi sebelumnya.

### 29.3 Kriteria Penerimaan Rilis

- CI lulus.
- Windows build berhasil.
- ZIP dan `.exe` tersedia.
- SHA-256 dibuat dan diverifikasi.
- Git tag, app version, GitHub Release, dan changelog konsisten.
- Updater membaca latest release dan membandingkan SemVer.
- Update gagal tidak membuat aplikasi rusak.
- Dokumentasi versi sesuai release.

---



### 29.4 Kriteria Penerimaan UI Desktop Lengkap

Mempertahankan seluruh kriteria penerimaan UI desktop; saat ini mencakup 60 kriteria. ID menggunakan namespace `UI-AC` agar tidak bertabrakan dengan workflow prototype.

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
| UI-AC-57 | Generator authoring | Generator browser membedakan generator dari effects/transitions; parameter dan preview generator bekerja. |
| UI-AC-58 | 2D Visual Composition | Komposisi multi-layer dari shapes, text, images, video, dan generators dapat disusun, diedit, disimpan, serta dibuka kembali. |
| UI-AC-59 | 2.5D Motion Graphics | Text/shape extrusion, XYZ transform, camera/perspective, lighting dan shadow dapat diatur serta dianimasikan sesuai scope. |
| UI-AC-60 | Universal Preset Library | Preset dari kategori yang didukung dapat dibuat, dicari, diterapkan, diedit, diduplikasi, diimpor, dan diekspor; dependency dinyatakan. |

#### Data uji

Sediakan satu video dengan embedded audio, satu audio terpisah, dua gambar, satu title/text, beberapa shape, clip untuk Group/Compound Clip, dan keyframe Position/Opacity/Mask/Shape/Warp; satu generator/particle composition, satu 2D multi-layer composition, dan satu 2.5D text/shape sample. Asset harus tersedia lokal dan legal.

#### Kriteria keluar

Jalankan workflow prioritas, catat hasil, pastikan kontrol inti tidak inert, layout tersimpan/dipulihkan, simulasi diberi label, dan temuan UX diselesaikan atau dicatat. Pemilihan framework final dilakukan berdasarkan uji nyata dan dicatat di `DECISIONS.md`.

### 29.5 Workflow Prototipe UI Interaktif

Mempertahankan 44 workflow prototype, data uji, batas simulasi, prinsip interaksi, serta kriteria keluar. ID menggunakan namespace `UI-PROT`.

#### 1. Batasan prototipe

Prototipe harus interaktif. Pengguna dapat mengklik, memilih, menggeser, resize, drag-and-drop, membuat titik, mengubah parameter, dan berpindah workspace.

Prototipe boleh memakai:

- Media contoh yang disertakan.
- Data timeline simulasi.
- State lokal sementara.
- Preview media melalui komponen yang didukung framework prototype.
- Efek visual sederhana untuk menunjukkan hasil UI.

Prototipe **tidak boleh mengaku** sudah memiliki decoding semua codec, GPU acceleration, export produksi, render cache produksi, audio engine final, atau plugin runtime yang aman. Semua simulasi harus diberi label di dokumentasi dan laporan.

#### 2. Struktur tampilan

##### Bar paling atas

- Project name / menu project.
- Workspace tabs: Edit, Effects, Color, Audio, Titles & Motion, Export.
- Tombol tambah workspace.
- Pengaturan workspace dan Reset.
- Area status proyek dan pencarian perintah.

##### Ruang kerja

- Default Edit: Media/Effects/Transitions di kiri, Viewer di tengah, Inspector di kanan, Timeline di bawah.
- Setiap bagian adalah editor area yang dapat diganti, split, resize, dipindah, dijadikan tab, maximize, floating, dan redock.
- Setiap workspace menyimpan layout independen.

##### Tema dan responsivitas

- Dark dan Light theme.
- Layout harus diuji pada 1366×768, 1920×1080, dan 2560×1440.
- Ketika area terlalu sempit, tampilkan mode compact, tab, atau drawer. Jangan sekadar mengecilkan semua kontrol.

#### 3. Alur pengujian wajib

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
| UI-PROT-45 | Generator browser | Tambahkan gradient, pattern, noise, dan particle generator. | Generator memiliki parameter, preview, seed, serta output yang dapat masuk ke timeline. |
| UI-PROT-46 | 2D Visual Composition | Gabungkan shape, text, image, dan generator dalam beberapa layer; ubah urutan, mask, effect, dan keyframe. | Komposisi tetap editable dan dapat disimpan/dibuka sebagai template. |
| UI-PROT-47 | 2.5D Motion Graphics | Ubah XYZ transform, extrusion/bevel text/shape, kamera, material, dan basic light/shadow. | Preview memperlihatkan depth/perspective dan parameter dapat dianimasikan; simulasi diberi label. |
| UI-PROT-48 | Preset Library | Simpan, cari, apply, duplicate, rename, import, dan export preset dari beberapa kategori. | Preset tetap editable setelah diterapkan dan dependency yang hilang diberi peringatan. |

#### 4. Data contoh

Sediakan project demo dengan:

- Satu video dengan audio embedded.
- Satu file audio terpisah.
- Dua gambar.
- Satu title/text.
- Beberapa shape.
- Beberapa clip yang bisa digunakan untuk group dan compound clip.
- Keyframe contoh pada Position, Opacity, Mask, Shape, dan Warp.

Jangan bergantung pada file sample eksternal yang tidak disertakan atau tidak memiliki lisensi penggunaan yang jelas.

#### 5. Prinsip interaksi

- Drag-and-drop harus memiliki indikator target yang jelas.
- Operasi berbahaya seperti menghapus workspace, menghapus media dari project, atau Reset All harus memerlukan konfirmasi yang tepat.
- Undo/Redo harus tersedia untuk operasi yang mengubah data project.
- Selection, focus, hover, active, disabled, loading, dan error state harus berbeda secara visual.
- Shortcut tidak boleh bentrok secara diam-diam. Tampilkan shortcut di menu dan sediakan editor shortcut.
- Klik kanan membuka context menu sesuai objek.
- Help Shift tidak boleh mengganggu drag atau painting.

#### 6. Kriteria keluar dari tahap prototype

- Semua alur P0 pada tabel sudah dijalankan.
- Tidak ada kontrol inti yang tidak memiliki respons.
- Data layout dan state project dapat disimpan dan dipulihkan.
- Semua fitur yang masih simulasi terdokumentasi.
- Temuan usability dicatat dan diselesaikan atau dibuat sebagai keputusan terbuka.
- Framework UI dipilih melalui uji nyata dan keputusannya dicatat di `DECISIONS.md`.

### 29.6 Strategi Pengujian Teknis

Mempertahankan strategi unit, integration, media, render, performance, compatibility, security, dan CI testing.

#### 1. Tujuan

Mencegah aplikasi terlihat berfungsi padahal workflow inti tidak benar, serta mencegah regresi pada timeline, layout, keyframe, media, dan project file.

#### 2. Lapisan pengujian

| Lapisan | Tujuan | Contoh |
|---|---|---|
| Unit test | Menguji logika kecil dan deterministik. | Timebase, snap calculation, interpolation, bezier, warp geometry, particle seed determinism, 2.5D projection math. |
| Integration test | Menguji beberapa modul bersama. | Linked A/V move/split, project save/load, cache invalidation, composition save/load, preset dependency resolution. |
| UI interaction test | Menguji interaksi pengguna. | Workspace reset, docking, drag clip, Razor, Inspector context, generator/composition/preset workflows. |
| Media test | Menguji sumber media dan decoding. | Seek, frame timing, audio sync, missing media, proxy. |
| Render test | Menguji hasil output. | Durasi, resolusi, frame count, audio track, color metadata, layer compositing, reproducible particles, 2.5D extrusion/light/shadow. |
| Regression test | Mencegah bug yang telah diperbaiki muncul kembali. | Satu tes otomatis untuk setiap bug penting. |
| Performance test | Mengukur penggunaan resource dan latency. | Playback FPS, dropped frames, RAM, GPU, cache, export speed, particle count, 2D composition layers, 2.5D rendering cost. |
| Compatibility test | Menguji lingkungan dan format berbeda. | Windows versions, iGPU, codec/container, high-DPI, multi-monitor. |
| Security test | Menguji plugin, updater, dan file project. | Plugin permission, signature, malformed project, path traversal. |

#### 3. Kriteria UI prototype

Kriteria interaktif dijabarkan di Bagian 29.5 dalam spesifikasi ini. Setidaknya uji:

- Workspace tabs, add/remove/reorder, reset satu dan semua.
- Area resize, split, tab, move, swap, float, redock, maximize/restore.
- Shift Help pada hover/focus.
- Media preview sebelum masuk timeline.
- Universal Track dan linked A/V.
- Group/ungroup dan compound/open/decompose.
- Playhead, snap, Alt temporary snap, Razor, Split at Playhead, ESC.
- Mask, Corner Pin, Mesh Warp, Shape, 2D Visual Composition, generators, 2.5D, keyframe lane, Graph Editor, dan Preset Library.
- Color controls, text animation, export settings, responsive layout.

UI prototype dapat memakai data simulasi, tetapi label simulasi harus jelas dan tidak boleh dipakai sebagai bukti engine produksi.

#### 4. Skenario uji media dan playback

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
11. 2D particle generator dengan seed tetap menghasilkan output/cache yang konsisten.
12. 2D layered composition dan 2.5D text/shape diuji pada preview serta export dengan CPU/GPU path yang tersedia.

Jangan menjanjikan angka FPS universal sebelum ada benchmark perangkat tertentu. Catat perangkat, driver, versi build, media, setting, dan hasil aktual.

#### 5. Target perangkat rendah

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

#### 6. Kriteria stabilitas

- Project autosave tidak merusak project.
- Crash recovery hanya memulihkan data yang valid.
- Media asli tidak dihapus saat cache dibersihkan.
- Tidak ada kehilangan clip saat undo/redo.
- Tidak ada desync A/V tanpa peringatan atau recovery.
- Layout restore tidak menghapus project data.
- Gagal decode/export memberikan pesan yang dapat ditindaklanjuti.
- Pembaruan aplikasi dapat dipulihkan jika instalasi gagal.

#### 7. Bukti pengujian

Setiap tes penerimaan perlu menyimpan:

- ID requirement.
- Versi/commit yang diuji.
- Sistem operasi dan hardware.
- Langkah reproduksi.
- Hasil yang diharapkan.
- Hasil aktual.
- Screenshot/log/video jika relevan.
- Status: lulus, gagal, diblokir, atau belum dijalankan.

#### 8. Aturan CI

Pipeline minimal dapat menjalankan format check, lint, unit test, integration test, dan build pada target yang ditentukan. Tes yang memerlukan hardware tertentu dijalankan sebagai tes manual/benchmark terpisah, bukan dipalsukan menjadi tes CI universal.
