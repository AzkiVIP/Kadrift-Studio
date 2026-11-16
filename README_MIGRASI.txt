KADRIFT STUDIO — PAKET DOKUMENTASI MINIMAL

Isi paket ini adalah tujuh file Markdown final yang siap menggantikan file bernama sama di root repository. Tidak ada script, perintah terminal, atau migrasi otomatis yang perlu dijalankan.

CARA MENERAPKAN
1. Buka repository Kadrift-Studio di GitHub.
2. Upload tujuh file Markdown dari paket ini ke root repository dan pilih replace/overwrite untuk file yang sudah ada.
3. Periksa diff setiap file sebelum commit.
4. Setelah hasilnya ditinjau dan tautan/requirement terverifikasi, file lama berikut tidak lagi menjadi sumber aktif dan dapat dihapus:

- FEATURE_STATUS.md
- NEXT_TASK.md
- RENCANA_PROTOTIPE_UI.md
- KRITERIA_PENERIMAAN_UI_DESKTOP.md
- VIDEO_EDITOR_MASTER_SPEC_INDEX.md
- STRATEGI_PENGUJIAN.md
- PLUGIN_SDK_DAN_PANDUAN_PLUGIN.md
- RENCANA_RILIS_DAN_PEMBARUAN.md
- PANDUAN_DOKUMENTASI.md
- PENAMAAN_PRODUK.md
- TEMPLATE_TUGAS_AI.md
- PROMPT_MEMULAI_SESI_AI.md
- AUDIT_DOKUMENTASI.md
- MIGRASI_DOKUMENTASI.md
- PENJELASAN_STATUS_REVISI.md
- MANIFEST.json
- docs/README.md
- docs/adr/000-TEMPLATE-KEPUTUSAN.md

LICENSE tidak diubah dan harus tetap dipertahankan.

JAMINAN KONSOLIDASI
- Master Spec mempertahankan requirement produk yang ada; acceptance UI, workflow prototype, strategi pengujian, Editor Type Switcher, dan standar dokumentasi ditempatkan di dalamnya.
- ID acceptance UI dan ID workflow prototype diberi namespace berbeda agar tidak tabrakan.
- Status fitur dan task aktif dipusatkan di PROJECT_STATUS.md.
- Detail arsitektur plugin dan release digabung ke ARCHITECTURE.md.
- Keputusan produk, isu teknis terbuka, dan penamaan dipusatkan di DECISIONS.md.
- AGENTS.md menjadi satu-satunya instruksi operasional AI. AI membaca skill, referensi, dan dokumentasi resmi hanya bila task membutuhkannya.

CATATAN
Paket ini dibuat dari snapshot branch main commit 79a54d985d207b3fe146ba2a1039ee54f1103cd0 yang telah diperiksa. Repository GitHub tidak diubah oleh paket ini. Periksa diff sebelum menghapus file lama atau membuat commit.
