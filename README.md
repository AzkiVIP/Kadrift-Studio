# Kadrift Studio

Kadrift Studio adalah proyek **professional non-linear video editor desktop** berbasis Rust. Target awal Windows x64. Aplikasi tidak memiliki fitur AI internal; mobile direncanakan setelah fondasi desktop matang.

Repository masih berada pada tahap persiapan dan validasi fondasi. Spesifikasi adalah target produk, bukan bukti bahwa fitur telah diimplementasikan.

## Dokumen

- `AGENTS.md` — alur kerja dan aturan AI coding agent.
- `VIDEO_EDITOR_MASTER_SPEC.md` — sumber seluruh fitur, perilaku, batasan, dan kriteria produk. Baca bagian yang relevan saja.
- `PROJECT_STATUS.md` — milestone, status fitur, blocker, dan task aktif.
- `ARCHITECTURE.md` — desain teknis, plugin, dan release.
- `DECISIONS.md` — keputusan produk serta pertanyaan teknis yang belum final.
- `CHANGELOG.md` — riwayat perubahan.

`LICENSE` tetap berlaku seperti sebelumnya.

## Prompt task singkat

> Kerjakan **[task]**. Ikuti `AGENTS.md`, periksa kondisi source code, lalu baca requirement terkait di `VIDEO_EDITOR_MASTER_SPEC.md`. Baca arsitektur, keputusan, skill, dan dokumentasi resmi hanya jika task memerlukannya. Pertahankan seluruh fitur yang sudah ditetapkan, implementasikan dalam scope, uji hasilnya, dan laporkan hasil aktual serta keterbatasannya.

Jangan membaca seluruh dokumen secara otomatis. Jangan menganggap requirement sebagai fitur yang sudah tersedia.
