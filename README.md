# Odoo ERP Boilerplate

Odoo is a suite of web based open source business apps.

The main Odoo Apps include an Open Source CRM, Website Builder, eCommerce, Warehouse Management, Project Management, Billing & Accounting, Point of Sale, Human Resources, Marketing, Manufacturing, ...

Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get
a full-featured <a href="https://www.odoo.com">Open Source ERP</a> when you install several Apps.

Boilerplate ini menyediakan konfigurasi Docker untuk menjalankan Odoo 18 dengan PostgreSQL yang sudah terpasang di host.

## 🚀 Fitur
- Menggunakan **Docker Compose** untuk manajemen layanan.
- Konfigurasi database melalui file `.env`.
- Dukungan untuk modul tambahan dengan folder `addons`.

## 📂 Struktur Folder
```
📦 odoo-boilerplate
├── 📜 docker-compose.yml  # Konfigurasi Docker Compose
├── 📜 .env.example        # Contoh konfigurasi environment
├── 📂 addons             # Folder untuk modul tambahan
└── 📂 config             # Folder untuk konfigurasi Odoo
```

## 🛠 Persiapan
1. **Salin file environment**:
   ```sh
   cp .env.example .env
   ```
2. **Edit file `.env`** sesuai dengan konfigurasi database yang ada di host.

## 🚀 Menjalankan Odoo
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


