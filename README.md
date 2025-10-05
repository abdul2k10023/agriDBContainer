# AgriDBContainer

A lightweight Docker-based setup providing the **database backend** for the Agri IoT project.  
It runs **PostgreSQL**, **PostgREST**, and **Grafana** for a quick REST API and dashboards.

---

## 🧩 Purpose
- Host the main PostgreSQL database (`mydb`) for sensor/motor/system logs.
- Expose a REST API via PostgREST (**port 3000**) for safe DB access.
- Provide Grafana dashboards (**port 3001**) connected to PostgreSQL.

---

## 🚀 Usage

### Start / Stop
```bash
sudo docker-compose up -d --build
sudo docker-compose down
```

### Logs / Containers
```bash
sudo docker ps
sudo docker logs -f <container-name>
```

---

## 🗂️ Services & Ports

| Service     | Port Map | Purpose                                | Data Volume            |
|-------------|----------|----------------------------------------|------------------------|
| PostgreSQL  | 5432:5432| Primary DB (`mydb`, user `agri`)       | `pgdata`               |
| PostgREST   | 3000:3000| REST API for PostgreSQL                 | —                      |
| Grafana     | 3001:3000| Dashboards & visualization              | `grafana_data`         |

Network: all services run on the `backend` Docker network.

---

## 🐘 PostgreSQL
Default credentials from `docker-compose.yml`:
- **User:** `agri`
- **Password:** `1234`
- **DB:** `mydb`

Connect:
```bash
sudo docker exec -it postgres_agri psql -U agri -d mydb
```

---

## 🔁 PostgREST
Runs at:
```
http://localhost:3000
```
`postgrest.conf` points to:
```
postgres://agri:1234@postgres:5432/mydb
```

---

## 📊 Grafana
Runs at:
```
http://localhost:3001
```
- **Login:** `admin` / `admin` (from `GF_SECURITY_ADMIN_PASSWORD=admin`)
- Add **PostgreSQL** data source:
  - Host: `postgres:5432`
  - Database: `mydb`
  - User: `agri`
  - Password: `1234`
  - SSL: off (default)
- Create a new dashboard → panel → run SQL from your schema.

Data persists in the `grafana_data` volume.

---

**Author:** Abdul Haseeb • **Project:** Agri IoT Database Backend
