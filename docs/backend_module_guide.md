# Backend Module Guide – Mangala Online PWA (Full Specification)

Bu döküman backend mimarisinin **tam klasör yapısını**, **her dosyanın görevlerini**, **içermesi gereken fonksiyon prototiplerini** ve modüller arası veri akışını içerir.
VSCode Backend Agent bu dokümana göre çalışmalı, **kendi dosya isimlerini türetmemelidir.**

---

# 📁 1) Backend Klasör Yapısı (Final)

```
src/
 ├─ auth/
 │   ├─ auth.controller.ts
 │   ├─ auth.service.ts
 │   ├─ firebase.strategy.ts
 │   └─ jwt.guard.ts
 │
 ├─ matchmaking/
 │   ├─ matchmaking.controller.ts
 │   ├─ matchmaking.service.ts
 │   ├─ queue.manager.ts
 │   └─ room.service.ts
 │
 ├─ game/
 │   ├─ game.gateway.ts
 │   ├─ game.service.ts
 │   ├─ state-store.ts
 │   ├─ rule-engine/
 │   │   ├─ distribute.ts
 │   │   ├─ capture.ts
 │   │   ├─ empty-pit.ts
 │   │   ├─ end-check.ts
 │   │   └─ validator.ts
 │   ├─ timers/
 │   │   └─ turn-timer.ts
 │   └─ dto/
 │       └─ move.dto.ts
 │
 ├─ tournament/
 │   ├─ tournament.controller.ts
 │   ├─ tournament.service.ts
 │   ├─ swiss-system.ts
 │   ├─ scoring.ts
 │   └─ ranking.ts
 │
 ├─ database/
 │   ├─ prisma.service.ts
 │   └─ schema.prisma
 │
 ├─ common/
 │   ├─ exceptions/
 │   ├─ filters/
 │   └─ utils/
 │
 └─ main.ts
```

---

# 🔐 2) AUTH MODULE (Detaylı Tanım)

## auth.controller.ts

Fonksiyonlar:

```
POST /auth/google → googleLogin()
POST /auth/guest  → guestLogin()
```

Görevler:

* istekleri service’a yönlendirmek

## auth.service.ts

Fonksiyonlar:

```
validateGoogleToken(token: string)
getOrCreateUser(profile)
createGuestUser()
generateJWT(user)
```

Görevler:

* Google token doğrulama
* user oluşturma / çekme
* JWT üretme

## firebase.strategy.ts

```
validate(idToken: string)
```

Görev:

* Google tarafından üretilen idToken’ı doğrulamak

---

# 👥 3) MATCHMAKING MODULE

## matchmaking.controller.ts

Fonksiyonlar:

```
POST /matchmaking/random → joinRandomQueue()
POST /matchmaking/room/create → createRoom()
POST /matchmaking/room/join   → joinRoom()
```

## matchmaking.service.ts

Fonksiyonlar:

```
queueRandomUser(userId)
findOpponentInQueue()
createMatch(playerA, playerB)
```

## queue.manager.ts

```
pushToQueue(userId)
popTwoFromQueue(): [userA,userB] | null
```

Redis Yapısı:

```
matchmaking_queue (LIST)
```

## room.service.ts

```
createRoom(userId)
joinRoom(roomCode, userId)
isRoomFull(roomCode)
deleteRoom(roomCode)
```

Redis Yapısı:

```
rooms:<roomCode> (HASH)
```

---

# 🕹 4) GAME ENGINE MODULE

## game.gateway.ts (WebSocket)

Fonksiyonlar:

```
handleConnection()
handleDisconnect()
handleJoinMatch(client, payload)
handleMakeMove(client, payload)
sendStateUpdate(matchId)
sendSetResult(matchId)
sendMatchEnd(matchId)
```

## game.service.ts

Ana fonksiyonlar:

```
joinMatch(matchId, userId)
makeMove(matchId, userId, pitIndex)
applyRules(matchState)
updateState(matchId, newState)
checkSetEnd(matchState)
checkMatchEnd(matchState)
```

Görev:

* Oyun state’ini kontrol etmek
* Rule Engine’i çağırmak
* Set/Match bitişini yönetmek

## state-store.ts (Redis)

Fonksiyonlar:

```
getMatchState(matchId)
setMatchState(matchId, state)
resetTimer(matchId, playerId)
```

Redis Key Formatı:

```
match:<id>
match:<id>:timer:<playerId>
```

---

# 🧠 5) RULE ENGINE (Tüm Kural Dosyaları + Fonksiyonlar)

## distribute.ts

```
distributeStones(board, pitIndex, player)
getNextPitIndex(currentIndex)
```

Görev: Taşları doğru yönde dağıtmak (Kural 1)

## capture.ts

```
applyCaptureRule(board, lastPit, player)
```

Görev: Çiftleme kuralı (Kural 2)

## empty-pit.ts

```
applyEmptyPitRule(board, lastPit, player)
```

Görev: Boş kuyu → karşı kuyuyu alma (Kural 3)

## end-check.ts

```
checkEndOfSet(board, player)
transferRemainingStones()
```

Görev: Bölge taşları biterse seti bitirme (Kural 4)

## validator.ts

```
validateMove(board, pitIndex, player)
isPlayerPit(pitIndex, player)
```

Görev: Hamle kurallarını ihlal eden durumları engellemek

---

# ⏱ 6) TURN TIMER MODULE

## turn-timer.ts

```
startTimer(matchId, playerId, seconds = 30)
stopTimer(matchId)
handleTimerExpire(matchId, playerId)
```

Görev:

* Her turda 30 saniye saymak
* Süre bitince otomatik sıra geçirme

Redis Kullanımı:

```
SETEX match:<id>:timer:<playerId> 30 "active"
```

---

# 🏆 7) TOURNAMENT MODULE (Full Spec)

## tournament.controller.ts

```
POST /tournament/create → createTournament()
GET  /tournament/:id/pairings → getPairings()
POST /tournament/:id/result  → submitResult()
```

## tournament.service.ts

```
createTournament(name, players)
getCurrentRound(tournamentId)
submitMatchResult(tournamentId, matchId, winner)
updateStandings(tournamentId)
```

## swiss-system.ts

```
generatePairings(players)
sortPlayersByPoints(players)
```

## scoring.ts

```
addPoint(userId)
calculateStandings(players)
```

## ranking.ts

```
rankPlayers(players)
```

---

# 📦 8) DATABASE MODULE

## prisma.service.ts

Görev: DB bağlantısı + query interface

## schema.prisma

İçermesi gereken modeller:

```
User
Match
MatchSet
Tournament
TournamentPlayer
SwissRound
```

---

# 🧩 9) MODULELER ARASI ETKİLEŞİM

```
Auth → Matchmaking → Game Engine → Tournament → DB
```

---

# 🧱 10) AGENT KONTROL NOTU (KRİTİK)

Backend Agent **yalnızca** bu dokümanda belirtilen:

* klasörleri
* dosya adlarını
* fonksiyonları
  oluşturmakla yükümlüdür.

Hiçbir agent **kendi kafasına göre dosya üretmemeli**, sadece bu spesifikasyona bağlı kalmalıdır.

---

# ✔ ÖZET

Bu doküman backend tarafının *nihai, üretime hazır, tam davranış modelini* tanımlar ve tüm geliştirici ajanlar tarafından referans alınmalıdır.
