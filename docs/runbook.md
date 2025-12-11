# RUNBOOK – Mangala Online PWA

Bu döküman geliştirme (DEV), test (STAGE) ve üretim (PROD) ortamlarında sistemin nasıl çalıştırılacağını, izlenecek adımları ve olası sorunlarda yapılacak işlemleri içerir.

---

# 1) ORTAMLAR

## Development (DEV)

* Lokal makinede çalışır
* Hot reload açıktır
* Mock veri opsiyoneldir
* Docker yalnızca PostgreSQL ve Redis için kullanılır

## Staging (STAGE)

* Production'a çok yakın ortam
* Test maçları, turnuva akış testleri burada yapılır
* Auth gerçek çalışır (Google)

## Production (PROD)

* Canlı kullanıcıların bağlandığı ortam
* Monitoring, log rotation, backup zorunludur

---

# 2) GELIŞTIRME ORTAMINI BAŞLATMA (DEV)

## 2.1 Docker Servislerini Başlat

```
docker compose up -d
```

Açılacak servisler:

* PostgreSQL : 5432
* Redis : 6379

## 2.2 Backend Çalıştır

```
cd backend
npm install
npm run prisma:generate
npm run prisma:migrate
npm run start:dev
```

Backend açılınca: `http://localhost:3000`

## 2.3 Frontend Çalıştır

```
cd frontend
npm install
npm run dev
```

Frontend açılınca: `http://localhost:5173`

---

# 3) DEPLOYMENT (STAGE + PROD)

Deployment **Docker imajları** üzerinden yapılır.

## 3.1 Backend Build

```
cd backend
docker build -t mangala-backend:latest .
```

## 3.2 Frontend Build

```
cd frontend
docker build -t mangala-frontend:latest .
```

## 3.3 Sunucuda Servisleri Başlat

```
docker compose -f docker-compose.prod.yml up -d
```

---

# 4) PROD MONITORING

## 4.1 Loglar

Backend logları:

```
docker logs mangala-backend -f
```

Frontend logları:

```
docker logs mangala-frontend -f
```

Redis logları:

```
docker logs redis -f
```

## 4.2 Performans İzleme

Aşağıdaki değerler kritik:

* WebSocket latency < 200 ms
* Redis CPU < %60
* PostgreSQL bağlantı sayısı: max 20–30

---

# 5) BACKUP & RESTORE

## 5.1 PostgreSQL Backup

```
docker exec -t postgres pg_dump -U user mangala > backup.sql
```

## 5.2 Restore

```
cat backup.sql | docker exec -i postgres psql -U user -d mangala
```

---

# 6) SIK KARŞILAŞILAN SORUNLAR

### ❌ Backend 3000 portunda açılmıyor

Çözüm:

```
lsof -i :3000
kill -9 <pid>
```

### ❌ WebSocket bağlanmıyor

* VITE_WS_URL yanlış olabilir
* Reverse proxy websocket upgrade header’larını engelliyor olabilir

### ❌ Redis bağlantısı yok

```
docker restart redis
```

### ❌ Google Auth çalışmıyor

* Redirect URL Google Console’a eklenmemiştir

---

# 7) PROD HATA DURUMUNDA YAPILACAKLAR (RUNBOOK)

### 1) Hata → Log Kaydı

```
docker logs mangala-backend --tail 200
```

### 2) Redis Durumu Kontrol

```
docker exec -it redis redis-cli ping
```

### 3) PostgreSQL bağlantı testi

```
docker exec -it postgres psql -U user -d mangala -c "select now();"
```

### 4) Gerekirse Servisleri Yeniden Başlat

```
docker compose down
docker compose up -d
```

---

# 8) GÜNCELLEME VE RELEASE YÖNETİMİ

### 8.1 Yeni sürüm çıkarma

```
git checkout main
git pull
npm version patch
```

### 8.2 PROD’a deploy

```
docker build ...
docker compose -f docker-compose.prod.yml up -d
```

---

# 9) SORUMLULUKLAR

### DevOps

* PROD izleme
* Backup ve security

### Backend

* API stabilitesi
* WebSocket sürekliliği

### Frontend

* PWA performansı
* UI bug fix

---

Bu runbook, tüm ekip tarafından PROD ortamında yaşanan her sorun için başvurulacak ana kılavuzdur.
