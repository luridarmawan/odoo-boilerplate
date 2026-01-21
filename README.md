# Odoo ERP Boilerplate

> 🌐 [Baca dalam Bahasa Indonesia](README-id.md)

Odoo is a suite of web based open source business apps.

The main Odoo Apps include an Open Source CRM, Website Builder, eCommerce, Warehouse Management, Project Management, Billing & Accounting, Point of Sale, Human Resources, Marketing, Manufacturing, ...

Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get
a full-featured <a href="https://www.odoo.com">Open Source ERP</a> when you install several Apps.

This boilerplate provides Docker configuration for running Odoo 18 with PostgreSQL already installed on the host.

## 🚀 Features
- Uses **Docker Compose** for service management.
- Database configuration via `.env` file.
- Support for additional modules with the `addons` folder.
- **PgBouncer** - Connection pooling for PostgreSQL database optimization.

## 🔌 PgBouncer

PgBouncer is a lightweight connection pooler for PostgreSQL that helps manage database connections more efficiently.

### Benefits of Using PgBouncer:
- **Reduces connection overhead** - Limits the number of direct connections to PostgreSQL
- **Improves performance** - Reuses existing connections
- **Better scalability** - Supports more concurrent users
- **Transaction Pooling Mode** - Optimal for Odoo

### PgBouncer Configuration:
Configuration files are located in the `pgbouncer/` folder:
- `pgbouncer.ini` - Main PgBouncer configuration
- `userlist.txt` - User and password list for authentication

### Environment Variables (docker-compose.yml):
| Variable | Default Value | Description |
|----------|---------------|-------------|
| `POOL_MODE` | `transaction` | Pooling mode (session/transaction/statement) |
| `MAX_CLIENT_CONN` | `200` | Maximum client connections |
| `DEFAULT_POOL_SIZE` | `50` | Default pool size per database |
| `AUTH_TYPE` | `scram-sha-256` | Authentication method |

## 📂 Folder Structure
```
📦 odoo-boilerplate
├── 📜 docker-compose.yml   # Docker Compose configuration
├── 📜 .env.example         # Environment configuration example
├── 📂 addons               # Folder for additional modules
├── 📂 config               # Folder for Odoo configuration
│   ├── 📜 odoo.conf.example  # Odoo configuration example file
│   └── 📜 odoo.conf          # Odoo configuration file (created from example)
├── 📂 data                 # Folder for Odoo data (filestore, sessions)
├── 📂 log                  # Folder for Odoo logs
└── 📂 pgbouncer            # PgBouncer configuration folder
    ├── 📜 pgbouncer.ini      # PgBouncer configuration
    └── 📜 userlist.txt       # User list for authentication
```

## 🛠 Preparation
1. **Copy the environment file**:
   ```sh
   cp .env.example .env
   ```
2. **Edit the `.env` file** according to your host database configuration.

3. **Copy the Odoo configuration file**:
   ```sh
   cp config/odoo.conf.example config/odoo.conf
   ```
4. **Edit `config/odoo.conf`** as needed (optional).

## 🚀 Running Odoo
You can start Odoo in two ways:
1. **Run with log output in terminal:**
   ```sh
   docker compose up
   ```
2. **Run in background (detached mode):**
   ```sh
   docker compose up -d
   ```
Odoo will run at **http://localhost:8069**.

## 📦 Adding Additional Modules
1. Extract the module into the `addons/` folder.
2. Restart Odoo:
   ```sh
   docker compose restart odoo
   ```
3. Login to Odoo → **Apps** → **Update Apps List** → **Search & Install Module**.

## 🛑 Stopping Odoo
```sh
docker compose down
```

## 🛠 Custom Configuration

The Odoo configuration file is located at `config/odoo.conf`.

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


| Variable | Description |
|---|---|
| `proxy_mode` | Use this option if you want to use Multi-Database Based on Domain |
| `list_db` | To show or hide the 'Manage Database' feature |
| `dbfilter` | Database list display filter<br>`.*`: show all databases<br>`^%h$`: show based on domain |

## 🔐 Permission (REQUIRED on Linux)

Odoo in the container runs as UID 101. You must set the correct permissions:

```sh
sudo chown -R 101:101 data addons log
sudo chmod -R 755 data addons log
```

> [!WARNING]
> If permissions are not set correctly, you will encounter this error:
> ```
> Permission denied: filestore
> ```

## 📦 File Upload Location

Odoo uploaded files will be stored on the host at:

```
./data/.local/share/Odoo/filestore/<database_name>/
```

## 🧪 Validate Bind Mount (REQUIRED CHECK)

Verify that the bind mount is working correctly:

```sh
docker exec -it odoo-app bash
touch /var/lib/odoo/BIND_OK
exit
ls data/BIND_OK
```

If the file `data/BIND_OK` exists, the bind mount is working correctly.

## 🧰 Backup (Recommended)

**Data Backup:**
```sh
tar czvf odoo-data-$(date +%F).tar.gz data/
```

**Database Backup:**
Backup separately from the PostgreSQL server.
