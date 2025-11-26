# Vid2Tune

Web app untuk mengonversi video → audio (CloudConvert) dan audio → teks (AssemblyAI). Frontend dibuat dengan HTML + Tailwind CDN, backend proxy sederhana berbasis Express menangani API key agar tetap aman.

## Struktur Proyek

- `index.html`, `style.css`, `script.js` — UI dan interaksi browser.
- `backend/` — server Express + endpoint CloudConvert & AssemblyAI.

## Menjalankan Secara Lokal

1. **Clone / buka repo** di `C:\Users\zhaqi\Documents\WEBSITE SISMUL CONVERTER`.
2. **Konfigurasi backend:**
   - Masuk ke dashboard CloudConvert dan AssemblyAI, buat API key baru.
   - Salin file contoh: `cd backend && copy env.example .env` (jika file contoh sudah dihapus, buat `.env` manual).
   - Isi `.env`:
     ```
     CLOUDCONVERT_API_KEY=isi_dengan_token_cloudconvert
     ASSEMBLYAI_API_KEY=isi_dengan_token_assemblyai
     PORT=4000
     ```
3. **Install dependencies backend:**
   ```bash
   cd backend
   npm install
   ```
4. **Jalankan backend proxy:**
   ```bash
   npm run start
   ```
   Server aktif di `http://localhost:4000` dan menyediakan endpoint:
   - `POST /api/video-to-audio` (form-data field `file`)
   - `POST /api/audio-to-text` (form-data field `file`)

5. **Layani frontend secara statis** di terminal lain:
   ```bash
   cd "C:\Users\zhaqi\Documents\WEBSITE SISMUL CONVERTER"
   npx serve
   ```
   Buka browser ke alamat yang ditampilkan (mis. `http://localhost:3000`).

## Cara Pakai

- **Video → Audio:** pilih/drag file MP4/MOV/WebM → klik “Konversi ke MP3”. Backend akan membuat job CloudConvert, menunggu selesai, lalu mengunduh MP3.
- **Audio → Teks:** unggah audio atau rekam langsung. Tombol “Ubah ke Teks” akan mengunggah audio ke AssemblyAI, membuat transcript, dan menampilkan teks saat status `completed`.
- Status proses tampil di bawah masing-masing tombol; jika gagal, pesan error langsung berasal dari backend/API.

## Catatan Penting

- API key **jangan** di-commit; simpan hanya di `.env` lokal/server.
- Untuk produksi, deploy backend (mis. Render/Heroku) lalu set `window.VID2TUNE_API_BASE` pada frontend jika domain berbeda.
- CloudConvert gratis punya batas ukuran/durasi; AssemblyAI juga punya kuota. Pantau dashboard masing-masing jika sering gagal karena limit.

