# Architecture Diagrams – Mangala Online PWA

Bu doküman sistem mimarisinin görsel ve şema tabanlı açıklamalarını içerir. Diyagramlar geliştirme, planlama ve ekip içi iletişim için tek referanstır.

---

# 1) HIGH-LEVEL SYSTEM ARCHITECTURE DIAGRAM

```
               ┌──────────────────────────────┐
               │          FRONTEND (PWA)      │
               │  React + Vite + WebSocket    │
               └──────────────┬───────────────┘
                              │
                   REST API   │   WebSocket
                              │
        ┌──────────────────────┼─────────────────────────┐
        │                      │                         │
┌───────▼────────┐   ┌────────▼──────────┐    ┌─────────▼─────────┐
│  AUTH SERVICE  │   │ MATCHMAKING SVC   │    │ REALTIME GAME      │
│  (Google/Guest)│   │ Random + Room     │    │ ENGINE (WS)        │
└───────┬────────┘   └────────┬──────────┘    └─────────┬─────────┘
        │                     │                        │
        │                     │                        │
        │            ┌────────▼────────┐               │
        │            │  REDIS (State + │               │
        │            │   Match Queue)  │               │
        │            └────────┬────────┘               │
        │                     │                        │
        │            ┌────────▼────────┐               │
        └───────────► POSTGRESQL (DB)  ◄───────────────┘
                     │ Users, Match,   │
                     │ Tournament, Set │
                     └─────────────────┘
```

---

# 2) REALTIME GAME ENGINE FLOW (WebSocket)

```
Client                            Server (Game Engine)
  │                                      │
  │--- join_match ---------------------->│
  │                                      │ Load / Create game state
  │                                      │ Send initial state
  │<----------- state_update -------------│
  │                                      │
  │--- make_move(pitIndex) ------------->│
  │                                      │ Validate move
  │                                      │ Apply Rule Engine:
  │                                      │   - distribute stones
  │                                      │   - capture rule
  │                                      │   - empty pit opposite rule
  │                                      │   - end-of-set rule
  │                                      │ Update Redis game state
  │                                      │ Reset 30s timer
  │<----------- state_update -------------│
  │                                      │
  │                    [set finished]     │
  │<----------- set_result --------------│
  │                                      │
  │                    [match finished]   │
  │<----------- match_end ---------------│
```

---

# 3) MATCHMAKING FLOW (Random & Room)

## A) RANDOM MATCH

```
Client                 Matchmaking Service                Redis Queue
  │                            │                             │
  │--- random_match ---------->│                             │
  │                            │--- push user ---> queue ----│
  │                            │                             │
  │                            │<-- pop two users -----------│
  │                            │                             │
  │                            │--- create matchId ----------│
  │<------- match_found -------│
```

## B) ROOM MATCHING (Create + Join)

```
Creator Client                 Matchmaking SVC            Redis Room Store
   │                                  │                          │
   │--- create_room ----------------->│                          │
   │                                  │--- store roomCode ------>│
   │<----------- roomCode ------------│                          │

Joiner Client                   Matchmaking SVC            Redis Room Store
   │                                  │                          │
   │--- join_room(roomCode) --------->│                          │
   │                                  │--- lookup room ---------->│
   │                                  │<-- found (2 players) -----│
   │<----------- match_found ----------│
```

---

# 4) TOURNAMENT SWISS SYSTEM FLOW

```
               ┌──────────────────────────────┐
               │        CREATE TOURNAMENT     │
               └──────────────┬───────────────┘
                              │ players[]
                              │
                ┌─────────────▼───────────────┐
                │      ROUND 1 PAIRINGS       │
                │  (random or seeded)         │
                └─────────────┬───────────────┘
                              │ matches[]
                              │
                 ┌────────────▼──────────────┐
                 │     RECORD MATCHES        │
                 │ (winner gets 1 point)     │
                 └─────────────┬─────────────┘
                               │ standings
                               │
                ┌──────────────▼────────────────┐
                │  SORT BY POINTS (DESC)        │
                └──────────────┬────────────────┘
                               │
              ┌────────────────▼─────────────────┐
              │ GENERATE NEXT ROUND PAIRINGS    │
              │ Adjacent pairing based on score │
              └────────────────┬─────────────────┘
                               │
                               ▼
                       Repeat until finish
```

---

# 5) DATA FLOW – FROM LOGIN TO MATCH RESULT

```
User Login
   │
   ▼
Auth Service (Google/Guest)
   │ token
   ▼
Matchmaking (Random or Room)
   │ matchId
   ▼
WebSocket Game Engine
   │ real-time state
   ▼
Match Finished
   │ match result
   ▼
Tournament Service (if active)
   │ update standings
   ▼
Next Swiss Round
```

---

# 6) SERVICE RESPONSIBILITY MATRIX

```
AUTH SERVICE     →  Login, JWT, identity
MATCHMAKING SVC  →  Queues, rooms, match creation
GAME ENGINE      →  Rules, turns, timers, scoring
REDIS            →  Realtime match state, queues
POSTGRESQL       →  Users, matches, tournaments
TOURNAMENT SVC   →  Swiss rounds, standings, scoring
FRONTEND         →  UI + WebSocket client
```

---

# 7) ÖZET

Bu diyagram seti:

* Architect tarafından sistem bütünlüğünü
* Backend ekibi tarafından servis sınırlarını
* Frontend ekibi tarafından veri akışını
* DevOps tarafından servis bağımlılıklarını
  net görmek için kullanılır.

Görsel formatlara (draw.io, Mermaid, PNG) dönüştürülmesi istenirse ekleyebilirim.
