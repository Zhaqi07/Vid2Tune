# Vid2Tune

Aplikasi web untuk:

- Mengonversi video (MP4/MOV/WebM) menjadi MP3 menggunakan CloudConvert.
- Mengonversi audio atau rekaman mikrofon menjadi teks menggunakan AssemblyAI.

Frontend disajikan sebagai halaman statis (Tailwind CDN + JavaScript murni). Backend Express bertindak sebagai proxy agar API key tidak pernah diekspos di browser.

---

## 1. Struktur Proyek

```
.
├── index.html        # UI utama (Tailwind CDN)
├── style.css         # Custom styling kecil (opsional)
├── script.js         # Logika frontend (upload, status, fetch API)
└── backend/
    ├── server.js     # Express proxy untuk CloudConvert & AssemblyAI
    ├── package.json  # Dependensi backend
    └── .env          # API key (tidak diarepokan)
```

---

## 2. Prasyarat

- Node.js ≥ 18 (untuk backend).
- Akun CloudConvert & AssemblyAI dengan API key aktif.
- Git (opsional tetapi direkomendasikan).

---

## 3. Konfigurasi Backend

1. Masuk ke direktori backend dan install dependensi:
   ```bash
   cd backend
   npm install
   ```
2. Buat file `.env` (jangan commit) berisi:
   ```
   CLOUDCONVERT_API_KEY=token_cloudconvert_kamu
   ASSEMBLYAI_API_KEY=token_assemblyai_kamu
   PORT=4000
   ```
3. Jalankan backend:
   ```bash
   npm run start
   ```
   Backend tersedia di `http://localhost:4000` dengan endpoint:
   - `POST /api/video-to-audio`: menerima field `file` (video).
   - `POST /api/audio-to-text`: menerima field `file` (audio).

> Tip: gunakan `npm run dev` (nodemon) saat pengembangan supaya server reload otomatis.

---

## 4. Menjalankan Frontend Lokal

1. Buka tab terminal baru (backend tetap berjalan).
2. Layani folder root secara statis, misalnya:
   ```bash
   cd ..
   npx serve
   ```
   atau pakai Live Server / http-server / Python `python -m http.server`.
3. Buka URL yang diberikan (biasanya `http://localhost:3000`).  
   `script.js` otomatis memanggil API backend `http://localhost:4000`. Jika backend ada di domain lain, set global `window.VID2TUNE_API_BASE` sebelum memasukkan `script.js` di `index.html`.

---

## 5. Cara Menggunakan

### Video → Audio

1. Klik atau drag video ke area “Video → Audio”.
2. Setelah file terbaca, tombol “Konversi ke MP3” akan aktif.
3. Klik tombol; status progres ditampilkan di bawahnya.
4. Begitu selesai, MP3 otomatis diunduh. Jika download tidak berjalan, periksa popup blocker atau jaringan.

### Audio → Teks

1. Unggah file audio (MP3/WAV/M4A) **atau** klik “Rekam Suara Langsung” untuk memakai mikrofon.
2. Begitu audio siap, tombol “Ubah ke Teks” aktif.
3. Klik tombol; backend akan mengunggah audio ke AssemblyAI, membuat transcript, dan menampilkan teks saat status `completed`.
4. Jika gagal (mis. API limit), pesan error ditampilkan langsung di UI.

---

## 6. Deployment

1. **Backend**

   - Deploy `backend/` ke layanan Node (Render, Railway, Fly.io, dll.).
   - Set environment variable yang sama dengan `.env`.
   - Catat URL backend hasil deploy (mis. `https://vid2tune-backend.onrender.com`).

2. **Frontend**
   - Deploy folder root sebagai static site (mis. Vercel, Netlify, GitHub Pages).
   - Jika backend berada di domain berbeda, sisipkan skrip konfigurasi di `index.html`:
     ```html
     <script>
       window.VID2TUNE_API_BASE = "https://vid2tune-backend.onrender.com";
     </script>
     <script src="script.js"></script>
     ```

---

## 7. Tips & Batasan

- **API key rahasia**: simpan hanya di backend/layanan hosting, jangan commit ke repo.
- **Limit CloudConvert**: akun gratis punya batas ukuran & waktu; gunakan file kecil saat demo.
- **Limit AssemblyAI**: pastikan akun masih punya kredit/kuota sebelum transkripsi panjang.
- **Ukuran file**: contoh implementasi ini memakai upload ke CloudConvert/AssemblyAI secara langsung; jaringan lambat akan memperpanjang waktu.
- **Keamanan**: karena backend menerima file upload, pertimbangkan validasi tambahan (ukuran maksimal, MIME type) sebelum dirilis ke publik.

---

## 8. Kontribusi

1. Fork repo ini.
2. Buat branch fitur: `git checkout -b fitur-baru`.
3. Commit perubahan: `git commit -m "Tambah fitur ..."`
4. Push dan ajukan Pull Request.

---

## 9. Lisensi

Tambahkan lisensi pilihanmu (MIT, GPL, dsb.). Bila belum ditentukan, repositori ini dianggap “All Rights Reserved”.

---

Selamat menggunakan Vid2Tune! Jika menemukan bug atau butuh fitur tambahan, silakan buat issue atau hubungi maintainer.
