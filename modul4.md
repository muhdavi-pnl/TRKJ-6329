# Modul Praktikum 4 - Cloud Integration

---

## Tujuan Pembelajaran

1. Memahami konsep cloud computing dan model layanan IaaS, PaaS, dan SaaS.
2. Melakukan deployment aplikasi web/API menggunakan platform cloud.
3. Mengintegrasikan aplikasi cloud dengan layanan yang berjalan pada server lokal (on-premise).
4. Memahami mekanisme komunikasi antara layanan lokal dan cloud.
5. Menerapkan prinsip keamanan dasar dalam komunikasi hybrid cloud.

---

## Bahan Kajian

* Konsep cloud computing.
* Model layanan IaaS, PaaS, dan SaaS.
* Cloud application deployment.
* Platform deployment Render.
* Hybrid cloud integration.
* Cloudflare Tunnel.
* REST API dan komunikasi antarserver.
* Keamanan komunikasi cloud dan on-premise.

---

## Alat & Bahan

* Docker & Docker Compose.
* Node.js/Python (sebagai aplikasi layanan).
* Database (MySQL/PostgreSQL).
* Internet + terminal Linux.

---

## **3. Dasar Teori**

### **A. Cloud Computing**

Cloud computing merupakan model penyediaan sumber daya komputasi melalui jaringan internet sehingga pengguna dapat memanfaatkan server, penyimpanan, database, dan aplikasi tanpa harus memiliki seluruh infrastruktur secara fisik.

Tiga model layanan cloud:

- **IaaS (Infrastructure as a Service):** menyediakan infrastruktur virtual seperti server, jaringan, dan penyimpanan.
- **PaaS (Platform as a Service):** menyediakan lingkungan untuk membangun dan menjalankan aplikasi tanpa harus mengelola seluruh infrastruktur server.
- **SaaS (Software as a Service):** menyediakan aplikasi siap pakai melalui internet.

### **B. Cloud Deployment**

Deployment merupakan proses memindahkan aplikasi dari lingkungan pengembangan ke lingkungan server agar dapat diakses oleh pengguna.

Pada praktikum ini, mahasiswa menggunakan Render sebagai platform deployment aplikasi berbasis Node.js.

### **C. Hybrid Cloud Integration**

Hybrid cloud merupakan pendekatan yang menggabungkan infrastruktur lokal (on-premise) dengan layanan cloud.

Contoh arsitektur:

**Cloud Environment**

Render – Web Application / REST API

HTTPS melalui Cloudflare Tunnel

**On-Premise Environment**

Server lokal – REST API – Database

Gambaran arsitektur hybrid cloud praktikum

Dengan pendekatan ini, aplikasi yang berada di cloud dapat berkomunikasi dengan layanan yang berjalan di server lokal.

## **4. Alat dan Bahan**

| No. | Alat/Bahan                | Keterangan                    |
| --- | ------------------------- | ----------------------------- |
| 1   | Laptop/PC                 | Komputer praktikum            |
| 2   | Ubuntu/Linux atau Windows | Server lokal                  |
| 3   | Node.js dan npm           | Menjalankan aplikasi          |
| 4   | Git dan GitHub            | Pengelolaan source code       |
| 5   | Akun Render               | Deployment aplikasi           |
| 6   | Cloudflare Tunnel         | Koneksi server lokal ke cloud |
| 7   | MySQL/PostgreSQL          | Database lokal                |
| 8   | Browser                   | Pengujian aplikasi            |
| 9   | Internet                  | Koneksi cloud                 |

## **5. Langkah-Langkah Praktikum**

### **Langkah 1 – Membuat Aplikasi Web/API Sederhana**

Pada tahap ini mahasiswa membuat aplikasi API sederhana menggunakan Node.js.

1. Pastikan Node.js dan npm sudah terinstal.
2. Buat direktori proyek:

```
mkdir cloud-integration
cd cloud-integration
npm init -y
```

3\. Instal Express:

```
npm install express
```

4\. Buat file `server.js`:

```
const express = require('express');
const app = express();

app.use(express.json());

app.get('/', (req, res) => {
    res.send('Cloud Integration API');
});

app.get('/api/data', (req, res) => {
    res.json({
        status: 'success',
        message: 'Data berhasil diakses',
        source: 'Cloud Application'
    });
});

const PORT = process.env.PORT || 3000;

app.listen(PORT, '0.0.0.0', () => {
    console.log(`Server berjalan pada port ${PORT}`);
});
```

5\. Tambahkan script berikut pada `package.json`:

```
"scripts": {
    "start": "node server.js"
}
```

6\. Jalankan aplikasi:

```
npm start
```

7\. Buka browser:

```
http://localhost:3000
http://localhost:3000/api/data
```

**Hasil yang diharapkan:** Aplikasi API berjalan dan dapat memberikan respons JSON.

### **Langkah 2 – Mengunggah Source Code ke GitHub**

1. Buat repository baru di GitHub dengan nama `cloud-integration`.
2. Inisialisasi Git:

```
git init
git add .
git commit -m "Initial cloud integration app"
```

3\. Hubungkan repository lokal dengan GitHub:

```
git remote add origin https://github.com/username/cloud-integration.git
git branch -M main
git push -u origin main
```

4\. Pastikan source code sudah tersedia pada repository GitHub.

### **Langkah 3 – Deployment Aplikasi Menggunakan Render**

1. Buka [website Render](https://render.com/?utm_source=chatgpt.com).
2. Buat akun atau login.
3. Hubungkan akun GitHub.
4. Pilih menu **New → Web Service**.
5. Pilih repository `cloud-integration`.
6. Isi konfigurasi deployment:

| Parameter     | Nilai             |
| ------------- | ----------------- |
| Name          | cloud-integration |
| Runtime       | Node              |
| Branch        | main              |
| Build Command | npm install       |
| Start Command | npm start         |
| Instance Type | Free              |

7. Klik **Deploy Web Service**.
8. Tunggu hingga proses deployment selesai.
9. Buka URL aplikasi yang diberikan Render.

Contoh:

```
https://cloud-integration.onrender.com
```

10\. Uji endpoint:

```
https://cloud-integration.onrender.com/api/data
```

**Hasil yang diharapkan:** Aplikasi berhasil diakses melalui internet.

Catatan: Nama domain di atas hanya contoh. URL aktual diberikan oleh Render. Free service dapat mengalami cold start setelah tidak aktif.

### **Langkah 4 – Menyiapkan Layanan On-Premise**

Pada tahap ini mahasiswa membuat layanan API yang berjalan pada komputer lokal.

1\. Buat direktori:

```
mkdir local-service
cd local-service
npm init -y
npm install express
```

2\. Buat file `server.js`:

```
const express = require('express');
const app = express();

app.get('/api/local-data', (req, res) => {
    res.json({
        status: 'success',
        source: 'On-Premise Server',
        device: 'Local Server',
        temperature: 29,
        humidity: 75
    });
});

app.listen(3001, '0.0.0.0', () => {
    console.log('Local API berjalan pada port 3001');
});
```

3\. Tambahkan script `start` pada `package.json`:

```
"scripts": {
    "start": "node server.js"
}
```

4\. Jalankan aplikasi:

```
npm start
```

5\. Uji API melalui browser:

```
http://localhost:3001/api/local-data
```

Jika berhasil, akan muncul data JSON dari server lokal.

### **Langkah 5 – Menghubungkan Server Lokal Menggunakan Cloudflare Tunnel**

Cloudflare Tunnel digunakan agar layanan API lokal dapat diakses melalui koneksi HTTPS tanpa membuka port inbound pada router.

1. Instal `cloudflared` sesuai sistem operasi melalui [panduan instalasi Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/downloads/?utm_source=chatgpt.com).
2. Pastikan API lokal masih berjalan pada port 3001.
3. Buka terminal baru.
4. Jalankan:

```
cloudflared tunnel --url http://localhost:3001
```

5\. Tunggu hingga Cloudflare memberikan URL sementara, misalnya:

```
https://random-name.trycloudflare.com
```

6\. Uji endpoint:

```
https://random-name.trycloudflare.com/api/local-data
```

7\. Jika respons JSON berhasil ditampilkan, berarti layanan lokal sudah dapat diakses melalui tunnel.

**Catatan keamanan:** Quick Tunnel ditujukan untuk pengujian, URL bersifat sementara, dan siapa pun yang mengetahui URL dapat mengakses layanan tersebut. Jangan gunakan data sensitif atau database produksi.



### **Langkah 6 – Integrasi Cloud dengan On-Premise**

Pada tahap ini mahasiswa menghubungkan aplikasi cloud dengan layanan API lokal.

1. Buka kembali proyek `cloud-integration`.
2. Tambahkan endpoint baru pada `server.js`:

```
app.get('/api/hybrid-data', async (req, res) => {
    try {
        const response = await fetch(
            `${process.env.LOCAL_API_URL}/api/local-data`
        );

        if (!response.ok) {
            throw new Error('Local API tidak merespons');
        }

        const data = await response.json();

        res.json({
            status: 'success',
            integration: 'Hybrid Cloud',
            local_data: data
        });

    } catch (error) {
        res.status(500).json({
            status: 'error',
            message: error.message
        });
    }
});
```

3\. Push perubahan ke GitHub:

```
git add .
git commit -m "Add hybrid cloud integration"
git push
```

4. Tunggu Render melakukan deployment ulang.
5. Masuk ke dashboard Render, pilih aplikasi, lalu buka **Environment**.
6. Tambahkan environment variable:

```
LOCAL_API_URL=https://random-name.trycloudflare.com
```

Gunakan URL tunnel yang sedang aktif.

7\. Buka endpoint:

```
https://cloud-integration.onrender.com/api/hybrid-data
```

8\. Perhatikan hasil respons JSON.

**Hasil yang diharapkan:** Aplikasi yang berjalan di cloud berhasil mengambil data dari API server lokal.

Target integrasi

Cloud Application → Cloudflare Tunnel → Local API → Data JSON → Cloud Application.

### **Langkah 7 – Pengujian dan Analisis**

Lakukan beberapa pengujian berikut:

| No. | Skenario Pengujian             | Hasil yang Diharapkan                             |
| --- | ------------------------------ | ------------------------------------------------- |
| 1   | Mengakses aplikasi cloud       | Aplikasi tampil                                   |
| 2   | Mengakses API cloud            | JSON berhasil ditampilkan                         |
| 3   | Mengakses API lokal            | Data lokal tampil                                 |
| 4   | Mengakses API melalui tunnel   | Data lokal dapat diakses melalui HTTPS            |
| 5   | Mengakses endpoint hybrid      | Data lokal berhasil diterima aplikasi cloud       |
| 6   | Menghentikan Cloudflare Tunnel | Integrasi hybrid gagal                            |
| 7   | Menjalankan kembali tunnel     | Integrasi dapat dipulihkan setelah URL diperbarui |

## **6. Hasil yang Diharapkan**

Setelah menyelesaikan praktikum, mahasiswa mampu:

1. Melakukan deployment aplikasi ke platform cloud.
2. Mengakses aplikasi melalui domain publik.
3. Menjalankan layanan API pada server lokal.
4. Menghubungkan server lokal dengan cloud menggunakan Cloudflare Tunnel.
5. Mengintegrasikan data dari dua lingkungan komputasi berbeda.
6. Mengidentifikasi permasalahan komunikasi dan melakukan troubleshooting.

## **7. Tugas dan Laporan Praktikum**

Mahasiswa diminta mengerjakan tugas berikut:

1. Buat aplikasi REST API sederhana menggunakan Node.js.
2. Deploy aplikasi tersebut menggunakan Render.
3. Buat API lokal yang menghasilkan minimal tiga informasi data.
4. Hubungkan API lokal dengan Cloudflare Tunnel.
5. Integrasikan API lokal dengan aplikasi cloud.
6. Buat diagram arsitektur sistem hybrid cloud yang telah dibangun.
7. Dokumentasikan setiap tahapan praktikum dengan screenshot.
8. Buat analisis hasil pengujian dan kendala yang ditemukan.

**Pertanyaan Diskusi:**

1. Apa perbedaan deployment menggunakan VPS dengan PaaS seperti Render?
2. Mengapa aplikasi cloud tidak dapat langsung mengakses `localhost` pada komputer mahasiswa?
3. Apa fungsi Cloudflare Tunnel dalam arsitektur hybrid cloud?
4. Apa yang terjadi apabila server lokal mengalami gangguan koneksi internet?
5. Mengapa database tidak disarankan untuk diekspos langsung ke internet?
6. Apa keuntungan dan keterbatasan penggunaan hybrid cloud?

## **8. Referensi**

1. Erl, T. (2013). *Cloud Computing: Concepts, Technology & Architecture*. Prentice Hall.
2. Buyya, R., Vecchiola, C., & Selvi, S. T. (2013). *Mastering Cloud Computing*. McGraw-Hill.
3. Render Documentation. *Deploy for Free*. [Dokumentasi Render](https://render.com/docs/free?utm_source=chatgpt.com).
4. Render Documentation. *Web Services*. [Dokumentasi Web Services](https://render.com/docs/web-services?utm_source=chatgpt.com).
5. Cloudflare Documentation. *Cloudflare Tunnel*. [Dokumentasi Cloudflare Tunnel](https://developers.cloudflare.com/tunnel/?utm_source=chatgpt.com).
6. Cloudflare Documentation. *Quick Tunnels*. [Dokumentasi Quick Tunnels](https://developers.cloudflare.com/tunnel/get-started/quick-tunnels/?utm_source=chatgpt.com).
7. Node.js Documentation. [Node.js API Documentation](https://nodejs.org/docs/latest/api/?utm_source=chatgpt.com).
8. GitHub Documentation. [GitHub Docs](https://docs.github.com/?utm_source=chatgpt.com).
