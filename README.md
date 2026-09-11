# Lab Pesawat Sederhana — Virtual Lab IPA Kelas 8

Virtual lab interaktif (tuas, katrol, bidang miring, roda berporos) + tabel pengamatan + kuis HOTS.
Satu file HTML mandiri, **tanpa build, tanpa server, tanpa dependensi** — dan sekarang **siap disematkan (embed) di website mana pun**.

## File

| File | Fungsi |
|---|---|
| `index.html` | Aplikasi labnya. Ini yang dibuka/di-embed. |
| `embed.html` | Generator kode embed: pilih ukuran → salin `<iframe>` → tempel. Ada pratinjau langsung. |
| `lab-pesawat-sederhana.html` | Stub redirect ke `index.html` (kompatibilitas link lama). |
| `.nojekyll` | Mematikan pemrosesan Jekyll di GitHub Pages. |

## 1. Host dulu (sekali saja) — GitHub Pages

1. Buka repo di GitHub → **Settings** → **Pages**
2. **Source**: `Deploy from a branch`
3. **Branch**: `main` + folder `/ (root)` → **Save**
4. Tunggu ~1 menit. URL-nya:

```
https://mathiasbagas11-ai.github.io/Virtual-Lab-Pesawat-Sederhana/            → labnya
https://mathiasbagas11-ai.github.io/Virtual-Lab-Pesawat-Sederhana/embed.html  → generator kode embed
```

Alternatif hosting (semua gratis, tinggal drag-and-drop `index.html`): Netlify Drop, Vercel, Cloudflare Pages.
Syarat penting: host harus **HTTPS** dan tidak mengirim header `X-Frame-Options: DENY`. GitHub Pages aman di dua-duanya.

## 2. Sematkan di website

Cara cepat: buka `embed.html`, atur ukuran, klik **Salin kode**.

Atau pakai langsung snippet ini (URL-nya sudah sesuai repo ini):

**Responsif (disarankan — untuk website/WordPress/Blogger)**

```html
<div style="position:relative;width:100%;aspect-ratio:16 / 10;min-height:520px">
  <iframe src="https://mathiasbagas11-ai.github.io/Virtual-Lab-Pesawat-Sederhana/"
          title="Lab Pesawat Sederhana" loading="lazy"
          allowfullscreen allow="fullscreen; clipboard-write"
          style="position:absolute;inset:0;width:100%;height:100%;border:0;border-radius:16px"></iframe>
</div>
```

**Tinggi tetap (untuk Google Sites, Moodle, Canvas, LMS)**

```html
<iframe src="https://mathiasbagas11-ai.github.io/Virtual-Lab-Pesawat-Sederhana/"
        title="Lab Pesawat Sederhana" width="100%" height="760"
        allowfullscreen allow="fullscreen; clipboard-write"
        style="width:100%;height:760px;border:0"></iframe>
```

Per platform:

- **Google Sites** — Sisipkan → Sematkan → tab *Kode sematan* → tempel snippet tinggi tetap → Sisipkan.
- **Moodle** — di editor klik ikon `<>` (kode sumber), tempel. Pastikan filter HTML tidak membuang `iframe`.
- **Canvas / Schoology** — Rich Content Editor → Edit HTML.
- **WordPress** — blok *HTML Kustom*. **Blogger** — mode *HTML*.
- **Notion** — `/embed` lalu tempel URL langsung (bukan iframe).
- **Kalau LMS memblokir iframe** — bagikan URL langsung saja, atau jadikan QR code untuk siswa.

## 3. Dukungan embed yang sudah ditanam di `index.html`

- **Deteksi iframe otomatis** — muncul tombol **Tab baru** di header saat lab dijalankan di dalam website lain, supaya siswa bisa membuka versi layar penuh.
- **Layar penuh tahan banting** — kalau host tidak mengizinkan (`allowfullscreen` tidak ada), tombolnya tidak diam saja tapi memberi tahu apa yang harus dilakukan.
- **Auto-resize opsional** — lab mengirim tinggi kontennya ke halaman induk:

  ```js
  window.addEventListener('message', e => {
    const d = e.data;
    if (d && d.source === 'lab-pesawat-sederhana' && d.type === 'resize') {
      document.querySelector('#lab-frame').style.height = d.height + 'px';
    }
  });
  ```

  Event lain: `{source:'lab-pesawat-sederhana', type:'ready', url}` saat lab selesai dimuat.
- **Tinggi fleksibel** — `height:100%` + `100dvh` sebagai fallback, jadi lab mengisi ukuran iframe berapa pun; di layar sempit (<900px) otomatis beralih ke tata letak satu kolom yang bisa di-scroll.
- **Meta Open Graph** — link lab tampil rapi saat dibagikan di WhatsApp/grup guru.

## 4. Catatan teknis

- Font dari Google Fonts, tapi ada fallback `system-ui` — tetap terbaca kalau jaringan sekolah memblokir `fonts.googleapis.com`.
- Tombol **Salin tabel** memakai Clipboard API; di iframe lintas-domain butuh `allow="clipboard-write"` (sudah ada di snippet). Kalau tetap diblokir, tersedia fallback **Unduh CSV**.
- **Unduh CSV** butuh host yang tidak memakai `sandbox` tanpa `allow-downloads`.
