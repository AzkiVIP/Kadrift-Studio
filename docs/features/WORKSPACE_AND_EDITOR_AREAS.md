# Workspace dan Editor Areas
> **Status:** Spesifikasi produk; bukan bukti bahwa fitur sudah diimplementasikan.
> **Sumber kebenaran:** Dokumen ini adalah sumber detail untuk domain yang disebutkan. Indeks dan aturan global berada di [VIDEO_EDITOR_MASTER_SPEC.md](../../VIDEO_EDITOR_MASTER_SPEC.md).
> **Prinsip migrasi:** Nomor bagian dan seluruh requirement ID dipertahankan agar tetap dapat ditelusuri.
---## 3. Tab Workspace — Bar Paling Atas

**Ini adalah navigasi workspace utama**, bukan sekadar tab Media/Effects di panel samping. Posisi default ada di sisi atas aplikasi, terinspirasi dari workspace tabs Blender.

### 3.1 Workspace Bawaan

| Workspace | Tujuan | Susunan awal yang disarankan |
|---|---|---|
| Edit | Editing sehari-hari | Media Browser, Source Monitor/Viewer, Program Viewer, Inspector, Timeline |
| Effects | Mencari, menyusun, mengatur, dan membuat effects/transitions | Effects Browser, Transitions Browser, Generators Browser, Preset Library, Viewer, Effect Stack/Inspector, Timeline, Keyframe Lane |
| Color | Color correction dan grading | Viewer, Color Controls, Scopes, Timeline, Color Inspector |
| Audio | Editing dan mixing audio | Timeline, Audio Mixer, Waveform/Source Monitor, Audio Inspector |
| Titles & Motion | Text, shapes, 2D/2.5D visual composition, dan animasi | Titles/Shapes Library, 2D Visual Composition Editor, 2.5D Motion Graphics Editor, Viewer, Inspector, Timeline, Keyframe/Graph Editor |
| Export | Setup output dan render jobs | Export Settings, Render Queue, Preview, Job Details |

`Plugin Development` dapat disediakan sebagai workspace tambahan/opsional untuk developer, bukan workspace utama untuk editor biasa.

### 3.2 Pengelolaan Workspace

| ID | Fitur | Persyaratan |
|---|---|---|
| WS-01 | Workspace Tabs Top Bar | Semua workspace utama tampil sebagai tab di bagian atas. |
| WS-02 | Default Workspaces | Edit, Effects, Color, Audio, Titles & Motion, Export. |
| WS-03 | Add Workspace | Pengguna dapat menambahkan workspace baru. |
| WS-04 | Rename Workspace | Pengguna dapat mengganti nama workspace custom. |
| WS-05 | Duplicate Workspace | Menggandakan workspace termasuk susunan area. |
| WS-06 | Delete Custom Workspace | Menghapus workspace custom dengan konfirmasi. |
| WS-07 | Reorder Tabs | Mengubah urutan workspace tabs. |
| WS-08 | Save Per-Workspace Layout | Setiap tab menyimpan layout independen. |
| WS-09 | Reset Current Workspace | Mengembalikan hanya workspace/tab aktif ke layout default-nya. |
| WS-10 | Reset One Workspace | Memilih satu workspace untuk di-reset meskipun tidak sedang aktif. |
| WS-11 | Reset All Workspaces | Mengembalikan semua workspace ke layout default. Wajib ada konfirmasi. |
| WS-12 | Workspace Presets | Menyediakan layout standar dan custom presets. |
| WS-13 | Workspace Import/Export | Menyimpan dan memindahkan layout custom. |
| WS-14 | Remember Last Workspace | Membuka workspace terakhir yang digunakan. |
| WS-15 | Workspace Recovery | Memulihkan workspace/layout jika aplikasi crash. |
| WS-16 | Workspace Search | Mencari editor/panel dan menempatkannya di workspace. |

Reset layout tidak boleh menghapus project, media, effect presets, plugin, atau user data lainnya.

---

---

## 4. Area Editor Modular dan Sistem Docking

Setiap workspace memiliki editor areas yang dapat diubah susunannya.

| ID | Fitur | Persyaratan |
|---|---|---|
| UI-01 | Resizable Areas | Area dapat diperbesar/diperkecil dengan drag divider. |
| UI-02 | Horizontal Split | Membagi area secara horizontal. |
| UI-03 | Vertical Split | Membagi area secara vertikal. |
| UI-04 | Move Area | Memindahkan area ke posisi lain. |
| UI-05 | Swap Editors | Menukar isi dua area. |
| UI-06 | Tabbed Editors | Beberapa editor dapat berada pada area yang sama dalam bentuk tab. |
| UI-07 | Floating Editor Window | Editor dapat dilepas menjadi window terpisah. |
| UI-08 | Redock | Floating window dapat digabungkan kembali. |
| UI-09 | Dock Preview | Menampilkan target docking sebelum dilepas. |
| UI-10 | Maximize Area | Memperbesar satu area memenuhi ruang workspace. |
| UI-11 | Restore Area | Mengembalikan ukuran dan susunan sebelumnya. |
| UI-12 | Multi-monitor | Floating editor dapat dipindah ke monitor lain. |
| UI-13 | Fullscreen Editor | Editor dapat memenuhi layar. |
| UI-14 | Hide/Show Area | Menyembunyikan dan menampilkan area. |
| UI-15 | Save Layout | Menyimpan layout untuk workspace aktif. |
| UI-16 | Reset Layout | Reset layout tanpa mengubah data project. |
| UI-17 | Responsive Areas | Area menyesuaikan ukuran jendela dan ruang tersedia. |
| UI-18 | UI Scale | Mengubah skala interface. |
| UI-19 | Focus Area | Area aktif jelas dan menerima shortcut yang sesuai. |
| UI-20 | Keyboard Navigation | Area dan tab dapat diakses melalui keyboard. |

### 4.1 Jenis editor yang dapat ditempatkan di area

- Media Browser / Project Bin
- Source Monitor
- Program Viewer / Video Preview
- Timeline
- Inspector
- Effects Browser
- Transitions Browser
- Generators Browser
- Preset Browser/Library
- Titles & Shapes Browser
- 2D Visual Composition Editor
- 2.5D Motion Graphics Editor
- Keyframe Timeline / Keyframe Lane
- Graph Editor
- Color Editor
- Video Scopes
- Audio Mixer
- Audio Waveform Editor
- Render Queue
- Performance Monitor
- Plugin Manager
- Plugin Development Tools
- Documentation / Help Browser
- Project Settings
- History / Undo History

### 4.2 Default layout pada workspace Edit

Default awal (bukan susunan permanen):

- **Left area:** Media, Effects, Transitions, Titles/Shapes dapat dibuka sebagai tab atau editor yang ditukar.
- **Center area:** Viewer/Preview utama.
- **Right area:** Inspector yang mengikuti objek terpilih.
- **Bottom area:** Timeline.
- **Opsional:** Source Monitor dapat berbagi tab dengan Viewer atau ditempatkan di area terpisah.

Susunan harus bisa di-customize tanpa memindahkan project data.

---


### 4.3 Editor Type Switcher — Requirement Wajib

Setiap **Editor Area** memiliki kontrol **Editor Type Switcher** pada header area, mengikuti pola interaksi Blender. Switcher mengganti jenis editor di area yang sama; bukan Workspace Tab dan bukan aksi menambahkan editor sebagai tab.

- Menampilkan ikon dan nama jenis editor aktif.
- Membuka daftar editor yang tersedia; daftar panjang sebaiknya dapat dicari dan dikelompokkan.
- Memilih jenis editor mengganti konten area yang sama tanpa membuat area baru atau berpindah workspace.
- Contoh: Program Viewer → Graph Editor; Timeline → Audio Mixer; Media Browser → Effects Browser.
- Jika area berisi beberapa editor bertab, switcher mengganti tipe editor yang aktif. Menambah editor sebagai tab tetap menjadi aksi terpisah.
- Pergantian editor tidak menghapus project data; layout tetap tersimpan per workspace.
- Dapat dioperasikan dengan mouse dan keyboard serta memiliki tooltip, focus state, dan Shift Help.

| ID | Fitur | Persyaratan |
|---|---|---|
| UI-21 | Editor Type Switcher | Mengganti jenis editor di area aktif yang sama; berbeda dari Workspace Tabs dan Tabbed Editors. |

---

## 5. Bantuan Kontekstual Shift dan Kemudahan Menemukan Fitur

Pengguna meminta cara menahan Shift pada fitur/tombol untuk mengetahui penjelasan lebih lanjut.

| ID | Fitur | Persyaratan |
|---|---|---|
| HELP-01 | Shift + Hover | Ketika Shift ditahan dan pointer berada di kontrol, tampilkan bantuan kontekstual. |
| HELP-02 | Short Description | Penjelasan singkat fungsi kontrol. |
| HELP-03 | Detailed Help | Cara penggunaan dan contoh. |
| HELP-04 | Parameter Explanation | Arti parameter, satuan, dan efek perubahan nilai. |
| HELP-05 | Shortcut Hint | Shortcut yang berlaku. |
| HELP-06 | Related Controls | Tautan ke fitur terkait. |
| HELP-07 | Pinned Help | Bantuan dapat dibuka di panel Help terpisah. |
| HELP-08 | Keyboard Accessibility | Help dapat dipanggil saat kontrol difokuskan melalui keyboard. |
| HELP-09 | Touch Alternative | Tombol/info icon alternatif untuk perangkat tanpa keyboard. |

Aturan interaksi:

- Shift + Hover/Focus adalah perilaku utama.
- Jangan gunakan Shift + Click sebagai mekanisme utama karena Shift + Click biasa dipakai untuk multi-selection.
- Bantuan tidak boleh menutupi kontrol secara permanen atau menghalangi drag.
- Tooltip biasa tetap boleh tampil untuk label singkat.

---
