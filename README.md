# Master Prompt Architect

Aplikasi web satu file yang mengubah ide singkat menjadi prompt lengkap dengan enam bagian: **Role, Task, Context, Reasoning, Stop conditions, dan Output**. Strukturnya mengikuti kerangka "The Anatomy of a Claude Prompt".

Contoh: dari "Buatkan rencana peluncuran 90 hari untuk aplikasi AI task manager" menjadi prompt siap tempel yang punya persona, batasan, kriteria selesai, dan format keluaran yang jelas.

## Fitur

- **Generate dengan AI**: tulis ide singkat, pilih bahasa (Indonesia atau English) dan tingkat ketajaman, lalu klik *Bangun master prompt*.
- **Tiga tampilan hasil**
  - *Visual*: poster anatomi dengan garis warna per bagian.
  - *Edit*: ubah tiap bagian secara manual.
  - *Teks utuh*: hasil gabungan yang siap disalin.
- **Format salinan**: polos, dengan tag XML (`<role>`, `<task>`, dan seterusnya), atau dengan judul Markdown (`###`).
- **Salin per bagian** atau seluruh prompt sekaligus.
- **Contoh cepat**: empat ide siap pakai.
- **Riwayat**: 10 hasil terakhir tersimpan di browser (localStorage).
- **Tema otomatis** mengikuti mode terang atau gelap perangkat.
- **Pintasan**: `Ctrl/Cmd + Enter` di kolom ide untuk generate.

## Cara pakai

1. Buka halaman aplikasi.
2. Tulis ide di kolom *Tulis ide atau prompt singkat*. Konteks tambahan (misalnya ukuran tim atau budget) bersifat opsional.
3. Pilih bahasa dan tingkat ketajaman.
4. Klik **Bangun master prompt** dan setujui izin akses AI jika diminta.
5. Periksa hasilnya di tab *Visual*, sesuaikan di tab *Edit* bila perlu, lalu salin dari tab *Teks utuh* atau tombol *Salin*.

## Struktur proyek

```
master-prompt-architect.html   # seluruh aplikasi (HTML + CSS + JavaScript)
README.md
```

Tidak ada build step, framework, atau dependensi npm. Satu-satunya sumber eksternal adalah font Google (Newsreader, Inter, JetBrains Mono).

## Catatan penting soal fitur AI

Tombol generate memakai fitur `sample` dari runtime Artifact claude.ai (`claude.use('sample')`), jadi **tidak memerlukan API key, tetapi hanya berfungsi saat halaman dibuka lewat claude.ai**.

Jika file HTML ini dihosting di tempat lain (GitHub Pages, Netlify, server sendiri), tampilan, editor, riwayat, dan tombol salin tetap jalan. Tombol generate akan menampilkan pesan bahwa fitur AI belum tersedia.

Untuk menghidupkan generate di luar claude.ai, ganti pemanggilan di fungsi `generate()` dengan request ke backend milikmu. Jangan menaruh API key langsung di file HTML karena akan terlihat oleh semua pengunjung. Bagian yang perlu diganti:

```js
const sample = await claude.use('sample');
const r = await sample.json(brief, { modelTier: 'default', cache: false });
```

Hasilnya harus berupa objek JSON dengan tujuh kunci bertipe string: `analysis`, `role`, `task`, `context`, `reasoning`, `stopConditions`, `output`. Variabel `brief` sudah berisi instruksi lengkap, jadi cukup kirim ke model pilihanmu dan kembalikan JSON-nya.

## Kustomisasi

| Yang ingin diubah | Lokasi di file |
| --- | --- |
| Warna tiap bagian dan tema | variabel CSS di `:root` (`--c-role`, `--c-task`, dst.) |
| Nama, deskripsi, dan placeholder bagian | array `S` |
| Contoh cepat | array `PRE` |
| Instruksi ke AI | string `brief` di dalam `generate()` |
| Jumlah riwayat | `slice(0, 10)` di dalam `generate()` |

## Privasi

Riwayat prompt hanya disimpan di localStorage browser kamu dan bisa dihapus lewat tombol *Hapus semua* di panel Riwayat.
