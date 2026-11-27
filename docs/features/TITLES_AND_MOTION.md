# Title, Teks, dan Motion
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
> **Terkait:** [Effects dan 2D VFX](EFFECTS_AND_2D_VFX.md) · [Preset Library](PRESETS.md)
---## 17. Title, Teks, dan Animasi Gerak

Text harus menjadi objek timeline yang dapat diatur seperti objek visual lain.

### 17.1 Kontrol Teks

- Text editor
- Font family dan custom fonts
- Font size, weight, style
- Letter spacing dan line spacing
- Alignment
- Text color dan gradient
- Outline/stroke
- Shadow
- Background/box
- Padding dan corner radius untuk text box
- Position, scale, rotation, anchor
- Opacity dan blend mode
- Text presets
- Lower thirds
- Title templates
- Custom title presets
- Text clipping/mask/shape interaction
- Corner Pin dan Mesh Warp untuk text

### 17.2 Sistem Animasi Teks

| Animation | Perilaku |
|---|---|
| Typewriter | Huruf muncul satu per satu seperti sedang diketik. |
| Character Reveal | Reveal per karakter. |
| Word Reveal | Reveal per kata. |
| Line Reveal | Reveal per baris. |
| Fade In/Out | Opacity transition. |
| Slide In/Out | Bergerak dari arah tertentu. |
| Scale/Pop | Perubahan skala/pop. |
| Tracking Animation | Perubahan letter spacing. |
| Blur Reveal | Blur berkurang saat teks muncul. |
| Mask/Wipe Reveal | Teks muncul melalui mask/wipe. |
| Bounce | Gerakan pantul dengan preset kurva. |
| Rotate | Animasi rotasi. |
| Stagger | Delay antar karakter/kata/baris. |

Pengaturan Typewriter dan reveal:

- Kecepatan/durasi
- Character/word/line mode
- Start/end range
- Direction/reverse
- Delay/stagger
- Optional cursor/caret untuk gaya typewriter
- Easing/interpolation
- Keyframe progress dan parameter animasi
- Save as animation preset

Semua animasi text harus tetap dapat diedit. Preset tidak boleh mengunci teks menjadi video hasil render kecuali pengguna memilih bake/render option.

### 17.3 Subtitle

- Subtitle Editor
- Manual subtitle creation
- SRT import/export
- VTT import/export
- ASS/SSA support bila kompatibel
- Subtitle timing
- Font, size, color, outline, background, placement
- Subtitle track/timeline objects
- Burn-in subtitles on export

**Tidak ada AI auto-caption atau speech-to-text.**

---
