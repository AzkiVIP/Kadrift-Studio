# Color Correction dan Color Grading
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
> **Terkait:** [Preset Library](PRESETS.md)
---## 15. Koreksi Warna dan Color Grading

Color workspace merupakan workspace bawaan tersendiri di top bar.

### 15.1 Penyesuaian Dasar

| ID | Adjustment |
|---|---|
| COL-01 | Exposure |
| COL-02 | Brightness |
| COL-03 | Contrast |
| COL-04 | Pivot |
| COL-05 | Highlights |
| COL-06 | Shadows |
| COL-07 | Whites |
| COL-08 | Blacks |
| COL-09 | Saturation |
| COL-10 | Temperature |
| COL-11 | Tint |
| COL-12 | Vibrance |
| COL-13 | Midtone Detail |
| COL-14 | Color Boost |
| COL-15 | Reset Color |

### 15.2 Color Wheels

- Lift
- Gamma
- Gain
- Offset
- Log Wheels
- HDR Wheels
- Luminance controls
- Saturation controls
- Numeric controls
- Keyframe pada parameter yang mendukung animasi

### 15.3 Curves

- Custom RGB Curves
- Red Curve
- Green Curve
- Blue Curve
- Luma Curve
- Hue vs Hue
- Hue vs Saturation
- Hue vs Luminance
- Luminance vs Saturation
- Saturation vs Saturation
- Add/delete curve points
- Reset selected curve
- Keyframe pada parameter kurva yang dirancang untuk animasi

### 15.4 Alat Warna Lanjutan

- RGB Mixer
- HSL Qualifier
- Color Isolation
- Color Warper
- Color Windows / Power Windows
- Color Tracking (advanced)
- LUT Import / Apply / Strength
- Color Presets
- Copy/Paste Grade
- Before/After
- Color Management / Color Space transforms
- HDR grading controls

### 15.5 Video Scopes

- Waveform
- RGB Parade
- Vectorscope
- Histogram
- Scope zoom
- Multi-scope layout
- Scope visibility and dockability

---
