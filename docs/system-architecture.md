# MANGALA ONLINE PWA – SYSTEM ARCHITECTURE (Production Ready)

Bu doküman, Mangala Online uygulamasının teknik mimarisini tanımlar ve tüm geliştirici agent'ların uyacağı tek kaynaklı referanstır.  
PRD: (Mangala Turnuva Kuralları + MVP Gereksinimleri)

---

# 1. High-Level Architecture

## Ana Modüller
- Auth Module  
- Matchmaking Module  
- Real-Time Game Engine (WebSocket)  
- Rule Engine (4 resmi Mangala kuralı)  
- Tournament Module (Swiss System)  
- Timer Service (30 saniye)  
- State Store (Redis)  
- Persistent Database (PostgreSQL)

## Servis Diyagramı

```
Client (PWA)
   │
   ├─ REST API (Auth / Matchmaking / Tournament)
   │
   └─ WebSocket (Real-Time Game)
           ├─ Join Match
           ├─ Make Move
           ├─ Sync Game State
           ├─ Timer Events
           └─ Match Result Sync
```

---

# 2. Backend Architecture (NestJS)

```
backend/
 ├─ src/
 │   ├─ auth/
 │   │   ├─ auth.controller.ts
 │   │   ├─ auth.service.ts
 │   │   ├─ firebase.strategy.ts
 │   │   └─ jwt.guard.ts
 │   │
 │   ├─ matchmaking/
 │   │   ├─ matchmaking.controller.ts
 │   │   ├─ matchmaking.service.ts
 │   │   ├─ room.service.ts
 │   │   └─ queue.manager.ts (Redis)
 │   │
 │   ├─ game/
 │   │   ├─ game.gateway.ts
 │   │   ├─ game.service.ts
 │   │   ├─ state-store.ts (Redis)
 │   │   ├─ rule-engine/
 │   │   │   ├─ distribute.ts
 │   │   │   ├─ capture.ts
 │   │   │   ├─ empty-pit.ts
 │   │   │   ├─ end-check.ts
 │   │   │   └─ validator.ts
 │   │   ├─ timers/
 │   │   │   └─ turn-timer.ts
 │   │   └─ dto/
 │   │
 │   ├─ tournament/
 │   │   ├─ tournament.controller.ts
 │   │   ├─ tournament.service.ts
 │   │   ├─ swiss-system.ts
 │   │   ├─ scoring.ts
 │   │   └─ ranking.ts
 │   │
 │   ├─ database/
 │   │   ├─ prisma.service.ts
 │   │   ├─ migrations/
 │   │   └─ schema.prisma
 │   │
 │   ├─ common/
 │   └─ main.ts
 └─ test/
```

---

# 3. Real-Time Game Flow (WebSocket)

## 1) Oyuncu “join_match”
- Redis’te match state oluşturulur.
- Oyuncular eklenir.
- Game state client’a gönderilir.

## 2) Oyuncu “make_move”
- Rule Engine resmi Mangala kurallarını uygular:
  - Taş dağıtma  
  - Çiftleme (tek → çift olursa bütün taşlar)  
  - Boş kuyu → karşı kuyuyu alma  
  - Bölge taşları bitti → tüm taşları kazanma  
- Timer reset olur.
- Yeni state broadcast edilir.

## 3) Set & Match Yönetimi
- 1 maç: 3 set
- 2 set alan → kazanır
- Sonuç Tournament servisine işlenir (varsa)

---

# 4. Technology Stack

| Katman | Teknoloji |
|-------|-----------|
| Backend | NestJS (TypeScript) |
| Realtime | WebSocket Gateway |
| DB | PostgreSQL (Prisma) |
| Cache / State | Redis |
| Auth | Firebase Auth (opsiyonel) + JWT |
| Frontend | React + Vite (PWA) |
| Deployment | Docker |

---

# 5. Database (PostgreSQL High-Level)

- users  
- tournaments  
- swiss_rounds  
- matches  
- match_sets  
- match_history  

---

# 6. Non-Functional Requirements

- Hamle gecikmesi < 200ms  
- Timer drift: < 1s  
- 100+ eşzamanlı maç desteklenmeli  
- Tüm oyun motoru hileye kapalı (server-authoritative)  

---

# 7. Kullanım

Bu dosya, Architect Agent ve Backend/Frontend geliştirme ajanlarının **tek referans mimari dokümanı**dır.

