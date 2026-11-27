# Effects, Generators, Mask, Warp, dan 2D/2.5D VFX
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
> **Terkait:** [Transitions](TRANSITIONS.md) · [Preset Library](PRESETS.md)
---## 10. Inspector, Parameter, dan Tumpukan Efek

Inspector berada di area yang bisa dipindahkan/diganti, bukan area permanen yang tidak dapat dikustomisasi. Kontennya berubah berdasarkan objek/parameter yang dipilih.

### 10.1 Kategori Inspector

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

### 10.2 Transformasi dan Penyesuaian Dasar

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

### 10.3 Sistem Efek

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

### 10.4 Daftar Efek Video Bawaan

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

### 10.5 Visual Generators

Generator membuat sumber visual dari parameter. Generator berbeda dari effect: generator menghasilkan visual tanpa harus menerima footage sebagai input utama. Browser dan preset generator dipisahkan dari Effects dan Transitions.

| ID | Fitur | Persyaratan |
|---|---|---|
| GEN-01 | Generators Browser | Browser khusus untuk mencari, melihat kategori, preview, dan menambahkan generator. |
| GEN-02 | Solid/Gradient Generator | Membuat solid color, linear/radial gradient, dan gradient dengan beberapa color stops. |
| GEN-03 | Pattern Generator | Membuat grid, checkerboard, stripes, dots, dan pola geometris yang dapat dikontrol. |
| GEN-04 | Procedural Texture/Noise | Membuat noise dan tekstur prosedural dengan skala, detail, kontras, dan seed yang dapat dikontrol. |
| GEN-05 | 2D Particle Emitter | Membuat partikel 2D menggunakan shape atau image sprite sebagai sumber partikel. |
| GEN-06 | Particle Parameters | Mengatur emitter area/position, emission rate, jumlah maksimum, lifetime, direction, velocity, acceleration/gravity, size, rotation, color, dan opacity sepanjang lifetime. |
| GEN-07 | Particle Variation | Mengatur variasi yang terkontrol serta random seed agar preview, cache, dan export dapat direproduksi. |
| GEN-08 | Generator Timeline Item | Generator dapat ditempatkan pada timeline, diatur durasinya, di-keyframe bila relevan, serta menerima mask/effects/blend mode yang kompatibel. |
| GEN-09 | Generator Presets | Pengaturan generator dan particle emitter dapat disimpan serta digunakan ulang melalui Preset Library. |

Generator tidak boleh membuat UI freeze. Particle count, preview quality, cache, dan resource budget harus mempertimbangkan perangkat target. Generator 2D ini tidak berarti dukungan simulasi volumetrik, fluid, atau physics 3D.

---

---

## 11. Sistem Mask

Mask adalah fitur inti, bukan fitur tambahan. Pengguna dapat menggambar langsung pada Viewer dan mengatur parameter melalui Inspector.

### 11.1 Alat Pembuatan Mask

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

### 11.2 Parameter Mask

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

### 11.3 Keyframe Mask

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

---

## 12. Corner Pin, Corners, dan Mesh / Freeform Warp

**Requirement tambahan wajib.** Istilah “Corners” mengacu pada efek four-corner geometry/corner pin seperti yang ada di Kdenlive. Namun kebutuhan pengguna juga mencakup penambahan control points, sehingga fitur harus memiliki mode lebih luas.

### 12.1 Mode yang Harus Tersedia

| Mode | Deskripsi |
|---|---|
| Four-Corner / Corner Pin | Empat sudut independen: top-left, top-right, bottom-right, bottom-left. Dapat membentuk perspektif, skew, dan jajar genjang. |
| Mesh Warp | Mesh/grid yang bisa ditambah subdivisinya; titik sudut, tepi, dan titik interior bisa digeser. |
| Freeform Warp | Menambahkan/menghapus titik kontrol secara bebas pada area gambar. |
| Curve Warp (advanced) | Deformasi menggunakan points dan handles/kurva bila implementasi sudah matang. |

### 12.2 Persyaratan Corners / Corner Pin

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

### 12.3 Persyaratan Mesh / Freeform Warp

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

### 12.4 Kompatibilitas Objek

Corner Pin dan Warp dapat diterapkan pada elemen visual yang kompatibel:

- Video clips
- Still images
- Text/title layers
- Shapes/vector graphics
- Graphic overlays
- Compound visual compositions
- Mask paths bila model path mendukung transformasi tersebut

Warp tidak diterapkan langsung pada audio-only item karena tidak memiliki geometri visual. Audio tetap berjalan normal jika merupakan komponen pasangan video yang di-warp.

### 12.5 Aturan Topologi Keyframe

- Perpindahan titik warp dapat dianimasikan.
- Untuk interpolasi antar-keyframe yang stabil, struktur mesh/topology sebaiknya konsisten.
- Jika jumlah titik/struktur mesh berubah setelah ada keyframe, aplikasi harus memberi aturan yang jelas: propagate perubahan ke keyframe terkait, membuat keyframe baru yang kompatibel, atau memperingatkan bahwa interpolasi tidak tersedia.
- Jangan membuat interpolasi bentuk yang tidak valid secara diam-diam.

---

---

## 13. Sistem Pembuatan Shape

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

### 13.1 Keyframe Shape

Shape creation **wajib mendukung keyframes** untuk parameter yang relevan:

- Position, scale, rotation, anchor/pivot
- Opacity
- Fill color/gradient stops bila didukung
- Stroke color/width/opacity
- Corner radius
- Path control-point positions
- Shape-specific parameters (misalnya jumlah titik star bila modelnya mendukung)

Animasi perubahan path harus memperhatikan kesesuaian jumlah dan urutan nodes antar-keyframe. Bila struktur tidak kompatibel, jangan menghasilkan interpolasi yang rusak tanpa peringatan.

### 13.2 2D Visual Composition dan Motion Graphics

Fitur ini berbeda dari Custom Effect Builder. Custom Effect Builder merangkai operasi pemrosesan pada visual yang sudah ada; 2D Visual Composition merangkai objek/layer visual menjadi satu karya grafis yang tetap dapat diedit.

| ID | Fitur | Persyaratan |
|---|---|---|
| VFX2D-01 | Composition Editor | Membuat dan mengedit komposisi visual 2D dengan area kerja/preview sendiri. |
| VFX2D-02 | Visual Layer Sources | Menggabungkan shape/vector path, text/title, image, video, dan procedural generators yang kompatibel. |
| VFX2D-03 | Layer Stack | Mengatur urutan, duplicate, hide/show, lock/unlock, group/ungroup layer. |
| VFX2D-04 | Per-Layer Transform | Mengatur position, scale, rotation, anchor, crop, dan opacity tiap layer secara independen. |
| VFX2D-05 | Per-Layer Compositing | Mengatur blend mode dan compositing per layer. |
| VFX2D-06 | Per-Layer Effects | Memberi effect stack pada layer terpilih tanpa harus menerapkan efek ke seluruh komposisi. |
| VFX2D-07 | Per-Layer Mask/Warp | Menggunakan mask, Corner Pin, dan Mesh/Freeform Warp pada layer yang kompatibel. |
| VFX2D-08 | Layer Animation | Keyframe parameter layer dan objek menggunakan Keyframe Lane serta Graph Editor. |
| VFX2D-09 | Nested/Reusable Composition | Komposisi dapat digunakan pada timeline sebagai satu visual item, lalu dibuka kembali untuk mengedit layer internal tanpa flatten. |
| VFX2D-10 | Composition Templates | Menyimpan komposisi sebagai template/preset yang mempertahankan struktur layer dan kontrol yang diekspos. |

Komposisi harus tetap non-destructive. Menggunakan composition template tidak boleh mengubahnya menjadi video hasil render secara otomatis. Pengguna dapat memilih render/bake hanya bila memang tersedia sebagai tindakan eksplisit.

### 13.3 2.5D Motion Graphics

2.5D menyediakan efek kedalaman dan perspektif untuk motion graphics tanpa mengubah Kadrift menjadi full 3D scene editor atau node compositor.

| ID | Fitur | Persyaratan |
|---|---|---|
| MFX25D-01 | XYZ Transform | Objek/layer yang kompatibel memiliki position X/Y/Z, scale, serta rotation X/Y/Z. |
| MFX25D-02 | Depth Ordering | Mengatur kedalaman dan urutan layer berdasarkan posisi Z dengan aturan compositing yang konsisten. |
| MFX25D-03 | Virtual Camera | Mengatur perspektif, posisi/orientasi kamera, zoom atau field of view sesuai model yang dipilih. |
| MFX25D-04 | Text/Shape Extrusion | Memberi kedalaman/extrusion dan bevel pada text dan vector shape yang kompatibel. |
| MFX25D-05 | Basic Materials | Mengatur warna permukaan, roughness/specular sederhana, dan emission untuk objek 2.5D. |
| MFX25D-06 | Basic Lighting | Mendukung ambient, directional, dan point light sederhana beserta intensitas dan warna. |
| MFX25D-07 | Shadows | Mengatur cast/receive shadow, softness, dan opacity pada objek yang kompatibel. |
| MFX25D-08 | 2.5D Keyframes | Meng-keyframe transform XYZ, kamera, extrusion, material, dan parameter lighting yang dinyatakan animatable. |
| MFX25D-09 | 2.5D Presets | Menyimpan style, extrusion, lighting, camera, dan animation preset melalui Preset Library. |
| MFX25D-10 | 2D Compositing Output | Hasil 2.5D dirender ke komposisi video 2D dan dapat dikombinasikan dengan video, image, shape, text, mask, dan effects. |

**Batas scope 2.5D:** fokus pada text dan vector shapes serta layer sederhana yang didukung. Arbitrary 3D model/mesh import, mesh modeling, rigging karakter, physics 3D, volumetric/fluid simulation, dan full 3D scene compositing tidak termasuk scope desktop awal. Penambahan kemampuan tersebut memerlukan keputusan produk dan technical spike tersendiri.

---

---

## 14. Sistem Keyframe dan Graph Editor

Sebagian besar parameter yang bermakna untuk animasi harus mendukung keyframe, tetapi **tidak semua properti wajib keyframable**. Setiap parameter harus mendeklarasikan capability-nya secara eksplisit.

### 14.1 Sistem yang Mendukung Keyframe

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

### 14.2 Kontrol Keyframe

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

### 14.3 Graph Editor

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

---

### Prinsip Custom Effect dan Transition Builder

## 18. Pembuat Efek dan Transisi Kustom

Requirement penting: pengguna tidak boleh hanya memakai daftar efek/transisi bawaan. Mereka perlu dapat merakit, mengubah, menyimpan, dan membagikan efek/transisi sendiri langsung dari editor.

Ini **bukan Fusion** dan tidak memerlukan node compositor pada tahap ini. Sistem awal berbasis effect stack, parameter, layer, mask, shape, warp, dan keyframe.

### 18.1 Pembuat Efek Kustom

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
