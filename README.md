# project-metrics-B10

# Soal 1: Team Dai-Gurren Spiral Power Engine Monitoring

## Struktur Folder

```
project-metrics-[Kelompok]/
├── prometheus/
│   └── prometheus.yml
└── docker-compose.yml
```

## Step-by-Step Pengerjaan

### Step 1 — Buat Struktur Folder
```bash
mkdir -p project-metrics-B10/prometheus
cd project-metrics-B10
touch prometheus/prometheus.yml
touch docker-compose.yml
```

---

### Step 2 — Isi File `prometheus/prometheus.yml`

### Step 3 — Isi File `docker-compose.yml`

### Step 4 — Jalankan Docker Compose
```bash
docker compose up -d
```
Cek semua container berjalan:
```bash
docker compose ps
```
Semua service harus berstatus **Up**.

---

### Step 5 — Cek Target Prometheus
1. Buka browser → `http://localhost:9090`
2. Klik menu **Status → Targets**
3. Pastikan target `gurren-lagann-core` berstatus **UP** 

---

### Step 6 — Login Grafana
1. Buka browser → `http://localhost:3000`
2. Login dengan:
   - Username: `simon`
   - Password: `gigadrillbreaker`

---

### Step 7 — Tambah Data Source Prometheus di Grafana
1. Klik **Connections → Data sources**
2. Klik **Add data source** → pilih **Prometheus**
3. Isi Prometheus server URL: `http://prometheus:9090`
4. Klik **Save & test** → pastikan muncul tanda hijau

---

### Step 8 — Buat Dashboard "Gurren Lagann Vitals"
1. Klik **Dashboards** → **New** → **New Dashboard**
2. Klik **Panel** (gambar grafik + tanda plus)
3. Klik **Configure visualization**

#### Panel 1 — Spiral Power Output (CPU)
- Ganti data source ke **Prometheus**
- Klik tombol **Code** di query A
- Paste query berikut:
```
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[1m])) * 100)
```
- Klik **Run queries**
- Isi Title: `Spiral Power Output (CPU)`
- Klik **Save**

#### Panel 2 — Core Structural Integrity (Memory)
- Klik **+ Add panel**
- Klik **Configure visualization**
- Ganti data source ke **Prometheus**
- Klik tombol **Code** di query A
- Paste query berikut:
```
(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100
```
- Klik **Run queries**
- Isi Title: `Core Structural Integrity (Memory)`
- Klik **Save**

3. Simpan dashboard dengan nama: **"Gurren Lagann Vitals"**
   - Klik ikon ⚙️ Settings → ubah Name → **Save**

---

### Step 9 — Restart Sistem & Buktikan Data Tidak Hilang
```bash
# Matikan semua container
docker compose down

# Nyalakan kembali
docker compose up -d
```

Setelah container kembali running:
1. Buka `http://localhost:3000`
2. Login dengan `simon` / `gigadrillbreaker`
3. Buka **Dashboards** → pastikan **"Gurren Lagann Vitals"** masih ada ✅
4. Pastikan kedua panel (CPU & Memory) masih tampil ✅
