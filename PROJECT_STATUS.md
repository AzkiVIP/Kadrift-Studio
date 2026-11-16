# Status Proyek Kadrift Studio

> **Tanggal audit dokumentasi:** 2026-11-16  
> **Status keseluruhan:** Persiapan dokumentasi dan validasi fondasi; aplikasi desktop belum dimulai.  
> **Platform pertama:** Windows x64 · **Bahasa inti:** Rust  
> **Nama produk:** Belum final; catatan penamaan berada di `DECISIONS.md`.
> **Versi aplikasi:** Belum ada versi rilis.
> **Status build:** Belum ada build aplikasi.
> **Blocker utama:** Framework UI desktop Rust belum dipilih; perlu technical spike.

## Fase aktif

**Fase 0 — Validasi fondasi UI dan arsitektur desktop.** Belum ada aplikasi desktop yang dapat dijalankan dan belum ada video engine. Prioritas berikutnya adalah technical spike framework UI desktop Rust: workspace tabs, Editor Areas, Editor Type Switcher, docking, keyboard/focus, dan persistence layout.
## Status Kelompok Fitur

| ID / Kelompok | Cakupan | Prioritas | Status awal | Bukti / catatan |
|---|---|---:|---|---|
| DOC | Dokumen proyek dan aturan AI | P0 | Sedang disiapkan | Dokumentasi proyek tersedia; panduan pengguna final mengikuti implementasi aplikasi. |
| WS-01–WS-16, UI-01–UI-21 | Workspace tabs, editor areas, docking, responsive UI, theme | P0 | Belum dimulai | Menjadi fokus prototype. |
| HELP-01–HELP-09 | Bantuan kontekstual Shift | P0 | Belum dimulai | Shift + Hover/Focus menjadi perilaku utama. |
| MED-01–MED-28 | Project dan media management | P0/P1 | Belum dimulai | Source preview wajib diuji. |
| PRE-01–PRE-18 | Viewer / video preview | P0/P1 | Belum dimulai | Preview prototype dapat memakai media contoh. |
| TL-01–TL-68 | Universal Track, linked A/V, navigation, snap, editing tools | P0 | Belum dimulai | Universal Track harus dipertahankan. |
| CLP-01–CLP-22 | Clip operations, Group, Compound Clip | P0/P1 | Belum dimulai | Group dan Compound Clip berbeda. |
| TRF / EFX | Inspector, transform, effect stack, effects | P0/P1 | Belum dimulai | Custom effect builder juga tercakup di EFX/custom section. |
| MSK-01–MSK-26 | Mask tools, parameters, animation | P0/P1 | Belum dimulai | Draw langsung di Viewer. |
| WRP-01–WRP-22 | Corner Pin, Mesh/Freeform Warp | P1 | Belum dimulai | Empat sudut dan mesh dengan titik tambahan adalah mode berbeda. |
| SHP-01–SHP-24 | Shape creation dan shape animation | P0/P1 | Belum dimulai | Keyframe shape wajib. |
| KF | Keyframe lanes, Graph Editor, easing | P0/P1 | Belum dimulai | Graph Editor menampilkan channel terpilih. |
| COL | Color correction, wheels, curves, scopes | P1 | Belum dimulai | Workspace Color khusus. |
| AUD | Audio editing dan mixing | P1 | Belum dimulai | Jalur audio terpisah dari compositing visual. |
| TXT | Text, title, subtitle, text animation | P1 | Belum dimulai | Typewriter dan preset reveal/motion wajib masuk rancangan. |
| TRN | Transition library dan custom transition builder | P1 | Belum dimulai | Pengguna dapat merancang transisi sendiri. |
| EXP | Render, export, render queue | P1 | Belum dimulai | Harus berjalan di luar thread UI. |
| PLG | Plugin Manager, creator, SDK, compatibility | P2 | Belum dimulai | API, sandbox, dan versioning belum diputuskan. |
| PERF | Proxy, cache, GPU, memory, performance monitor | P0/P1 | Belum dimulai | Harus dirancang dari awal; bukan optimasi tempelan. |
| SYS | Autosave, recovery, undo/redo, project safety | P0/P1 | Belum dimulai | Project data tidak boleh rusak karena layout reset. |
| DOCS | Dokumentasi pengguna dan developer | P1 | Kerangka disiapkan | Dokumentasi final mengikuti perilaku aktual. |
| REL | CI, ZIP, EXE, GitHub Releases, updater | P2 | Belum dimulai | Implementasi setelah build aplikasi dasar tersedia. |
| MOB | Versi mobile | Masa depan | Ditunda | Tidak masuk scope desktop pertama. |
| FUSION | Node compositor/Fusion | Di luar scope saat ini | Ditunda | Jangan implementasikan tanpa keputusan baru. |
| AI | Fitur AI di dalam aplikasi | Tidak termasuk | Dibatalkan dari scope | AI boleh digunakan untuk membantu pengembangan, bukan menjadi fitur aplikasi. |

## Definisi Status

Seluruh fitur aplikasi saat ini berstatus **Belum dimulai**. Tidak ada fitur yang boleh ditandai selesai hanya karena telah dideskripsikan atau ditampilkan sebagai mockup.

Gunakan status berikut:

| Status | Arti |
|---|---|
| Belum dimulai | Belum ada implementasi. |
| Direncanakan | Sudah masuk rencana tahap tertentu, tetapi belum dikerjakan. |
| Sedang dikerjakan | Implementasi sedang berlangsung. |
| Prototype | Interaksi UI tersedia, tetapi dapat menggunakan data/media simulasi dan belum merupakan engine final. |
| Diimplementasikan | Implementasi kode ada, tetapi pengujian penerimaan belum lengkap. |
| Diuji | Acceptance test yang relevan sudah lulus dan bukti dicatat. |
| Selesai | Implementasi, tes, dokumentasi, dan kriteria penerimaan lengkap. |
| Ditunda | Sengaja ditunda dengan alasan yang dicatat. |
| Dibatalkan | Dibatalkan melalui keputusan pemilik proyek. Jangan menghapus jejaknya. |
| Diblokir | Pengerjaan tidak dapat dilanjutkan sebelum blocker yang tercatat diselesaikan. |

## Catatan Bukti Implementasi

Ketika fitur mulai dikerjakan, tambahkan rincian di bawah ini:

| ID fitur | Status | Versi/commit | Tes yang dijalankan | Hasil | Keterbatasan |
|---|---|---|---|---|---|
| — | Belum dimulai | — | — | — | Belum ada implementasi. |

Jangan menulis `Lulus` tanpa nama tes, lingkungan, dan hasil aktual.

## Keputusan Teknis Terbuka

Framework UI Rust, media framework, GPU/hardware decode, audio engine, project schema, timeline internals, plugin runtime, warp engine, text rendering, installer, dan updater belum final. Catat bukti serta trade-off di `DECISIONS.md` sebelum mengunci keputusan.

## Task Aktif dan Urutan Pengerjaan

### 1. Jangan langsung membuat seluruh aplikasi

Langkah yang benar bukan langsung mengimplementasikan semua fitur video editor. Pertama-tama pastikan kerangka UI bisa mendukung workspace modular yang menjadi ciri utama produk. Mengganti sistem docking setelah banyak editor dibangun akan mahal.

### 2. Urutan tugas

#### Tugas 1 — Technical spike framework UI

Buat uji kecil untuk 2–3 kandidat framework UI desktop Rust. Jangan membuat project besar dahulu.

Setiap kandidat diuji pada hal berikut:

- Workspace tabs di bar atas.
- Dua atau lebih editor areas.
- Resize divider.
- Split horizontal dan vertikal.
- Drag/move editor area.
- Tabbed editor.
- Floating window dan redock jika didukung.
- Shortcut keyboard dan fokus area.
- Responsivitas saat jendela diubah ukurannya.
- Konsumsi memori dan kestabilan pada Windows.

Catat versi library, dokumentasi, contoh resmi, batasan, lisensi, risiko, dan hasil build. Jangan memilih hanya dari demo visual.

#### Tugas 2 — Prototype interaktif

Setelah framework dipilih, bangun UI prototype yang dapat diuji. Tidak perlu video engine final, tetapi state dan interaksi harus benar-benar berjalan.

Prioritas prototipe:

1. Main window, menu, toolbar, workspace tabs.
2. Sistem editor area: tab, split, resize, move, swap, maximize, float/redock, reset workspace.
3. Media Browser, Source Preview, Viewer, Inspector, Timeline.
4. Universal Track dan clip sample.
5. Linked audio-video, unlink/relink, group/ungroup, compound clip.
6. Playhead, scrubbing, snap, Alt temporary snap, Razor, Split at Playhead, ESC.
7. Effects Browser dan Inspector dengan parameter contoh.
8. Mask drawing dengan shape, pen/path, brush, feather.
9. Corner Pin dan Mesh/Freeform Warp dengan titik yang dapat digeser.
10. Shape Creation dan beberapa kontrol keyframe.
11. Keyframe Lane serta Graph Editor untuk channel terpilih.
12. Color controls, Color Wheels, Curves, scopes mock/interaktif.
13. Text editor dan animasi Typewriter/reveal.
14. Export settings dan render queue sebagai alur UI.
15. Shift Help, shortcut search, dark/light theme, responsive behavior.

#### Tugas 3 — Uji UX dan revisi

- Jalankan workflow contoh yang disusun di Bagian 29.5 `VIDEO_EDITOR_MASTER_SPEC.md`.
- Catat kesulitan, jumlah langkah, konflik shortcut, dan layout yang membingungkan.
- Jangan menambahkan fitur hanya demi terlihat ramai.
- Perbaiki struktur sebelum membangun engine produksi.

#### Tugas 4 — Bekukan keputusan arsitektur awal

Setelah framework UI dan model interaksi diuji:

- Catat keputusan framework UI beserta bukti uji di `DECISIONS.md`.
- Tetapkan model workspace/editor area.
- Tentukan model project dan timeline awal.
- Tentukan jalur evaluasi media engine.
- Perbarui `ARCHITECTURE.md` dan status fitur.

### 3. Definisi selesai untuk tugas aktif

Tugas aktif dianggap selesai hanya jika:

- Framework UI dipilih berdasarkan bukti uji dan keputusannya dicatat di `DECISIONS.md`.
- Prototype dapat dijalankan pada Windows.
- Semua interaksi P0 di Bagian 29.5 `VIDEO_EDITOR_MASTER_SPEC.md` diuji.
- Tidak ada tombol utama yang inert tanpa label status yang jelas.
- Resize, split, tab, floating, redock, dan reset layout berjalan sesuai acceptance criteria framework yang dipilih.
- Workflow timeline/media utama dapat dicoba dari awal sampai akhir menggunakan data uji.
- Kekurangan prototype dan hal yang masih disimulasikan didokumentasikan.

### 4. Setelah selesai

Tentukan tugas berikutnya berdasarkan hasil teknis spike dan temuan UX. Jangan otomatis melompat ke plugin SDK atau mobile sebelum project model dan media pipeline memiliki fondasi yang teruji.
