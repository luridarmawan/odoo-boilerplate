# Odoo ERP Boilerplate

> 🌐 [Read in English](README.md)

Odoo is a suite of web based open source business apps.

The main Odoo Apps include an Open Source CRM, Website Builder, eCommerce, Warehouse Management, Project Management, Billing & Accounting, Point of Sale, Human Resources, Marketing, Manufacturing, ...

Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get
a full-featured <a href="https://www.odoo.com">Open Source ERP</a> when you install several Apps.

Boilerplate ini menyediakan konfigurasi Docker untuk menjalankan Odoo 18 dengan PostgreSQL yang sudah terpasang di host.

## 🚀 Fitur
- Menggunakan **Docker Compose** untuk manajemen layanan.
- Konfigurasi database melalui file `.env`.
- Dukungan untuk modul tambahan dengan folder `addons`.
- **PgBouncer** - Connection pooling untuk optimasi koneksi database PostgreSQL.

## 🔌 PgBouncer

PgBouncer adalah connection pooler ringan untuk PostgreSQL yang membantu mengelola koneksi database dengan lebih efisien.

### Keuntungan Menggunakan PgBouncer:
- **Mengurangi beban koneksi** - Membatasi jumlah koneksi langsung ke PostgreSQL
- **Meningkatkan performa** - Reuse koneksi yang sudah ada
- **Skalabilitas lebih baik** - Mendukung lebih banyak pengguna bersamaan
- **Mode Transaction Pooling** - Optimal untuk Odoo

### Konfigurasi PgBouncer:
File konfigurasi berada di folder `pgbouncer/`:
- `pgbouncer.ini` - Konfigurasi utama PgBouncer
- `userlist.txt` - Daftar user dan password untuk autentikasi

### Environment Variables (docker-compose.yml):
| Variabel | Nilai Default | Deskripsi |
|----------|---------------|-----------|
| `POOL_MODE` | `transaction` | Mode pooling (session/transaction/statement) |
| `MAX_CLIENT_CONN` | `200` | Maksimum koneksi client |
| `DEFAULT_POOL_SIZE` | `50` | Ukuran pool default per database |
| `AUTH_TYPE` | `scram-sha-256` | Metode autentikasi |

## 📂 Struktur Folder
```
📦 odoo-boilerplate
├── 📜 docker-compose.yml   # Konfigurasi Docker Compose
├── 📜 .env.example         # Contoh konfigurasi environment
├── 📂 addons               # Folder untuk modul tambahan
├── 📂 config               # Folder untuk konfigurasi Odoo
│   ├── 📜 odoo.conf.example  # Contoh file konfigurasi Odoo
│   └── 📜 odoo.conf          # File konfigurasi Odoo (dibuat dari example)
├── 📂 data                 # Folder untuk data Odoo (filestore, sessions)
├── 📂 log                  # Folder untuk log Odoo
└── 📂 pgbouncer            # Folder konfigurasi PgBouncer
    ├── 📜 pgbouncer.ini      # Konfigurasi PgBouncer
    └── 📜 userlist.txt       # User list untuk autentikasi
```

## 🛠 Persiapan
1. **Salin file environment**:
   ```sh
   cp .env.example .env
   ```
2. **Edit file `.env`** sesuai dengan konfigurasi database yang ada di host.

3. **Salin file konfigurasi Odoo**:
   ```sh
   cp config/odoo.conf.example config/odoo.conf
   ```
4. **Edit file `config/odoo.conf`** sesuai kebutuhan (opsional).

## 🚀 Menjalankan Odoo
Anda dapat memulai Odoo dengan dua cara:
1. **Menjalankan dengan output log di terminal:**
   ```sh
   docker compose up
   ```
2. **Menjalankan di background (detached mode):**
   ```sh
   docker compose up -d
   ```
Odoo akan berjalan di **http://localhost:8069**.

## 📦 Menambahkan Modul Tambahan
1. Ekstrak modul ke dalam folder `addons/`.
2. Restart Odoo:
   ```sh
   docker compose restart odoo
   ```
3. Login ke Odoo → **Apps** → **Perbarui Daftar Aplikasi** → **Cari & Install Modul**.

## 🛑 Menghentikan Odoo
```sh
docker compose down
```

## 🛠 Custom Konfigurasi

File konfigurasi odoo berada di `config/odoo.conf`.

```bash
[options]
.
.
.
proxy_mode = True
#dbfilter = ^%h$
DBFILTER=.*
list_db = False
```


| Variabel | Deskripsi |
|---|---|
| `proxy_mode` | Gunakan pilihan ini jika akan menggunakan Multi-Database Berdasarkan Domain |
| `list_db` | Untuk menampilkan atau menyembunyikan fitur 'Manage Database' |
| `dbfilter` | Filter tampilan daftar database<br>`.*`: menampilkan semua database<br>`^%h$`: menampilkan berdasarkan domain |
