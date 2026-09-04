# 🚀 Free Cloud VPS / Linux Web Desktop (Ubuntu + noVNC)

Proyek ini berisi konfigurasi lengkap untuk membuat **VPS / Server Linux Ubuntu dengan Tampilan Desktop (GUI XFCE4) & Terminal Root** yang dapat diakses langsung lewat browser (HTML5 Web VNC) dan di-deploy secara gratis di berbagai platform cloud (seperti **Railway.app**, **Render**, atau **Koyeb**) tanpa kartu kredit.

---

## 📁 Struktur File Proyek

```text
free-vps-railway/
├── Dockerfile              # Setup Ubuntu 22.04, XFCE4, noVNC, & utilities
├── startup.sh              # Script inisialisasi Xvfb, Desktop, & Websockify
├── railway.json            # Konfigurasi deploy Railway
├── docker-compose.yml      # Konfigurasi jika ingin dijalankan di Docker lokal
├── keep_alive.py           # Script monitoring agar server tidak sleep
└── .github/
    └── workflows/
        └── keep_alive.yml  # GitHub Actions otomatis untuk ping 24/7
```

---

## ⚡ Langkah-Langkah Deploy (Cepat & Praktis)

### Langkah 1: Upload / Push Kode ke Akun GitHub Anda
1. Buka terminal (PowerShell atau Command Prompt) di folder `free-vps-railway`.
2. Jalankan perintah berikut untuk menginisialisasi repository:
   ```bash
   git init
   git add .
   git commit -m "Initial commit Free VPS"
   ```
3. Buat repositori baru di [GitHub.com](https://github.com/new) (disarankan akun GitHub sekunder/dummy), beri nama misalnya `free-vps-railway`.
4. Hubungkan dan push kode ke GitHub:
   ```bash
   git remote add origin https://github.com/USERNAME_GITHUB/free-vps-railway.git
   git branch -M main
   git push -u origin main
   ```

---

### Langkah 2: Deploy di Railway.app
1. Buka [Railway.app](https://railway.app) dan login dengan akun GitHub Anda.
2. Klik tombol **New Project** -> pilih **Deploy from GitHub repo**.
3. Pilih repositori `free-vps-railway` yang tadi Anda upload.
4. Masuk ke menu **Settings** pada service project Anda di Railway:
   - **Networking / Public Networking**: Klik **Generate Domain** (Railway akan membuatkan domain publik seperti `https://free-vps-production.up.railway.app`).
   - **Service Settings / Region**: Pilih **Singapore** (Asia Tenggara) agar koneksi lancar dan latensi rendah.
   - **Port**: Pastikan port terhubung ke `6080` (secara default Railway otomatis mendeteksi port dari Dockerfile/aplikasi).
5. Tunggu proses **Build & Deploy** beberapa menit sampai statusnya hijau (**Active / Success**).

---

### Langkah 3: Akses VPS Melalui Web Browser
1. Buka Public Domain yang sudah di-generate dari Railway pada browser Anda:
   ```text
   https://NAMA-PROJECT-ANDA.up.railway.app/vnc.html
   ```
   *(Atau cukup buka link utamanya, halaman otomatis terhubung ke tampilan noVNC).*
2. Klik tombol **Connect**.
3. Anda langsung masuk ke desktop **Ubuntu XFCE4** dengan hak akses **Root**!
4. Buka terminal di dalam desktop VPS untuk mulai menginstal tools atau menjalankan script:
   ```bash
   # Cek spesifikasi
   neofetch

   # Update repositori & paket
   sudo apt update
   ```

---

### Langkah 4: Trik Menjaga VPS Online 24 Jam Nonstop

Layanan cloud gratis biasanya akan mematikan container jika tidak ada traffic masuk. Untuk menjaga VPS tetap aktif terus menerus:

#### Opsi A: Menggunakan Layanan Uptime Monitor (Paling Mudah)
1. Daftar gratis di [cron-job.org](https://cron-job.org) atau [UptimeRobot.com](https://uptimerobot.com).
2. Buat monitor baru:
   - **URL**: Masukkan Public URL Railway Anda (contoh: `https://free-vps.up.railway.app`).
   - **Interval**: Set setiap **2 menit** atau **5 menit**.
3. Monitor akan mengirim request berkala sehingga server selalu mendeteksi aktivitas dan tidak masuk mode *sleep*.

#### Opsi B: Menggunakan GitHub Actions (Sudah Disiapkan di Repo)
1. Buka repositori GitHub Anda -> masuk ke tab **Settings** -> **Secrets and variables** -> **Actions**.
2. Klik **New repository secret**:
   - **Name**: `VPS_URL`
   - **Secret**: Masukkan Public URL Railway Anda.
3. GitHub Actions otomatis melakukan ping setiap 5 menit via workflow [`.github/workflows/keep_alive.yml`](.github/workflows/keep_alive.yml).

---

## 🛠️ Alternatif Layanan Cloud Lainnya
Selain Railway, repositori ini juga bisa di-deploy di platform gratis lainnya:
* **Render.com**: Buat *Web Service* baru -> Deploy dari GitHub.
* **Koyeb.com**: Buat *App* baru -> Deploy Docker / GitHub repo.
* **Hugging Face Spaces**: Buat Space baru dengan SDK Docker.
