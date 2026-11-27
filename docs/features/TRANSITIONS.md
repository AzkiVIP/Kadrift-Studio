# Transitions dan Custom Transition Builder
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
> **Terkait:** [Effects dan 2D VFX](EFFECTS_AND_2D_VFX.md) · [Preset Library](PRESETS.md)
---### 18.2 Pembuat Transisi Kustom

- Membuat transisi dari dua keadaan visual/clip.
- Mengatur durasi dan alignment.
- Menggunakan opacity, transform, mask, shape, blur, color, warp, dan built-in effects.
- Mengatur keyframe start/end dan kurva.
- Mengatur arah, timing, easing, dan overlap.
- Mendukung audio crossfade untuk pasangan audio yang relevan.
- Preview A/B pada dua clip.
- Menyimpan menjadi custom transition preset.
- Import/export preset.

---

## 19. Pustaka Transisi

Transitions adalah operasi perubahan antar-clip/visual sepanjang waktu. Transitions memiliki browser, kategori, preview, dan pengelolaan preset sendiri; jangan mencampurnya sebagai efek biasa di Effects Browser.

| ID | Fitur | Persyaratan |
|---|---|---|
| TRN-01 | Separate Transition Browser | Browser transisi terpisah dari Effects Browser. |
| TRN-02 | Transition Categories | Memisahkan video transitions, audio transitions, dan custom transitions. |
| TRN-03 | Search/Favorites | Mencari transisi dan menandai favorit. |
| TRN-04 | Timeline Application | Drag/drop atau apply command menempatkan transisi pada boundary/target yang kompatibel. |
| TRN-05 | Duration/Alignment | Durasi, alignment, direction, dan parameter transisi dapat diatur. |
| TRN-06 | A/B Preview | Preview transisi menggunakan dua clip/visual yang bersebelahan. |
| TRN-07 | Custom Transition Link | Custom transitions dibuat melalui Builder pada Bagian 18.2 dan ditampilkan di browser ini. |
| TRN-08 | Transition Presets | Menyimpan, menerapkan, menggandakan, mengimpor, dan mengekspor konfigurasi transisi melalui Preset Library. |

Baseline transition set:

- Cross Dissolve
- Fade In
- Fade Out
- Dip to Black
- Dip to White
- Dip to Color
- Wipe
- Slide
- Push
- Zoom
- Blur
- Iris
- Audio Crossfade
- Custom Transition

Setiap transition mendukung durasi, alignment, parameter, preview, preset, dan keyframe bila parameter tersebut mendukung animasi.

---
