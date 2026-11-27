# Changelog Dokumentasi

## 2026-11-15

- Audit struktur dokumentasi dan instruksi baca AI.
- Menambahkan indeks selective reading untuk Master Spec tanpa memotong requirement aslinya.
- Menggabungkan status kelompok fitur dan task aktif ke `PROJECT_STATUS.md`.
- Mengubah kriteria UI menjadi fokus pada aplikasi desktop.
- Menambahkan Editor Type Switcher ala Blender sebagai requirement eksplisit.
- Menambahkan arahan Dark Theme netral berlapis dan guardrail rendering ikon/Shift Help.

## 2026-11-16

- Konsolidasi dokumentasi menjadi tujuh file Markdown utama.
- Memindahkan seluruh inventory acceptance UI, workflow prototype, dan strategi pengujian ke Master Spec tanpa mengurangi cakupan.
- Menggabungkan status fitur dan task aktif ke Project Status.
- Menggabungkan detail plugin dan release ke Architecture, serta analisis nama ke Decisions.
- Menghapus dokumen sementara dan dokumen yang sudah digabung setelah verifikasi hash sumber.


## 2026-11-27

- Memperjelas pemisahan Effects, Transitions, Generators, Shapes/Graphics, 2D Visual Composition, 2.5D Motion Graphics, dan Preset Library.
- Menambahkan 2D procedural generators termasuk 2D particle emitter dengan seed yang dapat direproduksi.
- Menambahkan 2D Visual Composition berlapis yang tetap editable serta 2.5D motion graphics terbatas; full 3D scene compositing tetap di luar scope awal.
- Memperluas Preset Library untuk efek, transisi, generator, shape, komposisi, 2.5D, text, mask/warp, color, audio, keyframe/curve, export, dan workspace.
- Menambahkan acceptance criteria dan prototype workflows untuk generator, 2D composition, 2.5D, dan preset lintas kategori.
- Menetapkan desain/wireframe/HTML prototype dari pemilik proyek sebagai acuan visual; spesifikasi tertulis fokus pada perilaku dan interaksi.

## 2026-11-27 — Pemisahan Spesifikasi Domain

- Memindahkan detail requirement dari Master Spec ke 10 dokumen fitur berdasarkan domain, tanpa mengubah nomor bagian atau requirement ID.
- Memindahkan acceptance criteria, workflow prototype, dan strategi pengujian ke `docs/quality/ACCEPTANCE_AND_TESTING.md`.
- Menjadikan Master Spec sebagai dokumen visi, aturan global, istilah, roadmap, dan indeks spesifikasi domain.
- Memperbarui AGENTS, README, Project Status, dan Architecture agar alur baca serta tautan lintas dokumen konsisten.
- Tidak mengubah status implementasi fitur; pemisahan ini hanya restrukturisasi dokumentasi.
