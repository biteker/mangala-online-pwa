# MANGALA ONLINE PWA – DATABASE ERD

Bu döküman PostgreSQL üzerinde kullanılacak tüm tabloları ve ilişkileri içerir.

---

## 1) USERS

```
users
- id (PK)
- name
- avatar
- authProvider (google | guest)
- createdAt
```

---

## 2) TOURNAMENTS

```
tournaments
- id (PK)
- name
- currentRound
- createdAt
```

---

## 3) TOURNAMENT PLAYERS

```
tournament_players
- id (PK)
- tournamentId (FK → tournaments.id)
- userId (FK → users.id)
- points (default 0)
```

---

## 4) SWISS ROUNDS

```
swiss_rounds
- id (PK)
- tournamentId (FK)
- roundNumber
```

---

## 5) MATCHES

```
matches
- id (PK)
- tournamentId (FK → tournaments.id, nullable)
- roundId (FK → swiss_rounds.id, nullable)
- playerA (FK → users.id)
- playerB (FK → users.id)
- winner (FK → users.id)
- createdAt
```

---

## 6) MATCH SETS

```
match_sets
- id (PK)
- matchId (FK → matches.id)
- setNumber
- scoreA
- scoreB
- winner (FK → users.id)
```

---

## 7) MATCH HISTORY (Opsiyonel)

```
match_history
- id (PK)
- matchId
- eventType
- payload (JSON)
- createdAt
```

---

## ERD İLİŞKİLERİ

```
users (1)───(∞) tournament_players ───(∞) tournaments
   │                                       │
   │                                       └──(1) swiss_rounds (∞)── matches (∞)── match_sets
   │                                                           │              
   └───────────────────────────(∞) matches (playerA, playerB, winner)
```

---

Bu ERD, NestJS + Prisma yapısı ile %100 uyumludur ve geliştirme ajanları tarafından doğrudan kullanılabilir.
