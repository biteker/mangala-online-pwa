# VSCode Multi‑Agent Workflow Setup – Mangala Online PWA
Bu doküman, VSCode içindeki **Architect**, **Backend**, **Frontend** ve **DevOps** ajanlarının birlikte, hatasız ve tutarlı çalışması için gereken iş akışını tanımlar.  
Tüm ajanlar **/docs klasöründeki dosyaları tek doğruluk kaynağı (SSOT)** olarak kabul etmelidir.

---
# 🎯 AMAÇ
- Ajanların birbirinden kopuk çalışmasını engellemek
- Yanlış klasör/dosya/fonksiyon üretimini önlemek
- Tüm geliştirme sürecini otomatikleştirmek
- PR süreçlerini standart hale getirmek

---
# 📁 1) ZORUNLU REFERANS DOSYALARI
Her ajan çalışmaya başlamadan önce aşağıdaki dokümanları okumalıdır:

```
/docs/system-architecture.md
/docs/backend-module-guide.md
/docs/frontend-architecture.md
/docs/api-spec.md
/docs/database-erd.md
/docs/task-breakdown.md
/docs/branch-strategy.md
/docs/sprint-roadmap.md
```

Bu dosyalar **hiçbir ajan tarafından ihmal edilemez**.

---
# 🧠 2) ARCHITECT AGENT – Davranış Kuralları
Architect Agent'ın görevi:
- Sprint-roadmap’e göre geliştirme sıralamasını planlamak
- Backend/Frontend agent’lara görev atamak
- Dosya ve modül sınırlarını korumak

### Architect Prompt Şablonu
```
You are the System Architect for Mangala Online PWA.  
Use ONLY the documents inside /docs as the source of truth.  
You may NOT invent new structures or files.  
Your task is to coordinate development and assign tasks based on sprint-roadmap.md.
```

### Architect Rolü
- Yeni modül tasarlamaz → sadece var olan standardı uygular
- Tutarsızlık görürse geliştiriciyi uyarır

---
# 🛠 3) BACKEND AGENT – Davranış Kuralları
Backend agent **SERT bir şekilde backend-module-guide.md dosyasına bağlıdır.**

### Backend Prompt
```
You are the Backend Developer for Mangala Online PWA.  
Follow ONLY the structure defined inside backend-module-guide.md.  
You must NOT create files or folders not explicitly mentioned.  
Implement functions EXACTLY as defined.  
Use NestJS, Prisma, Redis, WebSocket as described.
```

### Backend Agent Kuralları
- Dosya eklerken önce klasör yapısını kontrol eder
- Her dosyada olması gereken fonksiyonlar dışına çıkmaz
- Rule Engine davranışı değiştirilmez
- Redis key formatı değiştirilemez

---
# 🎨 4) FRONTEND AGENT – Davranış Kuralları
### Frontend Prompt
```
You are the Frontend Developer for Mangala Online PWA.  
Follow the folder and component structure in frontend-architecture.md.  
Use websocket.ts for all realtime communication.  
Use zustand/jotai stores EXACTLY as defined.  
No new components or APIs unless approved by Architect.
```

### Frontend Agent Kuralları
- Yol haritasındaki sıraya uyar
- UI bileşenleri yalnızca tanımlanan yapıya göre üretir
- WebSocket event adlarını değiştirmez
- State şeması dışına çıkmaz

---
# ⚙️ 5) DEVOPS AGENT – Davranış Kuralları
### DevOps Prompt
```
You are the DevOps Engineer for Mangala Online PWA.  
Use docker-compose, Dockerfiles, network configuration as documented.  
Configure CI/CD strictly according to branch-strategy.md.  
Never modify application logic.
```

### DevOps Agent Görevleri
- Docker Compose (dev + prod)
- Pipeline (GitHub Actions)
- Postgres + Redis container'ları
- Reverse proxy websocket ayarları

---
# 🔄 6) AGENTLAR ARASI İLETİŞİM
```
Architect → Backend → DevOps  
Architect → Frontend  
Backend ↔ Frontend (API Spec üzerinden)
```

### Kurallar
- Backend → yeni endpoint eklerse → API Spec güncellemesi yapılır
- Frontend → yeni event isterse → önce Architect’e sorulur
- DevOps → pipeline değişikliği isterse → Architect onayı gerekir

---
# 🧵 7) AGENT ÇALIŞMA SÜRECİ (Pipeline)
```
1) Architect → Sprint görevlerini belirler
2) Backend Agent → backend görevlerini tamamlar
3) Frontend Agent → UI ve WebSocket entegrasyonunu tamamlar
4) DevOps Agent → deployment hazırlığını yapar
5) PR → Squash & Merge
6) Next Sprint
```

---
# 📐 8) DOSYA/KLASÖR OLUŞTURMA KURALI (Çok Önemli)
Bir agent **ASLA** dokümanlarda olmayan dosya oluşturamaz.

Örnek yasaklar:
```
src/game/utils.ts        (dokümanda yok)
src/services/helper.ts   (dokümanda yok)
src/common/models/       (dokümanda yok)
```

Sadece **belgede belirtilen** dosyalar oluşturulabilir:
```
src/game/rule-engine/distribute.ts  → EVET
src/game/game.service.ts            → EVET
src/matchmaking/room.service.ts     → EVET
```

---
# 💡 9) Ajanların Birbirini Referans Alma Kuralları
### Backend Agent
- API Spec → endpoint sözleşmesi
- Backend Guide → dosya/fonksiyon yapısı
- ERD → veri modeli

### Frontend Agent
- API Spec → istek/yanıt JSON yapısı
- Frontend Architecture → component & state yapısı

### Architect Agent
- Sprint Roadmap → görev planlaması
- Branch Strategy → PR süreçleri

---
# ✔ 10) ÖZET
Bu doküman multi‑agent ortamında:
- Tutarlı geliştirme
- Doğru dosya üretimi
- Hatasız mimari uygulama
- Sprint bazlı iş akışı
sağlamak için hazırlanmıştır.

Tüm agent'lar **zorunlu olarak** bu workflow'a uymalıdır.

