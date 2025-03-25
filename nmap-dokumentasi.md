# Panduan Nmap dan Scanning Jaringan

## Pendahuluan

### TCP (Transmission Control Protocol)
TCP adalah protokol komunikasi yang bertanggung jawab menghubungkan lalu lintas internet. Protokol ini memungkinkan transfer data antara komputer dan server, misalnya saat melakukan pencarian di Google atau mengakses situs web.

## Pengenalan Nmap

Nmap (Network Mapper) adalah alat canggih untuk scanning dan menganalisis jaringan komputer. Alat ini membantu administrator jaringan dan profesional keamanan informasi untuk:
- Menemukan host aktif
- Mengidentifikasi port terbuka
- Mendeteksi versi sistem operasi dan layanan

## Scanning Host di Jaringan Lokal

### Metode Mendeteksi Host Aktif

1. Menggunakan Netdiscover
```bash
sudo netdiscover -r 192.168.100.0/24
```

2. Menggunakan Nmap
```bash
nmap -sn 192.168.100.0/24
```

**Catatan:** 
- `0/24` menunjukkan rentang IP yang akan di-scan
- Secara default, akan men-scan semua host dalam rentang tersebut

## Opsi Scanning Nmap

### Jenis Scan IP dan Port

1. `-sn`: Scan host aktif di LAN
2. `-Pn`: Scan spesifik untuk host yang mungkin diblokir
3. IP Range Scanning:
   - Menggunakan `0/24` atau `0-255`
   - Scan menggunakan file daftar IP (`nmap -iL listipaddress`)

## Metode Scanning Nmap

### 1. SYN Scan
- Command: `sudo nmap -sS ip_target`
- Membutuhkan izin admin
- Jejak scanning minimal
- Hasil akurat

### 2. TCP Connection Scan
- Command: `nmap -sT ip_target`
- Cepat
- Kekurangan: meninggalkan banyak jejak

### 3. UDP Scan
- Command: `sudo nmap -sU ip_target`
- Lebih lambat
- Berguna untuk scanning protokol UDP

## Fitur Lanjutan Nmap

### Deteksi Sistem Operasi
```bash
sudo nmap -O ip_target
```
Menampilkan informasi port dan sistem operasi target

### Scanning Versi Layanan
```bash
sudo nmap -sV ip_target
```
Mendeteksi versi spesifik layanan yang berjalan

### Intensitas Scanning Versi
- `--version-intensity 2-9`: Kustomisasi kedalaman scanning
- `--version-all`: Scanning intensitas tertinggi
- `--version-light`: Scanning cepat, informasi terbatas

### Scan Agresif
```bash
sudo nmap -A ip_target
```
- Mengumpulkan informasi komprehensif
- Kekurangan: meninggalkan banyak jejak

## Spesifikasi Port dan Opsi Tambahan

### Scanning Port Spesifik
```bash
sudo nmap -sS -p 21 ip_target
sudo nmap -sS -p 21,35,80,100 ip_target
sudo nmap -sS -p 21-200 ip_target
sudo nmap -sS -p 1-65535 ip_target
```

### Scanning 100 Port Teratas
```bash
sudo nmap -sS -p -F ip_target
```

### Menyimpan Hasil Scanning
```bash
sudo nmap -sS -p -F ip_target >> hasilscan.txt
sudo nmap -sS -oN hasilscan2
```

## Peringatan

- Selalu dapatkan izin terlebih dahulu sebelum melakukan scanning jaringan
- Scanning tanpa izin dapat dianggap sebagai tindakan ilegal
- Gunakan nmap secara etis dan profesional

## Kesimpulan

Nmap adalah alat powerful untuk analisis jaringan dengan berbagai opsi scanning yang fleksibel. Pemahaman mendalam tentang opsi dan metodenya sangat penting untuk keamanan dan administrasi jaringan.
