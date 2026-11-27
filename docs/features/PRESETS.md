# Universal Preset Library
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
> **Terkait:** [Effects dan 2D VFX](EFFECTS_AND_2D_VFX.md) · [Transitions](TRANSITIONS.md) · [Color Grading](COLOR_GRADING.md) · [Audio](AUDIO.md) · [Titles & Motion](TITLES_AND_MOTION.md)
---## 18.3 Universal Preset Library

Preset Library adalah sistem bersama untuk menyimpan dan menggunakan ulang pengaturan lintas fitur. Browser dapat dibuka dari kategori yang relevan tanpa harus membuat workspace baru untuk setiap jenis preset.

### Jenis preset yang wajib didukung

| ID | Jenis Preset | Cakupan |
|---|---|---|
| PST-01 | Single Effect | Parameter satu efek bawaan atau plugin. |
| PST-02 | Effect Stack | Kombinasi beberapa efek, urutan, parameter, dan exposed controls. |
| PST-03 | Transition | Parameter dan konfigurasi transisi bawaan maupun custom. |
| PST-04 | Generator | Parameter gradient, pattern, noise, dan 2D particle emitter. |
| PST-05 | Shape/Vector Style | Bentuk, fill, gradient, stroke, shadow, dan style lain. |
| PST-06 | 2D Composition Template | Struktur layer, objek, dan kontrol komposisi 2D yang dapat diedit. |
| PST-07 | 2.5D Motion Graphics | Extrusion, bevel, material, lighting, camera, dan konfigurasi animasi 2.5D. |
| PST-08 | Text Style/Title | Typography, style, lower-third, dan title template. |
| PST-09 | Text Animation | Typewriter, reveal, fade, slide, stagger, dan animation settings. |
| PST-10 | Mask | Shape/path mask dan parameter yang kompatibel. |
| PST-11 | Warp | Corner Pin, Mesh/Freeform Warp, dan parameter deformasi. |
| PST-12 | Color Grade | Color controls, wheels, curves, qualifier/window settings, dan LUT reference yang diperlukan. |
| PST-13 | Audio Chain | Rangkaian audio effects, routing/submix preset, dan parameter mixer yang kompatibel. |
| PST-14 | Keyframe/Curve | Easing, interpolation, tangent, dan Graph Editor curve preset. |
| PST-15 | Export Profile | Konfigurasi export yang tersimpan dan kompatibel dengan encoder tersedia. |
| PST-16 | Workspace Layout | Layout workspace; pengelolaan layout tetap terintegrasi dengan Workspace Manager. |

### Pengelolaan dan kompatibilitas preset

| ID | Fitur | Persyaratan |
|---|---|---|
| PST-17 | Preset Metadata | Menyimpan nama, kategori, deskripsi, tags, thumbnail/preview bila relevan, pembuat, dan versi. |
| PST-18 | User/Project Libraries | Mendukung preset library milik pengguna dan preset yang dibawa bersama project. |
| PST-19 | Save/Update Preset | Save as new; memperbarui preset yang ada harus menjadi tindakan eksplisit dan tidak menimpa diam-diam. |
| PST-20 | Organize Presets | Search, filter kategori, favorite, duplicate, rename, dan delete. |
| PST-21 | Import/Export | Mengimpor dan mengekspor preset dalam format yang tervalidasi. |
| PST-22 | Dependency Check | Memeriksa efek/plugin/generator yang dibutuhkan; jelaskan dependency yang hilang atau tidak kompatibel. |
| PST-23 | Version Compatibility | Menyimpan versi format preset dan menyediakan migration atau peringatan saat format berubah. |
| PST-24 | Editable Application | Preset diterapkan sebagai parameter/struktur yang masih dapat diedit, bukan otomatis di-bake menjadi video. |
| PST-25 | Safe Overwrite | Konfirmasi sebelum overwrite atau delete preset yang telah digunakan; jangan merusak preset asli saat mengubah hasil aplikasi preset. |

Preset yang memakai plugin atau asset eksternal harus memberi tahu pengguna tentang dependency tersebut. Jangan menyatakan preset portable apabila dependency yang dibutuhkan tidak ikut disertakan atau tidak tersedia.

---
