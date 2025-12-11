# 🧠 Mangala Online PWA
**Turnuva standartlarına uygun, gerçek zamanlı, web tabanlı Mangala oyunu.**  
Mobil öncelikli PWA tasarımı, server-authoritative oyun motoru, WebSocket tabanlı gerçek zamanlı iletişim, Google + Guest Auth, Swiss System turnuva modülü ile modern bir multiplayer deneyimi sunar.

---
# 📌 Özellikler
### 🎮 Oyun
- 3 setlik resmi Mangala formatı
- 4 temel kuralın tam uygulanması (hakem standartlarında)
- 30 saniyelik hamle zamanlayıcı
- Hileye kapalı **server-authoritative** mimari
- Gerçek zamanlı taş dağıtma, çiftleme, boş kuyu alma ve set bitişi

### 👥 Eşleşme
- Rastgele eşleşme (Random Matchmaking)
- Oda kurma + oda kodu ile bağlanma
- Gerçek zamanlı WebSocket iletişimi

### 🏆 Turnuva Modu
- İsviçre Sistemi (Swiss System)
- Otomatik eşleştirme
- Tur sonuçlarının kayıt altına alınması
- Sıralama (standings) hesaplama

### 📱 PWA (Mobile-First)
- Telefonlarda uygulama gibi çalışır
- Offline destek (opsiyonel)
- Performans odaklı React + Vite mimarisi

### 🔐 Kimlik Doğrulama
- Google ile giriş
- Misafir (Guest) modu
- JWT tabanlı backend doğrulama

---
# 🏗 Mimari Genel Bakış
Proje **NestJS + WebSocket + Redis + PostgreSQL** backend ile  
**React + Vite + PWA** frontend üzerine kuruludur.

Detaylar için:  
📄 `/docs/system-architecture.md`

## High-Level Architecture
```
Frontend (PWA)
   │    ├─ REST API (Auth, Matchmaking, Tournament)
   │    └─ WebSocket (Real-Time Engine)
Backend (NestJS)
   ├─ Auth
   ├─ Matchmaking
   ├─ Game Engine (WS)
   ├─ Rule Engine
   ├─ Timer Service
   ├─ Tournament
   └─ Redis + PostgreSQL
```

---
# 🧩 Proje Dizini (Özet)
```
backend/
  src/
    auth/
    matchmaking/
    game/
    tournament/
    database/
    common/

frontend/
  src/
    pages/
    components/
    services/
    app/
    store/
```
Tam liste için:  
📄 `/docs/backend-module-guide.md`  
📄 `/docs/frontend-architecture.md`

---
# 🔗 API Dokümantasyonu
Tüm REST endpointleri ve WebSocket eventleri:  
📄 `/docs/api-spec.md`

---
# 🗄 Database ERD
Modeller, ilişkiler ve şema:  
📄 `/docs/database-erd.md`

---
# 🎯 Sprint Planı (Roadmap)
4 sprintlik MVP geliştirme planı:
- Sprint 1 → Altyapı (Auth, DB, Redis, Setup)
- Sprint 2 → WebSocket + Rule Engine
- Sprint 3 → UI Tamamlama + Set/Match Yönetimi
- Sprint 4 → Turnuva Modu + Prod Deploy

Detaylar:  
📄 `/docs/sprint-roadmap.md`

---
# 🤖 VSCode Multi-Agent Geliştirme Modeli
Proje **multi-agent development workflow** kullanır.
- Architect Agent → yol haritası oluşturur
- Backend Agent → NestJS modüllerini oluşturur
- Frontend Agent → React + PWA geliştirmeyi yürütür
- DevOps Agent → Docker + CI/CD yönetir

Kurallar ve promptlar:  
📄 `/docs/agent-workflow.md`

---
# 🚀 Kurulum
## 1) Backend
```
cd backend
npm install
npm run prisma:generate
npm run prisma:migrate
npm run start:dev
```

## 2) Frontend
```
cd frontend
npm install
npm run dev
```

## 3) Docker ile Çalıştırma
```
docker compose up -d
```

---
# 🔥 MVP Hedefi
Bu repo tamamlandığında kullanıcılar:
- Gerçek zamanlı Mangala oynayabilecek,
- Turnuvalara katılabilecek,
- Mobil cihazlarından PWA olarak erişebilecek,
- Arkadaşlarıyla oda kurarak oynayabilecek,
- Hakem kural setine uygun bir deneyim yaşayacak.

---
# 📝 Lisans
MIT

---
# 👥 Katkı
Katkı kuralları PR template ve branch stratejisine göre yapılmalıdır.
📄 `/docs/branch-strategy.md`

---
# 🎉 Teşekkürler
Bu proje modern web teknolojileri ile geleneksel Mangala oyununu dijital dünyaya taşıma amacıyla geliştirilmiştir.  
Her katkı değerlidir!

