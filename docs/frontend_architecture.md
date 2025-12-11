# Frontend Architecture – Mangala Online PWA
Bu doküman Mangala Online uygulamasının **frontend mimarisini**, React bileşen yapısını, state yönetimini, WebSocket iletişimini, PWA davranışını ve UI akışlarını tanımlar. Frontend geliştiricileri ve VSCode agent'ları için ana referanstır.

---
# 1) TEKNOLOJİ STACKİ
- **React + Vite** (Performans odaklı)
- **TypeScript** (Tip güvenliği)
- **Zustand** veya **Jotai** (minimal state management)
- **WebSocket Client** (gerçek zamanlı oyun)
- **REST API** (Auth, matchmaking, turnuva)
- **PWA Support** (manifest + service worker)

---
# 2) KLASÖR YAPISI
```
frontend/
 ├─ src/
 │   ├─ app/
 │   │   ├─ routes.tsx
 │   │   └─ store/
 │   │       ├─ userStore.ts
 │   │       ├─ gameStore.ts
 │   │       └─ uiStore.ts
 │   │
 │   ├─ pages/
 │   │   ├─ Home.tsx
 │   │   ├─ Login.tsx
 │   │   ├─ Match.tsx
 │   │   ├─ Room.tsx
 │   │   └─ Tournament.tsx
 │   │
 │   ├─ components/
 │   │   ├─ Board/
 │   │   │   ├─ Board.tsx
 │   │   │   ├─ Pit.tsx
 │   │   │   └─ Stone.tsx
 │   │   ├─ Timer.tsx
 │   │   ├─ ScoreBoard.tsx
 │   │   ├─ MatchResult.tsx
 │   │   └─ RoomCode.tsx
 │   │
 │   ├─ services/
 │   │   ├─ api.ts
 │   │   ├─ websocket.ts
 │   │   └─ auth.ts
 │   │
 │   ├─ hooks/
 │   ├─ assets/
 │   └─ main.tsx
 │
 ├─ public/
 └─ vite.config.ts
```

---
# 3) UI AKIŞI
```
Login → Home → (Random Match / Room Match)
                 │
                 ├── Random → Match.tsx
                 └── Room → Room.tsx → Match.tsx

Match.tsx → (state updates via WebSocket)

Match End → Tournament Service (varsa)
```

---
# 4) WEBSOCKET MİMARİSİ
`websocket.ts` dosyası tüm gerçek zamanlı bağlantı yönetimini içerir.

## 4.1 Başlatma
```
const ws = new WebSocket(import.meta.env.VITE_WS_URL)
```

## 4.2 Event Listener Yapısı
```
ws.onmessage = (event) => {
  const msg = JSON.parse(event.data)
  switch (msg.type) {
    case 'state_update': gameStore.setState(msg.payload); break
    case 'set_result':  uiStore.showSetModal(msg.payload); break
    case 'match_end':   uiStore.showMatchResult(msg.payload); break
    case 'invalid_move': uiStore.showError(msg.payload.error); break
  }
}
```

## 4.3 Client → Server Event Gönderme
```
function makeMove(pitIndex) {
  ws.send(JSON.stringify({ type: "make_move", pitIndex }))
}
```

---
# 5) STATE MANAGEMENT
State üç ana kategoriye ayrılır:

## 5.1 User Store (`userStore.ts`)
- userId
- name
- avatar
- token
- isAuthenticated

## 5.2 Game Store (`gameStore.ts`)
```
{
  board: [],
  storeA: 0,
  storeB: 0,
  currentPlayer: null,
  setScore: { A: 0, B: 0 },
  timers: { A: 30, B: 30 }
}
```

## 5.3 UI Store (`uiStore.ts`)
- Loading state
- Modal state
- Error message
- Match result popup

---
# 6) BOARD COMPONENT MİMARİSİ
```
Board
 ├─ Pit (12 adet)
 ├─ Store (A & B)
 ├─ Timer
 └─ ScoreBoard
```

## Pit Davranışı:
- Tıklanınca `make_move(pitIndex)` çağrılır
- Eğer player’ın sırası değilse tıklanamaz
- Animasyonlar opsiyonel (stone movement fake)

---
# 7) TIMER COMPONENT
Timer **server state’e bağlıdır**.  
Client sadece gösterir — sayaç yönetilmez.

Gösterim:
```
remaining = gameStore.timers[playerId]
```

---
# 8) MATCH PAGE AKIŞI
```
useEffect → join_match event’i gönderilir
Backend → state_update döner
User → pit seçer → make_move
Backend → yeni state gönderir
```

Set bitince:
```
uiStore.showSetResultModal
```

Match bitince:
```
uiStore.showMatchResult
navigate('/tournament'?)
```

---
# 9) API SERVICE (REST)
`api.ts` içerisinde axios veya fetch tabanlı wrapper bulunur.

Örnek:
```
export function createRoom() {
  return api.post('/matchmaking/room/create')
}
```

---
# 10) AUTH FLOW
```
Login.tsx → Google login
 → auth.ts üzerinden /auth/google
 → userStore.setUser
 → navigate('/home')
```

Guest login:
```
POST /auth/guest
```

---
# 11) PWA DOSYALARI
`public/manifest.json`:
```
{
  "name": "Mangala Online",
  "short_name": "Mangala",
  "display": "standalone",
  "icons": [...],
  "theme_color": "#222",
  "background_color": "#fff"
}
```

Service Worker:
- asset caching
- offline fallback (opsiyonel)

---
# 12) FRONTEND ERROR HANDLING
- WebSocket error → tam ekran uyarı
- invalid_move → popup
- API hatası → toast notification

---
# 13) ÖZET
Bu doküman frontend ajanları için tek referans mimaridir:
- UI akışları
- WebSocket iletişimi
- State yönetimi
- PWA davranışı
- Bileşen yapısı

Frontend geliştirme bu standartlara uymalıdır.

