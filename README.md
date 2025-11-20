# OpenLiteSpeed
## Kelebihan dan Kekurangan OLS

---

## **⚙️ Pengertian Singkat**

OpenLiteSpeed (OLS) adalah versi open source dari LiteSpeed Web Server (LSWS), sebuah web server modern yang dirancang agar super cepat, ringan, dan mudah diatur.
Dia sering dibandingkan dengan Apache dan Nginx, karena fungsi utamanya sama: menyajikan konten web ke pengguna.


---

## 💪 Kelebihan OpenLiteSpeed

### **1. 🚀 Performa Tinggi**

Didesain dengan arsitektur event-driven asinkron, seperti Nginx, jadi lebih efisien menangani ribuan koneksi bersamaan tanpa membebani CPU dan RAM.

Bisa memproses PHP jauh lebih cepat karena menggunakan LiteSpeed SAPI (LSAPI), bukan CGI/FPM seperti Apache atau Nginx.



---

### **2. 🧠 Caching yang Kuat**

Memiliki LiteSpeed Cache (LSCache) bawaan sangat cepat dan bisa mempercepat situs WordPress, Joomla, atau Laravel secara drastis.

Tidak perlu plugin caching tambahan (beda dengan Apache atau Nginx).



---

### **3. 🔒 Keamanan Tinggi**

Built-in Anti-DDoS, Brute Force Protection, dan reCAPTCHA Defense.

Mendukung mod_security rules (seperti Apache), jadi bisa pakai aturan keamanan yang sama.



---

### **4. ⚡ Kompatibilitas Apache**

Mendukung sebagian besar aturan .htaccess, mod_rewrite, dan mod_security.

Jadi, migrasi dari Apache ke OpenLiteSpeed cukup mudah.



---

### **5. 🎛️ Web GUI (Panel Admin)**

Ada antarmuka web (WebAdmin Console) yang memudahkan konfigurasi virtual host, SSL, PHP, log, dan lainnya tanpa edit file manual.

Cocok buat pengguna yang belum terlalu nyaman dengan command line.



---

### **6. 🧩 Open Source & Gratis**

100% gratis dan open-source (tidak seperti versi komersial LiteSpeed Enterprise).

Cocok untuk proyek pribadi, belajar, dan server kecil–menengah.



---

## ⚠️ Kekurangan OpenLiteSpeed

### **1. 🔁 Konfigurasi .htaccess Tidak Dinamis**

 Berbeda dari Apache, OpenLiteSpeed tidak membaca .htaccess secara real-time.

Artinya, setiap kali file .htaccess diubah, kamu harus reload server agar perubahan berlaku.



---

### **2. 🧩 Fitur Enterprise Tidak Tersedia**

  - Beberapa fitur hanya ada di LiteSpeed Enterprise, seperti:

  - HTTP/3 early access (kadang masih eksperimental di OLS)

  - LSCache advanced (E-commerce, WooCommerce full cache)

  - Dukungan teknis resmi.




---

### **3. ⚙️ Sedikit Lebih Rumit untuk Multi-Domain**

Untuk hosting banyak domain atau akun (seperti cPanel/WHM), OLS kurang fleksibel dibanding versi Enterprise.

Harus buat virtual host manual satu per satu.



---

### **4. 🧑‍💻 Komunitas Lebih Kecil dari Nginx/Apache**

Dokumentasi resmi bagus, tapi kadang contoh konfigurasi dari komunitas masih terbatas.

Artinya, troubleshooting bisa lebih lama kalau error jarang ditemui.



---

### **5. 🔄 Integrasi Panel Hosting Masih Terbatas**

Beberapa panel populer seperti cPanel atau Plesk tidak mendukung OLS, hanya mendukung versi Enterprise.

Tapi panel seperti CyberPanel sudah mendukung OLS penuh dan gratis.



---

## **🔍 Kesimpulan**

**Aspek	OpenLiteSpeed	Penjelasan**

  - ⚡ Performa	Sangat cepat	Cocok untuk traffic tinggi
  - 🔒 Keamanan	Kuat	Ada proteksi bawaan
  - 🧰 Kemudahan konfigurasi	Mudah lewat GUI	Tapi .htaccess harus reload
  - 💰 Lisensi	Gratis & Open Source	Tidak semua fitur Enterprise
  - 🌐 Kompatibilitas	Baik dengan Apache	Tapi tidak sepenuhnya
  - 👥 Komunitas	Sedang berkembang	Tidak sebesar Nginx/Apache.

___
# Tutorial Instalasi OpenLiteSpeed (OLS)
## **A. Persiapan Server Debian**

Sebelum mulai memasang OpenLiteSpeed, pastikan kamu sudah menyiapkan:
- Server Debian yang sudah memiliki alamat IP dan terhubung ke jaringan LAN.
- Repositori Debian sudah bisa digunakan untuk instalasi paket.
- Coba akses server melalui SSH di CMD dan WinSCP, untuk memastikan konek ke server aman dan lancar.
___
## **B. Instalasi OpenLiteSpeed (OLS)**

Karena OLS memakai repo tambahan, pastikan server bisa internet. Setelah itu jalankan:

**1. Upgrade dan Update Sistem**
  >- apt update
  >- apt upgrade

**2. Install utilitas penting**
  >- apt install wget curl 

**3. Tambahkan repository OLS**
  >- wget -O - https://repo.litespeed.sh | bash
 
**4. Install OpenLiteSpeed**
  >- apt install openlitespeed 

**5. Install PHP 8.4 + MySQL extension**
  >- apt install lsphp84 lsphp84-mysql
 
**6. Jalankan dan aktifkan service OLS**
  >- systemctl start lsws 
  >- systemctl enable lsws 
___

## **C. Membuat Password Panel Admin**

Untuk masuk ke panel konfigurasi OLS, kamu harus membuat username dan password:

**1. Jalankan script:**
>- /usr/local/lsws/admin/misc/admpass.sh 

**2. Masukkan username (contoh: admin)**
- Buat password
- Buka panel admin di browser: >-http://ip-server:7080

___
## **D. Tes Halaman Default OLS**
Buka browser dan ketik:
>- http://ip-server:8088 

Kalau tampil halaman default, berarti instalasi berhasil.

___
## **E. Mengatur Versi PHP di OLS**
Untuk membuat OLS memakai PHP 8.4 Pastikah lsphp84 sudah terinstal, untuk merubahnya ikuti tahapan beriku:

### **Konfigurasi External App**

1. Di menu kiri pilih: **Server Configuration → External App**

2. **Klik Add → pilih LiteSpeed SAPI App → Next**

3. Isi:
  - Name: **lsphp84**
  - Address: 
   >-uds://tmp/lshttpd/lsphp.sock
  - Notes: **PHP 8.4**
  - Max Connections: **35**
  - Initial Request Timeout: **60**
  - Retry Timeout: **0**
  - Persistent Connection: **Yes**
  - Command:
   >-/usr/local/lsws/lsphp84/bin/lsphp
  - Instances: **1 (default)**

  **Save -> _Graceful Restart_**


### **Atur Script Handler**
1. Masih di menu kiri: **Server Configuration → Script Handler**

2. Edit handler lsphp atau buat baru:
   - Suffixes: **php**
   - Handler Type: **LiteSpeed SAPI**
   - Handler Name: **lsphp84**

**Save → _Graceful Restart_**

___
## **F. Ubah Port 8088 → 80 🔄**

Secara default, OLS jalan di port 8088, sedangkan website pada umumnya pakai port 80 atau 443. Tenang aja, kita bisa ubah pengaturannya biar sama seperti web sungguhan! 🚀

1. **Login ke panel admin** (http://ip-server:7080)
2. **Masuk ke Menu → Listeners → Default → Edit**
3. Ganti Port: **80**
4. **Klik Save → Graceful Restart**
5. Sekarang akses website di browser: 👉            
>-http://ip-server

___
## **G. Supaya index.php terbaca otomatis di OLS 📄**

Biasanya, kita memakai file index.php sebagai halaman utama website. Tapi di OLS, secara default yang bisa dibaca hanya index.html. Supaya website kita bisa menjalankan file index.php, kita perlu menambahkan sedikit pengaturan tambahan.

1. Login ke admin panel: → >-http://ip-server:7080
2. **Menu kiri → Virtual Hosts → Example → General**
3. Cari bagian Index Files
4. Biasanya isinya: **index.html**
5. Ubah jadi: **index.php, index.html**
6. Artinya OLS akan mencari index.php dulu,  kalau tidak ada baru index.html
7. Klik Save

___
## **H. Membuat Self-Signed 
SSL 🔐**

Supaya website kita bisa diakses lewat https, kita perlu menambahkan sertifikat SSL. Kali ini, kita akan membuat SSL self-signed, yaitu sertifikat buatan sendiri yang bisa digunakan untuk belajar atau pengujian di server lokal.

### **Buat Sertifikat Self-Signed**
1. Mmasuk sebagai root, lalu ketik:

>-mkdir /etc/ssl/private

>-cd /etc/ssl/private

>-openssl req -x509 -newkey rsa:2048 -nodes 
-keyout self.key -out self.crt -days 365

2. Saat diminta mengisi data (Country, State, dst), boleh diisi asal atau tekan Enter saja.

3. Jika proses berjalan normal, kita akan memiliki dua buah file yaitu self.key dan self.crt yang tersimpan di /etc/ssl/private

### **Menambahkan SSL/TSL di OLS**

1. Login ke admin panel: → **http://ip-server:7080**
2. Masuk menu: **Listeners → Add → +**
3. Isi dengan:
   - Listener Name: **SSL**
   - IP Address: **ANY**
   - Port: **443**
   - Secure: **Yes**
4. Pada bagian Virtual Host Mappings, Tambahkan:
   - Virtual Host: **Example (atau nama virtual host kamu)**
   - Domains: * **(artinya semua domain/IP)
Save**
5. Pada tab SSL → isi:
   - Private Key File: /etc/ssl/private/self.key
   - Certificate File: /etc/ssl/private/self.crt
6. **Save → Graceful Restart**

___
## **I. Pengujian OLS 🚀**
Jika semua langkah sudah berjalan dengan lancar, server kalian seharusnya sudah bisa diakses melalui _http://ip-server_ atau _https://ip-server_. Untuk mengubah tampilan halamannya, kalian bisa mengelola isi file yang ada di direktori **_/usr/local/lsws/Example/_**.
___

## **Cara Memindahkan Projek html Kedalam Server**

### **A. Cara Mengakses Server Menggunakan CMD (SSH)**

- Gunakan perintah berikut untuk mengakses server:
**ssh kahiang@<ipserver>**
 
**Catatan**: Login sebagai root tidak diizinkan. Gunakan perintah sudo untuk setiap tindakan yang memerlukan hak akses root, misalnya:
Install PHP: **sudo apt install php**
Edit file: **sudo nano /var/www/html/index.php**

### **Menggunakan WinSCP**
Karena login sebagai root tidak diizinkan, akun kahiang perlu dikonfigurasi untuk menjalankan tugas root melalui WinSCP. Ikuti langkah berikut:

1. Buka WinSCP.
2. Masukkan:
 - Host Name: Nama server (lihat tabel di atas).
3. User: kahiang
4. Password: Tanya ke KM.
5. Klik Advanced → Environment → SFTP.
Pada bagian SFTP Server, masukkan:
**sudo /usr/lib/openssh/sftp-server**



