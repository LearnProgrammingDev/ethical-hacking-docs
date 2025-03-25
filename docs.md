````markdown
# Dokumentasi Dasar TCP dan Nmap

### Pengantar TCP (Transmission Control Protocol)

TCP adalah protokol komunikasi utama yang menjadi **penghubung lalu lintas data di internet**. Contohnya, ketika kamu ingin mencari informasi di Google, perangkatmu harus terhubung ke internet. **TCP bertugas membangun koneksi dan memastikan data yang kamu minta dari server Google (atau server lainnya) sampai dengan benar dan utuh ke perangkatmu, begitu juga sebaliknya.**

### Pengenalan Nmap

**Nmap (Network Mapper) adalah sebuah tools yang digunakan untuk melakukan pemindaian (scanning) jaringan.** Fungsinya antara lain:

* Menganalisis dan memantau jaringan.
* Mendeteksi host (perangkat) yang aktif dalam jaringan.
* Mengidentifikasi layanan (services) yang berjalan pada host tersebut.
* Mendapatkan informasi tentang sistem operasi yang digunakan oleh host (OS Detection).
* Menganalisis port-port yang terbuka pada host.

### Cara Mengetahui Host yang Aktif di Jaringan Lokal

Ada dua cara yang bisa digunakan untuk mengetahui host yang aktif di jaringan lokal komputermu:

**1. Menggunakan `netdiscover` (Membutuhkan hak akses `sudo`)**

   Command:
   ```bash
   sudo netdiscover -r 192.168.100.0/24
   ```
   Keterangan:
   * `sudo`: Digunakan untuk menjalankan perintah dengan hak akses administrator.
   * `netdiscover`: Nama tools untuk menemukan host di jaringan.
   * `-r 192.168.100.0/24`: Menentukan rentang alamat IP yang akan dipindai. `192.168.100.0/24` artinya kita memindai semua alamat IP dari 192.168.100.1 hingga 192.168.100.254. `/24` adalah notasi CIDR yang menunjukkan jumlah bit yang digunakan untuk network ID. Kamu bisa menggantinya sesuai dengan konfigurasi jaringan lokalmu. Jika tidak spesifik, `netdiscover` akan mencoba memindai seluruh jaringan yang terdeteksi.

**2. Menggunakan `nmap` (Lebih disarankan untuk hasil yang lebih baik)**

   Command:
   ```bash
   nmap -sn 192.168.100.0/24
   ```
   Keterangan:
   * `nmap`: Nama tools untuk pemindaian jaringan.
   * `-sn`: Opsi ini digunakan untuk melakukan "ping scan" atau "host discovery". Nmap hanya akan mencoba menentukan host mana yang aktif (merespons ping) tanpa melakukan pemindaian port lebih lanjut.
   * `192.168.100.0/24`: Sama seperti di `netdiscover`, ini adalah rentang alamat IP yang akan dipindai.

### Opsi-Opsi Dasar Nmap

* `-sn`: **Scan host yang aktif** di jaringan lokal (LAN) komputermu. Ini adalah cara cepat untuk melihat perangkat mana saja yang sedang online.
* `-Pn`: Digunakan untuk **memaksa Nmap untuk menganggap semua host target aktif**, meskipun mereka tidak merespons ping. Opsi ini berguna jika ada firewall yang memblokir ping.
* `0/24`: Ini adalah contoh **rentang alamat IP (IP Range)** dalam notasi CIDR. Bisa juga ditulis dengan format `0-255` pada bagian terakhir alamat IP (misalnya `192.168.100.0-255`) atau menggunakan tanda bintang `*` (misalnya `192.168.100.*`).

### Metode Pemindaian (Scanning) Nmap

1.  **SYN Scan (`-sS`) - Stealth Scan (Membutuhkan hak akses `sudo`)**
    * Command: `sudo nmap -sS ip_target`
    * Membutuhkan izin administrator (root atau menggunakan `sudo`).
    * Merupakan salah satu metode pemindaian yang **paling umum dan direkomendasikan**.
    * **Kelebihan:** Relatif cepat dan **tidak meninggalkan jejak yang banyak** pada sistem target karena tidak menyelesaikan three-way handshake TCP secara penuh. Server target akan menerima paket SYN, tetapi Nmap tidak akan mengirimkan ACK untuk menyelesaikan koneksi.
    * **Tujuan:** Untuk menentukan port mana yang terbuka, tertutup, atau terfilter.

2.  **TCP Connect Scan (`-sT`)**
    * Command: `nmap -sT ip_target`
    * **Tidak memerlukan hak akses administrator.**
    * **Kelebihan:** Lebih cepat daripada UDP scan.
    * **Kekurangan:** **Meninggalkan jejak yang lebih banyak** pada sistem target karena menyelesaikan three-way handshake TCP secara penuh. Ini akan tercatat dalam log sistem target.

3.  **UDP Scan (`-sU`)**
    * Command: `sudo nmap -sU ip_target`
    * Membutuhkan hak akses administrator (`sudo`).
    * Digunakan untuk **memindai port UDP**.
    * **Kekurangan:** **Lebih lambat** dibandingkan SYN scan dan TCP Connect scan karena sifat protokol UDP yang tidak memiliki mekanisme handshake seperti TCP.

### Pemindaian Deteksi Sistem Operasi (OS Detection)

Nmap memiliki kemampuan yang sangat baik dalam mendeteksi sistem operasi yang digunakan oleh target karena memiliki database yang besar tentang ciri-ciri berbagai versi OS.

Command:
```bash
sudo nmap -O ip_target
```
Keterangan:
* `-O`: Opsi untuk mengaktifkan OS detection.
* `ip_target`: Alamat IP target yang ingin dipindai.

Output dari perintah ini akan menampilkan port yang terbuka dan perkiraan informasi sistem operasi yang digunakan oleh target.

### Pemindaian Versi Layanan (Service Version Scanning)

Fitur ini memungkinkan Nmap tidak hanya menampilkan port yang terbuka, tetapi juga **versi detail dari layanan (aplikasi) yang berjalan di port tersebut**. Informasi ini sangat berguna, contohnya untuk mengidentifikasi potensi kerentanan keamanan yang mungkin ada pada versi layanan tertentu, yang kemudian bisa dimanfaatkan oleh tools seperti Metasploit.

Command:
```bash
sudo nmap -sV ip_target
```
Keterangan:
* `-sV`: Opsi untuk mengaktifkan service version scanning.
* `ip_target`: Alamat IP target.

**Konfigurasi Intensitas Kecepatan Pengecekan Versi:**

Kamu bisa mengatur seberapa intens Nmap mencoba mengidentifikasi versi layanan menggunakan opsi `--version-intensity` dengan nilai antara 1 hingga 9.

Command:
```bash
sudo nmap -sV --version-intensity <nilai> ip_target
```
Contoh: `sudo nmap -sV --version-intensity 2 ip_target`

Selain itu, ada opsi lain:

* `--version-all`: Melakukan pengecekan versi secara menyeluruh (intensitas tertinggi).
* `--version-light`: Melakukan pengecekan versi dengan cepat namun mungkin tidak mendapatkan semua informasi.

### Opsi Agresif (`-A`)

Opsi `-A` adalah opsi yang **agresif** karena melakukan beberapa pemindaian sekaligus, termasuk:

* Pemindaian versi port terbuka (`-sV`)
* Deteksi sistem operasi (`-O`)
* Trace route
* Beberapa script NSE dasar

Command:
```bash
sudo nmap -A ip_target
```
**Kelebihan:** Menyediakan informasi yang sangat lengkap tentang target.
**Kekurangan:** **Meninggalkan jejak yang lebih banyak** pada sistem target, sehingga lebih mudah terdeteksi.

### Spesifikasi Port dan Penanganan Output

**Memilih Port Tertentu untuk Dipindai:**

Kamu bisa menentukan port mana saja yang ingin dipindai menggunakan opsi `-p`.

Command:
```bash
sudo nmap -sS -p <daftar_port> ip_target
```
Keterangan:
* `-sS`: Melakukan SYN scan (membutuhkan `sudo`).
* `-p`: Opsi untuk menentukan port.
* `<daftar_port>`: Port yang ingin dipindai. Bisa berupa:
    * Port tunggal: `-p 21` (hanya memindai port 21)
    * Beberapa port dipisahkan koma: `-p 21,35,80,100`
    * Rentang port: `-p 21-200`
    * Semua port: `-p 1-65535`

**Memindai 100 Port yang Umum Digunakan (Fast Scan):**

Command:
```bash
sudo nmap -sS -p -F ip_target
```
Keterangan:
* `-F`: Opsi untuk melakukan "fast scan", yaitu memindai daftar port yang dianggap paling umum digunakan.

**Menyimpan Hasil Pemindaian ke File:**

Kamu bisa menyimpan hasil pemindaian Nmap ke dalam file untuk referensi nanti. Ada beberapa format output yang bisa digunakan:

* Menyimpan ke format normal (mudah dibaca manusia):
    ```bash
    sudo nmap -sS -p -F ip_target >> hasilscan.txt
    ```
    Tanda `>>` akan menambahkan output ke akhir file `hasilscan.txt`. Jika kamu ingin menimpa file, gunakan tanda `>`.
* Menyimpan ke format normal (opsi `-oN`):
    ```bash
    sudo nmap -sS -oN hasilscan2 ip_target
    ```
    Opsi `-oN` akan menyimpan output dalam format normal ke file `hasilscan2`.
````