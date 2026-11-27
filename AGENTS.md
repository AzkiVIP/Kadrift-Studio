# Aturan Kerja AI — Kadrift Studio

Ikuti alur ini untuk setiap task. Jangan membaca seluruh dokumentasi atau skill jika tidak relevan.

1. **Analisis sebelum mengubah apa pun.** Periksa source code, konfigurasi, dependency, branch, dan tes yang relevan; identifikasi modul terdampak dan jalankan baseline test/build bila environment tersedia. Pisahkan fakta terverifikasi, asumsi, dan hal yang belum diketahui; jangan menyetujui solusi yang belum didukung bukti. Untuk ketidakpastian yang berdampak pada arsitektur/perilaku, verifikasi atau minta keputusan terlebih dahulu.
2. **Periksa `PROJECT_STATUS.md`.** Baca ringkasan milestone, status, dan blocker. Baca bagian Task Aktif hanya bila task berkaitan dengannya.
3. **Ambil requirement yang relevan** dari `VIDEO_EDITOR_MASTER_SPEC.md`. Gunakan daftar isi di awal dokumen; baca bagian terkait saja. Baca bagian lain hanya bila ada dampak lintas fitur.
4. **Baca `ARCHITECTURE.md` dan `DECISIONS.md` bila diperlukan**, yaitu saat task mengubah batas modul, dependency, media pipeline, data project, plugin, GPU, audio, atau release. Jangan memuatnya untuk perubahan kecil yang tidak menyentuh area tersebut.
5. **Gunakan skill dan dokumentasi resmi secara selektif.** Untuk pekerjaan UI/UX, baca `.agents/skills/impeccable/SKILL.md` bila file itu tersedia di lingkungan kerja. Baca skill lain hanya bila benar-benar sesuai task. Untuk API/library yang berubah, periksa dokumentasi resmi versi yang digunakan. Skill dan referensi tidak boleh mengubah requirement produk.
6. **Buat perubahan yang terukur.** Pertahankan semua fitur dan requirement yang disetujui. Jangan mengganti scope, framework, format project, atau dependency besar tanpa bukti dan keputusan pemilik proyek.
7. **Uji dan laporkan secara jujur.** Jalankan formatter, lint, test, atau build yang relevan. Sebutkan perintah dan hasil aktual; bedakan implementasi, simulasi, dan hal yang sudah diuji.
8. **Perbarui `PROJECT_STATUS.md` dan `CHANGELOG.md`** bila ada perubahan nyata. Ubah hanya bagian dokumentasi lain yang terdampak. Dokumentasi baru ditulis dalam Bahasa Indonesia; istilah API/teknologi boleh tetap dalam bahasa aslinya.
9. Untuk task besar, rumuskan tujuan, scope, requirement ID, acceptance criteria, dan tes yang relevan. Task kecil tidak perlu template/form panjang.

## Batasan yang tidak boleh dilanggar

- Produk adalah aplikasi video editor **desktop** berbasis Rust; Windows x64 menjadi platform pertama.
- Tidak ada fitur AI internal. Fusion/node compositor dan full 3D scene compositing/modeling berada di luar scope awal. 2.5D motion graphics yang dibatasi di `VIDEO_EDITOR_MASTER_SPEC.md` tetap termasuk scope. Mobile adalah rencana masa depan.
- Jangan menghapus, mengurangi, atau menyederhanakan kapabilitas/requirement tanpa persetujuan pemilik proyek.
- Bedakan prototype, mock/simulasi, implementasi, dan hasil yang sudah diuji.
- Jangan mengklaim codec, GPU acceleration, playback, export, plugin sandbox, atau updater berhasil tanpa tes yang membuktikannya.
- Gunakan Rust yang aman dan idiomatis. Hindari `unsafe` kecuali benar-benar diperlukan, disertai alasan, dan ditinjau. Pecah fungsi/modul secara wajar; hindari desain monolitik.
- Jangan melakukan refactor massal yang tidak terkait task. Jangan menambah dependency tanpa alasan, evaluasi lisensi, dan dampak maintenance.
- Pekerjaan berat seperti decode, render, dan file I/O tidak boleh memblokir UI thread. Pisahkan UI, domain model, timeline, media, render/export, persistence, plugin, dan updater secara wajar. Hindari global mutable state yang tidak perlu, tangani error secara jelas, dan jangan menaruh business logic di komponen UI.
- Jangan membuat tombol inert, placeholder yang diklaim selesai, hasil visual hard-coded untuk satu screenshot, atau state project yang hanya hidup di memori UI jika perlu disimpan. Jangan menggunakan sample/mock sebagai bukti engine produksi.
- Jaga shortcut dan drag/drop agar tidak saling bertabrakan. Hindari format ulang atau refactor massal yang tidak terkait task.
- Jika keputusan penting belum diketahui, nyatakan sebagai belum diputuskan; jangan mengarang atau memilih diam-diam. Catat keputusan yang berdampak pada arsitektur, dependency, project format, plugin API, rendering, atau kompatibilitas di `DECISIONS.md`.
- Jika pemilik proyek menyediakan desain, wireframe, screenshot, atau HTML prototype, perlakukan itu sebagai sumber kebenaran visual. Implementasikan dengan setia; jangan melakukan redesign atau menulis spesifikasi estetika panjang yang menduplikasi desain tanpa diminta. Fokus dokumen pada perilaku, state, interaksi, aksesibilitas, constraint, dan acceptance criteria.

## Laporan akhir

Sampaikan secara singkat: file yang berubah, perilaku yang dibuat, tes/build beserta hasil, hal yang belum dibuat/diuji, serta risiko atau blocker.
