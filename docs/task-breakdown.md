# Task Breakdown (Backend + Frontend + DevOps)

Bu döküman, Mangala Online PWA projesindeki tüm geliştirme görevlerini modül bazlı ve JSON formatı ile tanımlar. Her görev, GitHub Flow stratejisine göre ayrı bir feature branch olarak açılacaktır.

---

# 1) BACKEND TASKS

Aşağıdaki tüm görevler NestJS + PostgreSQL + Redis mimarisine göre hazırlanmıştır.

```
[
  {
    "taskId": "BE-01",
    "title": "Auth Service (Google + Guest)",
    "files": ["auth.controller.ts", "auth.service.ts", "firebase.strategy.ts", "jwt.guard.ts"],
    "desc": "Google OAuth ve misafir girişi için kimlik doğrulama servisi. JWT üretimi ve doğrulama."
  },
  {
    "taskId": "BE-02",
    "title": "Matchmaking - Random Queue",
    "files": ["matchmaking.service.ts", "queue.manager.ts"],
    "desc": "Rastgele eşleşme kuyruğu için Redis tabanlı eşleştirme servisi. Rakip bulununca matchId üretimi."
  },
  {
    "taskId": "BE-03",
    "title": "Matchmaking - Room Service",
    "files": ["room.service.ts", "matchmaking.controller.ts"],
    "desc": "Oda oluşturma ve oda kodu ile eşleşmeye katılma mekanizması."
  },
  {
    "taskId": "BE-04",
    "title": "WebSocket Gateway Setup",
    "files": ["game.gateway.ts"],
    "desc": "Gerçek zamanlı iletişim için NestJS WebSocket Gateway yapılandırması. join_match, make_move event'leri."
  },
  {
    "taskId": "BE-05",
    "title": "Game State Store (Redis)",
    "files": ["state-store.ts"],
    "desc": "Her maçın oyun tahtası, timer ve oyuncu durumlarının Redis üzerinde saklanması."
  },
  {
    "taskId": "BE-06",
    "title": "Rule Engine - Distribute Stones",
    "files": ["distribute.ts"],
    "desc": "Mangala Temel Kural 1'e göre taş dağıtım algoritması."
  },
  {
    "taskId": "BE-07",
    "title": "Rule Engine - Capture Opponent Pit",
    "files": ["capture.ts"],
    "desc": "Son taş rakip kuyusunu çift yaparsa tüm taşların hazinelere aktarılması (Temel Kural 2)."
  },
  {
    "taskId": "BE-08",
    "title": "Rule Engine - Empty Pit Opposite Capture",
    "files": ["empty-pit.ts"],
    "desc": "Son taş kendi bölgesinde boş kuyuda biterse karşı kuyudaki taşların alınması (Temel Kural 3)."
  },
  {
    "taskId": "BE-09",
    "title": "Rule Engine - End of Game Set",
    "files": ["end-check.ts"],
    "desc": "Bir oyuncunun bölgesindeki taşlar bittiğinde rakibin taşlarının toplanması (Temel Kural 4)."
  },
  {
    "taskId": "BE-10",
    "title": "Turn Timer (30 seconds)",
    "files": ["turn-timer.ts", "game.service.ts"],
    "desc": "Her hamle sonrasında 30 saniyelik sayaç başlatılması ve süresi dolduğunda otomatik hamle sırası geçişi."
  },
  {
    "taskId": "BE-11",
    "title": "Set & Match Logic",
    "files": ["game.service.ts"],
    "desc": "3 setlik maç sistemi: set skorları, 2 set kazananın belirlenmesi, maç bitiş event'i."
  },
  {
    "taskId": "BE-12",
    "title": "Tournament - Create & Pairings",
    "files": ["tournament.service.ts", "swiss-system.ts", "tournament.controller.ts"],
    "desc": "Turnuva oluşturma, oyuncu ekleme, İsviçre eşleştirme algoritması."
  },
  {
    "taskId": "BE-13",
    "title": "Tournament - Submit Match Result",
    "files": ["tournament.service.ts", "scoring.ts", "ranking.ts"],
    "desc": "Her turun maç sonuçlarının işlenmesi, puan tablosunun hesaplanması."
  }
]
```

---

# 2) FRONTEND TASKS

```
[
  {
    "taskId": "FE-01",
    "title": "Auth UI (Google + Guest)",
    "files": ["Login.tsx", "auth.ts"],
    "desc": "Google ile giriş ve misafir girişi arayüzü + token yönetimi."
  },
  {
    "taskId": "FE-02",
    "title": "Matchmaking UI",
    "files": ["Home.tsx", "Room.tsx", "api.ts"],
    "desc": "Random match ve oda oluşturma/katılma arayüzleri."
  },
  {
    "taskId": "FE-03",
    "title": "Game Board UI",
    "files": ["Board/*", "Match.tsx", "websocket.ts"],
    "desc": "Kuyular, taşlar, animasyonlar, hazine, hamle göstergesi."
  },
  {
    "taskId": "FE-04",
    "title": "Timer Component",
    "files": ["Timer.tsx"],
    "desc": "30 saniyelik geri sayım göstergesi. Server state ile senkron olmalı."
  },
  {
    "taskId": "FE-05",
    "title": "Set & Match UI",
    "files": ["ScoreBoard.tsx", "Match.tsx"],
    "desc": "Set skorları, maç sonucu ve animasyonlu geçişler."
  },
  {
    "taskId": "FE-06",
    "title": "Tournament UI",
    "files": ["Tournament.tsx", "api.ts"],
    "desc": "Eşleşme listesi, sonuç ekranları ve standings görüntüleme."
  }
]
```

---

# 3) DEVOPS TASKS

```
[
  {
    "taskId": "DO-01",
    "title": "Docker Compose Setup",
    "files": ["docker-compose.yml"],
    "desc": "PostgreSQL + Redis + Backend + Frontend için Docker compose yapılandırması."
  },
  {
    "taskId": "DO-02",
    "title": "Backend Dockerfile",
    "files": ["backend/Dockerfile"],
    "desc": "NestJS prod image oluşturma."
  },
  {
    "taskId": "DO-03",
    "title": "Frontend Dockerfile",
    "files": ["frontend/Dockerfile"],
    "desc": "React PWA build ve static serving."
  },
  {
    "taskId": "DO-04",
    "title": "GitHub Actions CI/CD",
    "files": [".github/workflows/deploy.yml"],
    "desc": "Test → Build → Deploy pipeline."
  }
]
```

---

# 4) BRANCH ADLANDIRMA KURALI

Her görev için şu formatta branch açılır:

```
feature/<taskId>-<kısa-açıklama>
```

Örnek:

```
feature/BE-06-rule-engine-distribute
feature/FE-03-game-board-ui
```

---

Bu döküman, tüm geliştirici ajanların görev koordinasyonu için zorunlu bir referanstır.
