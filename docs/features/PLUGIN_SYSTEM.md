# Plugin System, Plugin Creator, dan SDK
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
---## 21. Sistem Plugin dan Pembuatan Plugin

Aplikasi perlu mendukung penggunaan plugin dan menyediakan workflow untuk membuat plugin. Plugin system adalah extensibility layer, bukan sekadar halaman daftar plugin.

### 21.1 Jenis Plugin

- Video effects
- Transitions
- Audio effects/processors
- Titles/motion presets
- Generators (misalnya shape/pattern/graphic generator)
- Importers/exporters
- Custom panels/editors/workspace tools
- Custom masks/tools bila API mendukung
- Metadata/utility tools

### 21.2 Pengelola Plugin

- Browse installed plugins
- Install/uninstall
- Enable/disable
- Plugin version and author
- Compatibility status
- Dependency detection
- Update notification
- Plugin permissions/capabilities
- Plugin logs/errors
- Safe mode (start app with third-party plugins disabled)
- Load/reload plugin while developing when supported

### 21.3 Pembuat Plugin / Alat Developer

- New Plugin wizard
- Plugin template selection by plugin type
- Plugin ID/name/version/author/description/license
- SDK/API reference
- Parameter declaration and UI controls
- Preview/test harness with sample media
- Development reload/hot reload where technically safe
- Debug logs and validation
- Build/package plugin
- Compatibility validation
- Example plugins and starter templates
- Plugin documentation generator or template

### 21.4 SDK Plugin dan Kompatibilitas

- Define a stable, versioned Plugin API.
- Prefer a sandboxed/cross-platform interface such as WebAssembly/WIT for suitable extensions, subject to technical validation.
- GPU custom effects may later expose a controlled shader interface such as WGSL, with validation and resource limits.
- Native plugins require strict ABI/version strategy; do not casually expose unstable internal Rust ABI.
- Third-party plugins must not be allowed to corrupt project files or access unrestricted resources without clear permission.
- Plugins must declare required app/API version and supported platform.
- Plugin failure should not crash the entire editor where isolation is possible.
- Document performance expectations and plugin lifecycle.

**Keputusan arsitektur plugin harus diuji melalui technical spike sebelum dijadikan kontrak permanen.** Jangan mengklaim plugin cross-platform atau sandbox aman sebelum diuji.

---
