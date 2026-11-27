# Core Editing: Project, Media, Viewer, Timeline, dan Clip
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
---## 6. Pengelolaan Proyek dan Media

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

#### 6.1 Persyaratan Preview Media

- Pengguna dapat memilih media di browser dan memutarnya sebelum menaruhnya di timeline.
- Preview sumber tidak mengubah playhead timeline utama kecuali pengguna memilih aksi khusus untuk itu.
- Pengguna dapat menetapkan In/Out pada source monitor.
- Drag dari media browser ke timeline harus membuat clip dari sumber yang dipilih.
- Video yang memiliki audio dapat ditambahkan sebagai linked video/audio components sesuai pengaturan insert.

---

---

## 7. Viewer / Pratinjau Video

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

---

## 8. Timeline dan Sistem Track Universal

### 8.1 Track Universal

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

### 8.2 Komponen Audio-Video yang Terhubung

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

### 8.3 Navigasi Timeline

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

### 8.4 Snap / Penempelan Otomatis

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

### 8.5 Alat Editing

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

---

## 9. Operasi Clip, Grouping, dan Compound Clip

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
