### Tutorial Instalasi OpenLiteSpeed (OLS)
**A. Persiapan Server Debian**

Sebelum mulai memasang OpenLiteSpeed, pastikan kamu sudah menyiapkan:
- Server Debian yang sudah memiliki alamat IP dan terhubung ke jaringan LAN.
- Repositori Debian sudah bisa digunakan untuk instalasi paket.
- Coba akses server melalui SSH di CMD dan WinSCP, untuk memastikan konek ke server aman dan lancar.
___
**B. Instalasi OpenLiteSpeed (OLS)**

Karena OLS memakai repo tambahan, pastikan server bisa internet. Setelah itu jalankan:

1. Upgrade dan Update Sistem
  >- apt update
  >- apt upgrade


2. Install utilitas penting
   apt install wget curl 
3. Tambahkan repository OLS
wget -O - https://repo.litespeed.sh | bash 
4. Install OpenLiteSpeed
apt install openlitespeed 
5. Install PHP 8.4 + MySQL extension
apt install lsphp84 lsphp84-mysql 
6. Jalankan dan aktifkan service OLS
systemctl start lsws systemctl enable lsws 
C. Membuat Password Panel Admin
Untuk masuk ke panel konfigurasi OLS, kamu harus membuat username dan password:
• Jalankan script:
/usr/local/lsws/admin/misc/admpass.sh 
• Masukkan username (contoh: admin)
• Buat password
• Buka panel admin di browser:
http://ip-server:7080 
D. Tes Halaman Default OLS
Buka browser dan ketik:
http://ip-server:8088 
Kalau tampil halaman default, berarti instalasi berhasil.
E. Mengatur Versi PHP di OLS
Untuk membuat OLS memakai PHP 8.4, lakukan pengaturan berikut:
1. Konfigurasi External App
Menu: Server Configuration → External App → Add → LiteSpeed SAPI App
Isi seperti berikut:
• Name: lsphp84
• Address: uds://tmp/lshttpd/lsphp.sock
• Notes: PHP 8.4
• Max Connections: 35
• Initial Request Timeout: 60
• Persistent Connection: Yes
• Command: /usr/local/lsws/lsphp84/bin/lsphp
• Instances: 1
Simpan → klik Graceful Restart
2. Atur Script Handler
Menu: Server Configuration → Script Handler
• Suffix: php
• Handler Type: LiteSpeed SAPI
• Handler Name: lsphp84
Simpan → Graceful Restart
F. Mengganti Port 8088 → 80
Agar websitenya berjalan di port standar (80):
• Masuk panel admin
• Menu: Listeners → Default → Edit
• Ubah port dari 8088 menjadi 80
• Simpan dan lakukan Graceful Restart
Sekarang coba akses:
http://ip-server 
G. Supaya index.php Dibaca Otomatis
OLS biasanya membaca index.html dulu, jadi kita ubah:
• Login ke panel admin
• Menu: Virtual Hosts → Example → General
• Cari bagian Index Files
• Ubah menjadi:
index.php, index.html 
• Simpan
H. Membuat SSL Self-Signed
Agar website bisa dipakai via HTTPS, kita buat sertifikat SSL sederhana.
1. Buat sertifikat
mkdir /etc/ssl/private cd /etc/ssl/private openssl req -x509 -newkey rsa:2048 -nodes -keyout self.key -out self.crt -days 365 
Hasilnya ada dua file:
• self.key
• self.crt
2. Pasang SSL di OLS
Masuk panel admin → Listeners → Add
Isi:
• Listener Name: SSL
• IP Address: ANY
• Port: 443
• Secure: Yes
Tambahkan Virtual Host:
• Virtual Host: Example
• Domains: *
Pada tab SSL, isi:
• Private Key File → /etc/ssl/private/self.key
• Certificate File → /etc/ssl/private/self.crt
Simpan → Graceful Restart
I. Pengujian Akhir
Server sekarang bisa diakses melalui:
• HTTP → http://ip-server
• HTTPS → https://ip-server
Kalau mau mengubah isi halaman default, edit file yang ada di:
/usr/local/lsws/Example/ 
C. Mengakses Server via SSH & WinSCP
1. SSH menggunakan CMD
ssh kahiang@ipserver 
Karena login root tidak diizinkan, gunakan sudo:
• Install: sudo apt install php
• Edit file: sudo nano /var/www/html/index.php
2. Akses pakai WinSCP
Masukkan:
• Host Name: IP server
• User: kahiang
• Password: (tanya KM)
Masuk menu:
Advanced → Environment → SFTP
Ubah menjadi:
sudo /usr/lib/openssh/sftp-server 
Login seperti biasa.