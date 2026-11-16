# Rancangan Arsitektur Aplikasi

> **Status:** Rancangan awal. Belum menjadi keputusan implementasi final.
> **Tujuan:** Memisahkan tanggung jawab aplikasi agar UI, timeline, media engine, render, audio, plugin, dan updater dapat berkembang tanpa saling mengunci.

## 1. Prinsip arsitektur

1. Rust menjadi bahasa utama untuk core dan logika aplikasi.
2. UI tidak boleh menjadi tempat menjalankan decoding, rendering, atau file I/O berat.
3. Project model menjadi sumber data utama; UI menampilkan dan mengubah model melalui command/action yang tervalidasi.
4. Timeline visual dan routing audio mempunyai logika terpisah walaupun pengguna melihat Universal Track.
5. Media engine memakai komponen matang; jangan menulis codec video sendiri.
6. Rendering efek harus mendukung CPU fallback jika GPU tidak tersedia atau tidak cocok.
7. Semua tugas panjang dapat dibatalkan dan melaporkan progres.
8. Format project memiliki versi schema dan mekanisme migrasi.
9. Plugin beroperasi melalui kontrak API yang stabil dan batas keamanan.
10. Jangan mengunci arsitektur desktop pada asumsi bahwa mobile akan tersedia sejak awal.

## 2. Gambaran lapisan

```text
Aplikasi Desktop
├── UI Shell
│   ├── Menu, Toolbar, Command Search
│   ├── Workspace Tabs
│   ├── Workspace Layout Manager
│   ├── Editor Area / Docking System
│   └── UI Accessibility, Help, Themes
│
├── Application Layer
│   ├── Command / Action Router
│   ├── Undo / Redo History
│   ├── Project Session Manager
│   ├── Task Scheduler
│   └── Notification / Error Center
│
├── Domain Core
│   ├── Project Model
│   ├── Media Asset Model
│   ├── Timeline Model
│   ├── Clip / Group / Compound Model
│   ├── Keyframe / Animation Model
│   ├── Mask / Shape / Warp Model
│   ├── Effect / Transition Model
│   └── Color / Audio Routing Model
│
├── Media & Render Services
│   ├── Media Probe / Decode
│   ├── Proxy / Thumbnail / Waveform
│   ├── Video Compositor
│   ├── Effect Processing
│   ├── Audio Engine / DSP
│   ├── Color Processing
│   ├── Render / Encode Queue
│   └── Cache / Resource Manager
│
├── Extension Layer
│   ├── Plugin Manager
│   ├── Plugin Runtime / Sandbox
│   ├── Effect / Transition APIs
│   └── Plugin SDK / Documentation
│
└── Platform Services
    ├── File System / OS Integration
    ├── GPU / Hardware Codec Adapters
    ├── Logging / Crash Recovery
    ├── Update Service
    └── Windows Packaging
```

Diagram ini adalah pembagian tanggung jawab konseptual. Jangan memecahnya menjadi banyak crate sebelum dependensi dan batas modulnya dipahami.

## 3. Rancangan workspace dan editor area

### Workspace Manager

Tanggung jawab:

- Mengelola daftar workspace bawaan dan custom.
- Menyimpan urutan tab atas.
- Menyimpan layout terpisah per workspace.
- Menangani tambah, ubah nama, duplikasi, hapus, impor, ekspor, dan reset workspace.
- Memulihkan layout terakhir.

### Editor Area Manager

Tanggung jawab:

- Menyimpan pohon layout area.
- Split horizontal/vertikal.
- Resize dan minimum size.
- Tabbed editors.
- Move/swap editor.
- Maximize dan restore.
- Floating window, multi-monitor, dan redock.
- Pemulihan jika monitor/window sebelumnya sudah tidak tersedia.

Layout workspace harus disimpan terpisah dari project editing. Reset workspace tidak boleh memodifikasi isi project.

## 4. Rancangan model timeline

### Universal Track di lapisan domain

Universal Track adalah abstraksi interaksi pengguna. Setiap item memiliki tipe dan capability. Sistem internal menentukan pipeline yang sesuai:

- Visual item → compositing/video pipeline.
- Audio item → audio routing/mixer.
- Item gabungan → dapat memiliki komponen visual dan audio yang linked.
- Text/shape/adjustment/effect item → visual generator atau operation sesuai tipe.

Urutan track menjadi urutan compositing untuk item visual. Audio tidak boleh dicampur berdasarkan urutan visual; audio masuk ke audio graph/routing terpisah.

### Linked audio-video

Model hubungan harus memakai identitas komponen dan constraint sinkronisasi, bukan sekadar memindahkan dua objek secara kebetulan. Operasi seperti move, trim, split, ripple, copy, paste, dan relink harus mendefinisikan apakah pasangan tetap linked.

### Group dan Compound Clip

- Group menyimpan himpunan referensi clip individual dan transformasi operasional bersama.
- Compound Clip membentuk container/sequence internal yang menjadi satu item pada timeline induk.
- Decompose harus mempertahankan waktu, urutan track, efek, dan hubungan yang dapat direpresentasikan.

## 5. Model waktu dan keyframe

- Gunakan timebase berbasis frame/tick yang eksplisit; jangan mengandalkan floating-point detik untuk identitas posisi frame.
- Setiap parameter menyatakan apakah mendukung animasi, tipe nilainya, interpolasi yang diperbolehkan, batas nilai, dan satuannya.
- Keyframe menyimpan waktu, nilai, tipe interpolasi, dan data tangent/handle jika diperlukan.
- Graph Editor membaca channel yang dipilih dari model keyframe yang sama dengan Timeline.
- UI lane dan Graph Editor tidak boleh memiliki dua sumber data berbeda.
- Struktur path/mesh yang berubah harus mengikuti aturan topologi; perubahan tidak boleh membuat interpolasi rusak secara diam-diam.

## 6. Media pipeline

Komponen yang perlu dievaluasi:

- FFmpeg untuk demux/decode/encode/probe dan dukungan format luas.
- GStreamer untuk graph/pipeline media dan integrasi Rust.
- Kombinasi keduanya hanya jika batas modul dan lisensinya dipahami.

Kriteria evaluasi:

- Codec dan container yang benar-benar didukung.
- Hardware decode/encode pada Intel dan AMD iGPU sasaran.
- Sinkronisasi audio/video.
- Seek performance dan random access.
- Frame format, color metadata, HDR, bit depth.
- Lisensi komponen serta kewajiban distribusi.
- Ukuran paket aplikasi.
- Kemudahan debugging dan pengujian Windows.

Jangan menganggap hardware decode selalu lebih cepat. Transfer frame GPU↔RAM yang berlebihan dapat menghapus keuntungan akselerasi.

## 7. Render dan efek

Effect stack harus memiliki urutan deterministik. Setiap efek mendeskripsikan:

- Parameter dan rentang nilai.
- Apakah parameter mendukung keyframe.
- Input/output yang diharapkan.
- Kebutuhan CPU/GPU.
- Mode preview/cache.
- Versi implementasi.
- Pesan error dan fallback.
- Dokumentasi parameter untuk Shift Help.

Custom Effect Builder dan Custom Transition Builder pada tahap ini adalah sistem komposisi efek/parameter/animasi yang dapat disusun pengguna, bukan Fusion-style node compositor.

## 8. Performa dan thread

Kelompok pekerjaan:

- Thread UI: input, layout, drawing UI, state presentasi.
- Worker pool: thumbnail, probe, waveform, proxy, file scan.
- Playback scheduler: menjaga prioritas frame yang akan ditampilkan.
- Media decode: pembacaan/decode stream.
- GPU queue: pemrosesan visual sesuai kemampuan perangkat.
- Audio callback/engine: jalur audio berprioritas dan bebas blocking.
- Render workers: export di background.

Aturan:

- Tidak ada file I/O besar atau decode blocking pada thread UI.
- Worker harus bisa dibatalkan ketika media/clip tidak lagi dibutuhkan.
- Cache harus memiliki batas dan kebijakan eviction.
- Resource budget memperhitungkan RAM sistem dan memori bersama iGPU.
- Playback mendapat prioritas dibanding thumbnail/proxy generation yang tidak mendesak.
- Tampilkan dropped frames dan penggunaan resource jika informasi tersedia.

## 9. Penyimpanan project

Project perlu menyimpan:

- Identitas project dan schema version.
- Project settings/timebase/color configuration.
- Referensi media dan metadata yang diperlukan.
- Timeline, track, clip, group, compound clip.
- Efek, mask, warp, shapes, text, audio routing.
- Keyframes, preset references, dan plugin references.
- Workspace/layout hanya jika memang diputuskan sebagai bagian project; default-nya pisahkan user workspace dari project data.

Cache yang dapat dibuat ulang sebaiknya disimpan terpisah dari data inti project. Jangan menanam absolute path tanpa strategi relink.

## 10. Plugin runtime

Model plugin belum diputuskan. Kandidat yang perlu dievaluasi adalah WASM/WASI Component Model untuk kontrak yang lebih stabil dan batas sandbox yang lebih baik. Native plugin dapat dipertimbangkan untuk plugin tepercaya di masa depan, tetapi memiliki risiko ABI, crash, akses sistem, dan keamanan lebih besar.

Jangan memuat plugin native tidak tepercaya dalam proses utama. Plugin perlu capability declaration, versi API, validasi, permission, dan kegagalan terisolasi sejauh memungkinkan.

## 11. Pembaruan aplikasi

Updater harus dipisahkan dari UI utama. Ia bertanggung jawab memeriksa versi rilis, memverifikasi sumber dan integritas paket, meminta persetujuan, mengunduh, menutup aplikasi jika diperlukan, memasang versi baru, dan memulihkan versi lama jika pemasangan gagal.

Rincian build, distribusi, dan updater berada pada Bagian 15 dokumen ini.

## 12. Struktur crate yang mungkin

Ini contoh pemisahan awal, bukan perintah untuk membuat semua crate sejak hari pertama:

```text
crates/
  app-shell/          # bootstrap aplikasi dan integrasi OS
  ui-workspace/       # workspace tabs, area, docking, layout persistence
  ui-editors/         # viewer, timeline, inspector, browser, scopes
  app-core/           # command router, history, task coordinator
  project-model/      # project schema dan migrasi
  timeline-core/      # track, clip, group, compound, timebase
  animation-core/     # keyframe, interpolation, graph data
  geometry-core/      # mask, shape, corner pin, mesh warp
  media-core/         # probe, decode, asset model
  media-cache/        # proxy, thumbnail, waveform, cache policy
  render-core/        # compositing dan effect processing
  audio-core/         # audio routing dan DSP
  export-core/        # encode dan render queue
  plugin-api/         # kontrak API plugin
  plugin-host/        # runtime dan permission
  update-core/        # update check dan verifikasi paket
```

Mulai dengan jumlah modul minimal. Pisahkan crate hanya ketika batas dependensi sudah nyata dan memberi manfaat.

## 13. Technical spike yang wajib

Sebelum mengunci teknologi:

1. Uji minimal dua kandidat UI framework.
2. Uji docking/floating windows pada Windows.
3. Uji pipeline preview satu file video dan satu stream audio.
4. Uji GPU display/compositing sederhana.
5. Uji model waktu dan sinkronisasi audio-video.
6. Uji persistence layout dan project.
7. Dokumentasikan lisensi dependency.

Hasil setiap spike harus dicatat. Keputusan yang mengubah arsitektur atau kontrak teknis dimasukkan ke `DECISIONS.md` menggunakan format catatan di bagian akhir dokumen ini.


# 14. Plugin SDK dan Pengembangan Plugin

## 14.1 Tujuan sistem plugin

Sistem plugin nantinya memungkinkan pihak lain menambahkan kemampuan ke editor tanpa mengubah source code inti. Sasaran jenis plugin:

- Efek video.
- Transisi video.
- Generator visual dan title.
- Shape atau tool tambahan.
- Filter/audio DSP.
- Import/export format tambahan jika API dan lisensi memungkinkan.
- Panel/editor tambahan.
- Workflow dan automation yang aman.

Tidak semua jenis plugin harus didukung pada rilis pertama. API harus tumbuh secara bertahap dan kompatibilitasnya dijaga.

## 14.2 Prinsip desain

1. Versi API plugin harus eksplisit.
2. Plugin menyatakan jenis, nama, versi, pembuat, kompatibilitas aplikasi, izin, dan dependensi.
3. Plugin tidak boleh memperoleh akses file, jaringan, proses, atau sistem secara diam-diam.
4. Plugin yang gagal tidak boleh menyebabkan kerusakan project atau crash seluruh aplikasi jika dapat diisolasi.
5. Plugin dapat diaktifkan/dinonaktifkan tanpa menghapus file penggunanya.
6. Perubahan API harus memiliki migration guide.
7. Dokumentasi SDK dan contoh plugin disertakan.
8. Plugin pihak ketiga harus diperlakukan sebagai kode yang tidak tepercaya sampai diverifikasi.

## 14.3 Model runtime yang perlu dievaluasi

#### Kandidat A — WASM / WASI Component Model

Kelebihan potensial:

- Batas kemampuan dan permission lebih jelas.
- Lebih mudah mengisolasi plugin dibanding native library yang berjalan dalam proses utama.
- Kontrak lintas bahasa dapat dirancang melalui interface yang eksplisit.
- ABI tidak bergantung langsung pada ABI internal Rust.

Risiko/biaya:

- Integrasi GPU dan akses frame berperforma tinggi lebih rumit.
- Perlu runtime dan desain data exchange yang efisien.
- API host harus dirancang dengan baik agar plugin tidak menyalin frame besar secara berlebihan.

#### Kandidat B — Native plugin

Kelebihan potensial:

- Akses langsung ke kemampuan sistem dan performa native.
- Lebih cocok untuk integrasi tingkat rendah tertentu.

Risiko:

- ABI dan kompatibilitas binary lebih sulit.
- Plugin dapat crash atau merusak proses utama.
- Sandbox jauh lebih sulit.
- ABI Rust bukan kontrak stabil yang sebaiknya diekspos langsung antarversi compiler.

**Rekomendasi awal:** evaluasi WASM/WASI sebagai jalur plugin umum. Native plugin tepercaya dapat dipertimbangkan kemudian dengan proses keamanan terpisah. Jangan mengekspor Rust internal API sebagai ABI plugin publik.

## 14.4 Rancangan metadata plugin

Plugin perlu menyediakan metadata seperti:

- ID unik berbentuk reverse-domain atau format stabil.
- Nama tampilan.
- Versi plugin.
- Versi minimum/maksimum API host yang didukung.
- Nama pembuat dan informasi bantuan.
- Jenis plugin.
- Deskripsi dan kategori.
- Daftar permission.
- Dependensi plugin jika ada.
- Entry point/runtime yang digunakan.
- Checksum atau signature paket jika didistribusikan melalui katalog.

## 14.5 Plugin Manager

Fungsi yang direncanakan:

- Melihat plugin terpasang.
- Mengaktifkan/menonaktifkan plugin.
- Melihat versi dan pembuat.
- Melihat permission yang diminta.
- Membuka dokumentasi plugin.
- Mengatur urutan atau prioritas bila relevan.
- Memeriksa kompatibilitas dengan versi aplikasi.
- Melihat error log plugin.
- Menghapus plugin tanpa menghapus project/preset pengguna.
- Mengimpor plugin dari paket lokal.
- Memeriksa update plugin ketika katalog resmi tersedia.

## 14.6 Plugin Creator / Developer Tools

Tool pengembangan plugin dapat menyediakan:

- Template plugin.
- Metadata editor.
- API reference.
- Contoh effect plugin.
- Contoh transition plugin.
- Contoh audio plugin.
- Preview/test host.
- Validasi manifest.
- Validasi versi API.
- Pengujian permission.
- Log dan diagnostic.
- Paket plugin untuk distribusi.

Jangan menyebut tool tersebut sebagai compiler/editor kode penuh kecuali memang ada editor dan build pipeline yang nyata. Pengguna dapat menggunakan editor kode eksternal.

## 14.7 API effect dan transition

API effect minimal perlu mendeskripsikan:

- Input dan output.
- Parameter bertipe dan batas nilai.
- Default value.
- Satuan dan penjelasan.
- Dukungan keyframe per parameter.
- Kebutuhan CPU/GPU.
- Format pixel yang didukung.
- Kebijakan cache.
- Versi plugin.
- Penanganan error.

API transition perlu mendeskripsikan:

- Dua atau lebih input visual.
- Rentang waktu/durasi.
- Parameter transisi.
- Keyframe/interpolasi.
- Batas input/output.
- Cache dan kebutuhan resource.

## 14.8 Permission dan sandbox

Contoh permission yang mungkin diperlukan:

- Tidak ada akses eksternal.
- Akses hanya ke file yang dipilih pengguna.
- Akses jaringan dengan domain/izin yang dijelaskan.
- Akses perangkat audio tertentu.
- Akses render resource yang disediakan host.

Permission harus terlihat sebelum plugin dijalankan. Tolak plugin yang meminta izin melebihi kebutuhan tanpa alasan.

## 14.9 Dokumentasi wajib untuk setiap plugin

- Cara memasang.
- Cara mengaktifkan.
- Fungsi dan batasan.
- Arti seluruh parameter.
- Versi aplikasi/API yang kompatibel.
- Permission yang diminta.
- Contoh penggunaan.
- Cara melaporkan error.

## 14.10 Kriteria plugin siap rilis

Plugin hanya dianggap siap bila:

- Manifest valid.
- API compatibility lulus.
- Permission terdokumentasi.
- Plugin tidak merusak project ketika gagal.
- Tes effect/transition sesuai jenisnya lulus.
- Versi dan dependency jelas.
- Dokumentasi tersedia.
- Paket dapat diverifikasi.

# 15. Build, Distribusi, dan Sistem Pembaruan

## 15.1 Hasil distribusi yang diharapkan

Untuk setiap rilis Windows x64, rencanakan aset berikut sesuai kesiapan versi:

- Paket ZIP portable.
- Installer `.exe`.
- Catatan rilis.
- Berkas checksum.
- Metadata versi/rilis untuk updater.

Nama aset harus konsisten, misalnya:

```text
NamaAplikasi-0.1.0-windows-x64.zip
NamaAplikasi-0.1.0-windows-x64-setup.exe
SHA256SUMS.txt
release-manifest.json
```

Nama produk masih sementara; jangan mengunci nama contoh ini sebelum identitas final diputuskan.

## 15.2 Alur GitHub Actions

Pipeline rilis yang disarankan:

1. Pemilik proyek membuat tag versi seperti `v0.1.0`.
2. GitHub Actions berjalan pada Windows runner yang ditentukan.
3. Checkout repository dan pasang toolchain Rust yang dipatok versinya.
4. Pulihkan cache dependency dengan key yang aman.
5. Jalankan format check, lint, unit test, integration test yang sesuai, dan build release.
6. Buat paket portable ZIP.
7. Buat installer EXE menggunakan teknologi installer yang sudah diputuskan.
8. Hitung SHA-256 setiap aset.
9. Buat metadata release dan catatan rilis.
10. Jika penandatanganan diaktifkan, tandatangani metadata/manifest atau paket menggunakan private key yang hanya tersedia di GitHub Actions Secrets.
11. Buat GitHub Release dari tag dan unggah aset.
12. Simpan hasil build/log untuk audit.

Jangan menjalankan rilis publik hanya karena branch utama berubah. Rilis harus dipicu oleh tag versi yang disengaja atau workflow manual dengan approval.

## 15.3 Versi

Gunakan Semantic Versioning:

- `MAJOR`: perubahan besar yang memutus kompatibilitas project/plugin/API.
- `MINOR`: fitur baru yang kompatibel.
- `PATCH`: perbaikan bug dan keamanan yang kompatibel.

Contoh tahap awal: `0.1.0`, `0.2.0`, `0.2.1`. Tag Git menggunakan awalan `v`, misalnya `v0.2.1`.

Pre-release dapat memakai suffix seperti `-alpha.1`, `-beta.1`, atau `-rc.1`. Updater stabil sebaiknya tidak menawarkan pre-release kecuali pengguna mengaktifkan kanal beta.

## 15.4 Metadata pembaruan

Updater perlu membaca data rilis resmi. Format manifest dapat memuat:

- Nama produk dan platform.
- Versi SemVer.
- URL GitHub Release.
- URL ZIP dan installer.
- Ukuran file.
- SHA-256.
- Catatan rilis.
- Kanal stable/beta.
- Informasi minimum versi updater jika diperlukan.
- Tanda tangan digital manifest atau metadata rilis.

Contoh struktur konseptual (bukan format final):

```json
{
  "product": "NamaAplikasi",
  "channel": "stable",
  "version": "0.1.0",
  "platform": "windows-x64",
  "release_url": "https://github.com/OWNER/REPO/releases/tag/v0.1.0",
  "installer": {
    "url": "https://github.com/OWNER/REPO/releases/download/v0.1.0/NamaAplikasi-0.1.0-windows-x64-setup.exe",
    "sha256": "NILAI_SHA256_DARI_BUILD"
  },
  "notes": "URL_CATATAN_RILIS",
  "signature": "TANDA_TANGAN_MANIFEST"
}
```

Jangan memakai nilai contoh ini sebagai data rilis nyata. URL, hash, dan signature harus dihasilkan otomatis dari rilis yang sesungguhnya.

## 15.5 Alur updater di dalam aplikasi

1. Pengguna dapat memeriksa update dari menu aplikasi.
2. Aplikasi membaca versi saat ini dan kanal update.
3. Updater mengambil metadata dari sumber resmi.
4. Updater memeriksa format versi dan memastikan versi baru memang lebih tinggi.
5. Verifikasi tanda tangan dan hash sebelum paket dijalankan.
6. Tampilkan versi, ukuran unduhan, catatan rilis, dan perubahan penting.
7. Minta persetujuan pengguna sebelum mengunduh/memasang.
8. Unduh ke lokasi sementara dan validasi ulang.
9. Tutup aplikasi dengan aman jika pemasangan memerlukannya.
10. Jalankan update helper/installer.
11. Verifikasi file hasil pemasangan dan versi aplikasi.
12. Jika gagal, pulihkan versi sebelumnya atau berikan prosedur pemulihan yang aman.

Jangan menjalankan installer yang diunduh tanpa verifikasi. Jangan mengeksekusi file berdasarkan nama tag atau release text saja.

## 15.6 Keamanan dan keandalan updater

- Gunakan HTTPS.
- Hash SHA-256 memeriksa integritas, tetapi hash saja tidak membuktikan siapa penerbitnya jika manifest dan paket berasal dari sumber yang sama tanpa signature.
- Gunakan tanda tangan digital dengan public key yang tertanam pada aplikasi; private key disimpan sebagai secret CI dan tidak pernah masuk repository.
- Batasi izin updater pada direktori aplikasi yang diperlukan.
- Jangan menghapus project, media pengguna, plugin, atau preset ketika update.
- Sediakan rollback atau pemulihan ketika pemasangan gagal.
- Log proses update tanpa menyimpan informasi pribadi yang tidak diperlukan.
- Terapkan kebijakan kompatibilitas project dan plugin antarversi.

## 15.7 GitHub Release

Release GitHub menjadi sumber versi yang dapat dibaca updater. Untuk setiap tag rilis:

- Tag harus sesuai SemVer.
- Release notes menjelaskan fitur baru, perbaikan, perubahan kompatibilitas, dan masalah yang diketahui.
- File ZIP dan installer harus berasal dari build yang sama.
- Checksum/signature harus cocok dengan file yang diunggah.
- Jangan mengubah aset rilis setelah updater mulai menggunakannya; jika harus diperbaiki, buat rilis versi baru atau prosedur koreksi yang jelas.

## 15.8 Hal yang belum boleh diputuskan secara sepihak

- Teknologi installer.
- Mekanisme update helper.
- Lokasi instalasi default dan hak administrator.
- Format manifest final.
- Kanal beta/stable dan kebijakan update otomatis.
- Tanda tangan kode Windows dan proses sertifikat.

Semua perlu diuji dan dicatat di `DECISIONS.md` sebelum digunakan pada rilis publik.
