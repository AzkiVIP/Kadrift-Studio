# Kadrift Studio

Kadrift Studio adalah proyek **professional non-linear video editor desktop** berbasis Rust. Target awal Windows x64. Aplikasi tidak memiliki fitur AI internal; mobile direncanakan setelah fondasi desktop matang.

Repository masih berada pada tahap persiapan dan validasi fondasi. Spesifikasi adalah target produk, bukan bukti bahwa fitur telah diimplementasikan.

## Dokumen utama

- [AGENTS.md](AGENTS.md) — alur kerja dan aturan AI coding agent.
- [VIDEO_EDITOR_MASTER_SPEC.md](VIDEO_EDITOR_MASTER_SPEC.md) — visi, batas produk, istilah global, aturan UI/UX, roadmap, dan indeks ke seluruh spesifikasi fitur.
- [PROJECT_STATUS.md](PROJECT_STATUS.md) — milestone, status fitur, blocker, dan task aktif.
- [ARCHITECTURE.md](ARCHITECTURE.md) — rancangan teknis, modul, plugin, dan release.
- [DECISIONS.md](DECISIONS.md) — keputusan produk serta pertanyaan teknis yang belum final.
- [CHANGELOG.md](CHANGELOG.md) — riwayat perubahan dokumentasi.

## Spesifikasi fitur

Seluruh detail requirement berada di `docs/features/`, dikelompokkan berdasarkan domain. Mulai dari [indeks Master Spec](VIDEO_EDITOR_MASTER_SPEC.md#02-indeks-dokumen-domain), lalu baca hanya file yang relevan dengan task.

Acceptance criteria, workflow prototype, dan strategi testing berada di `docs/quality/ACCEPTANCE_AND_TESTING.md`.

`LICENSE` tetap berlaku seperti sebelumnya.

## Prompt task singkat

> Kerjakan **[task]**. Ikuti `AGENTS.md`, periksa kondisi source code, lalu gunakan indeks Master Spec untuk membuka requirement domain yang relevan. Baca arsitektur, keputusan, skill, dan dokumentasi resmi hanya jika task memerlukannya. Pertahankan seluruh fitur yang sudah ditetapkan, implementasikan dalam scope, uji hasilnya, dan laporkan hasil aktual serta keterbatasannya.

Jangan membaca seluruh dokumen secara otomatis. Jangan menganggap requirement sebagai fitur yang sudah tersedia.
