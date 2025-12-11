# Sprint Roadmap – Mangala Online PWA (Detailed + GitHub Issue Format)
Bu döküman MVP geliştirme sürecini **çok daha detaylı** sprint görevlerine ayırır ve her görevi **GitHub Issue formatında** tekrar tanımlar.
Tüm görevler sprint sırasına göre düzenlenmiştir.

---
# 🏁 Sprint 1 — Core Setup (Backend + Frontend + DevOps)
**Amaç:** Projenin temel iskeletini, altyapı katmanlarını ve ilk modülleri kurmak.

---
## ✔ Sprint 1 Backend Tasks (Detaylı)
### **Issue: BE-01 – NestJS Projesinin Oluşturulması**
```
Title: BE-01 – Create base NestJS project
Assignee: Backend
Labels: backend, setup

Tasks:
- NestJS new project oluştur
- src/ klasör yapısını dokümana göre düzenle
- main.ts içinde CORS + validation pipe ekle
``` 

---
### **Issue: BE-02 – PostgreSQL + Prisma Setup**
```
Title: BE-02 – PostgreSQL + Prisma initialization
Labels: backend, database

Tasks:
- prisma init
- schema.prisma dosyasını ERD’ye göre oluştur
- PostgreSQL bağlantı testi
- migrate dev çalıştır
```

---
### **Issue: BE-03 – Redis Connection Setup**
```
Title: BE-03 – Redis integration for state & matchmaking queue
Labels: backend, redis

Tasks:
- ioredis kurulumu
- state-store.ts içinde bağlantı fonksiyonları
- queue.manager.ts boş iskelet
``` 

---
### **Issue: BE-04 – Auth Module (Google + Guest) Skeleton**
```
Title: BE-04 – Auth module skeleton
Labels: backend, auth

Tasks:
- auth.controller.ts temel endpointler
- auth.service.ts iskelet fonksiyonlar
- firebase.strategy.ts boş yapı
- jwt.guard.ts iskeleti
``` 

---
### **Issue: BE-05 – Matchmaking Module Skeleton**
```
Title: BE-05 – Matchmaking service skeleton
Labels: backend, matchmaking

Tasks:
- matchmaking.controller.ts iskeleti
- matchmaking.service.ts iskeleti
- queue.manager.ts tanımı
- room.service.ts tanımı
``` 

---
### **Issue: BE-06 – Tournament Module Skeleton**
```
Title: BE-06 – Tournament module base structure
Labels: backend, tournament
```

---
## ✔ Sprint 1 Frontend Tasks (Detaylı)
### **Issue: FE-01 – React + Vite + TypeScript Setup**
```
Title: FE-01 – Initialize React + Vite project
Labels: frontend, setup
```  

### **Issue: FE-02 – PWA Manifest Oluşturma**
```
Title: FE-02 – Create PWA manifest
Labels: frontend, pwa
```  

### **Issue: FE-03 – Auth UI Taslakları**
```
Title: FE-03 – Login screen (Google + Guest)
Labels: frontend, auth
```  

### **Issue: FE-04 – API Service Wrapper**
```
Title: FE-04 – api.ts (REST wrapper)
Labels: frontend, api
``` 

---
## ✔ Sprint 1 DevOps Tasks (Detaylı)
### **Issue: DO-01 – Docker Compose Setup (Postgres + Redis)**
```
Title: DO-01 – Docker compose for DB services
Labels: devops
``` 

### **Issue: DO-02 – Backend Dockerfile**
### **Issue: DO-03 – Frontend Dockerfile**

---
# ⚙️ Sprint 2 — WebSocket + Rule Engine + Oyun Motoru
**Amaç:** Gerçek zamanlı oyun altyapısının tamamlanması.

---
## ✔ Sprint 2 Backend Tasks (Detaylı)
### **Issue: BE-07 – WebSocket Gateway Setup**
```
Title: BE-07 – Implement WebSocket gateway (join_match, make_move)
Labels: websocket, backend
``` 

### **Issue: BE-08 – Game State Store (Redis)**
```
Title: BE-08 – Implement state-store.ts
Tasks:
- getMatchState()
- setMatchState()
- resetTimer()
``` 

### **Issue: BE-09 – Rule Engine – distribute.ts**
### **Issue: BE-10 – Rule Engine – capture.ts**
### **Issue: BE-11 – Rule Engine – empty-pit.ts**
### **Issue: BE-12 – Rule Engine – end-check.ts**
### **Issue: BE-13 – Rule Engine – validator.ts**

### **Issue: BE-14 – Turn Timer (30s)**
```
Tasks:
- startTimer()
- stopTimer()
- handleTimerExpire()
``` 

---
## ✔ Sprint 2 Frontend Tasks (Detaylı)
### **Issue: FE-05 – websocket.ts oluşturma**
### **Issue: FE-06 – gameStore & uiStore**
### **Issue: FE-07 – Board Component (temel)**
### **Issue: FE-08 – Pit click → make_move event**

---
# 🎮 Sprint 3 — UI + Set/Match Yönetimi
**Amaç:** Oyun ekranının tamamlanması.

---
## ✔ Sprint 3 Backend Tasks
### **Issue: BE-15 – Set bitişi hesaplama**
### **Issue: BE-16 – Match bitişi hesaplama**
### **Issue: BE-17 – state_update + set_result + match_end eventleri**

---
## ✔ Sprint 3 Frontend Tasks
### **Issue: FE-09 – Tam Board UI**
### **Issue: FE-10 – Timer Component**
### **Issue: FE-11 – ScoreBoard**
### **Issue: FE-12 – SetResult & MatchResult modalları**

---
# 🏆 Sprint 4 — Turnuva Modu + Production Hazırlığı
**Amaç:** MVP turnuva sisteminin tamamlanması.

---
## ✔ Sprint 4 Backend Tasks
### **Issue: BE-18 – Tournament API Implementasyonu**
### **Issue: BE-19 – Swiss System Pairing**
### **Issue: BE-20 – Turnuva puan hesaplama**

---
## ✔ Sprint 4 Frontend Tasks
### **Issue: FE-13 – Tournament Page**
### **Issue: FE-14 – Room Create/Join UI**
### **Issue: FE-15 – Home Menü**

---
## ✔ Sprint 4 DevOps Tasks
### **Issue: DO-04 – Production docker-compose**
### **Issue: DO-05 – GitHub Actions CI/CD Pipeline**

---
# ✔ BONUS: GitHub Issue Template (Kopyala → .github/ISSUE_TEMPLATE/task.md)
```
### 🎯 Task ID
<taskId>

### 📌 Summary
Kısa açıklama

### 🧩 Module
Backend / Frontend / DevOps

### 📁 Files
- <file1>
- <file2>

### ✔ Acceptance Criteria
- [ ] ...
- [ ] ...

### 🧪 Tests
- [ ] Unit tests
- [ ] Manual tests

### 🔗 Related Docs
- /docs/system-architecture.md
- /docs/backend-module-guide.md
- /docs/frontend-architecture.md
- /docs/api-spec.md
```

---
# 🧭 ÖZET
Bu detaylı sprint planı sayesinde:
- Her görev bir GitHub issue haline getirilebilir
- Agent’lara otomatik görev atanabilir
- Sprintler net bir çıktı üretir
- MVP yol haritası tamamen kontrol altındadır

Geliştiricilerin tek yapması gereken:  
**Her görevi GitHub'da bir issue olarak açmak.**

