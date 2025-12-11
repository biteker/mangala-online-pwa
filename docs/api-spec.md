MANGALA ONLINE PWA – API SPECIFICATION (Production Ready)

Bu doküman tüm HTTP REST API endpoint’lerini ve WebSocket event sözleşmelerini içerir.
Backend servisleri:

Auth

Matchmaking

Tournament

Game WebSocket Engine

Mimari referans: /docs/system-architecture.md

------------------------------------------------------------
1) 🔐 AUTH API
------------------------------------------------------------
POST /auth/google

Google OAuth ile giriş.

Request
{
  "idToken": "string"
}

Response
{
  "userId": "string",
  "name": "string",
  "avatar": "string",
  "token": "jwt"
}

POST /auth/guest

Misafir kullanıcı giriş yapar.

Response
{
  "userId": "guest_x3a91",
  "name": "Player_391",
  "token": "jwt"
}

------------------------------------------------------------
2) 👥 MATCHMAKING API
------------------------------------------------------------
POST /matchmaking/random

Rastgele eşleşme kuyruğuna girer.

Request
{
  "userId": "string"
}

Response (rakip bulunduğunda)
{
  "matchId": "m123",
  "player1": "u100",
  "player2": "u233"
}

POST /matchmaking/room/create

Özel oda oluşturur.

Response
{
  "roomCode": "82541"
}

POST /matchmaking/room/join

Oda kodu ile oyuna katılır.

Request
{
  "userId": "string",
  "roomCode": "82541"
}

Response
{
  "matchId": "m991",
  "opponentId": "u344"
}

------------------------------------------------------------
3) 🕹 GAME ENGINE – WEBSOCKET SPEC
------------------------------------------------------------

Tüm event isimleri standardized edilmiştir.

🔸 Client → Server Event’leri
1) join_match
{
  "matchId": "string",
  "userId": "string"
}

2) make_move

Hamle isteği.

{
  "pitIndex": 0
}

🔸 Server → Client Event’leri
1) state_update

Oyun tahtasının güncel versiyonu.

{
  "board": [4,4,4,4,4,4, 4,4,4,4,4,4],
  "storeA": 0,
  "storeB": 0,
  "currentPlayer": "u233",
  "setScore": {
    "A": 0,
    "B": 0
  },
  "timers": {
    "A": 22,
    "B": 30
  }
}

2) invalid_move

Hamle kurallara aykırıysa.

{
  "error": "Invalid pit selection."
}

3) set_result

Set bitince gönderilir.

{
  "setNumber": 1,
  "winner": "u233"
}

4) match_end

3 setlik maçın sonucu.

{
  "winner": "u233",
  "scores": {
    "A": 2,
    "B": 1
  }
}

------------------------------------------------------------
4) 🏆 TOURNAMENT API
------------------------------------------------------------

Turnuva modülü İsviçre eşleştirme sistemi kullanır.

POST /tournament/create

Yeni turnuva oluşturur.

Request
{
  "name": "Okul Turnuvası",
  "players": ["u1","u2","u3","u4"]
}

Response
{
  "tournamentId": "t1001"
}

GET /tournament/{id}/pairings

Aktif turun eşleşmeleri.

Response
{
  "round": 2,
  "pairings": [
    { "p1": "u1", "p2": "u4" },
    { "p1": "u2", "p2": "u3" }
  ]
}

POST /tournament/{id}/result

Bir maçın sonucunu işler.

Request
{
  "matchId": "m233",
  "winner": "u3"
}

Response
{
  "status": "ok"
}

GET /tournament/{id}/standings

Turnuvadaki puan durumunu verir.

Response
{
  "round": 2,
  "standings": [
    { "userId": "u3", "points": 2.0 },
    { "userId": "u1", "points": 1.0 },
    { "userId": "u4", "points": 1.0 },
    { "userId": "u2", "points": 0.0 }
  ]
}

------------------------------------------------------------
5) ⚙️ COMMON RESPONSE CODES
------------------------------------------------------------
Kod	Açıklama
200 OK	Başarılı
201 Created	Yeni kaynak oluşturuldu
400 Bad Request	Eksik/yanlış veri
401 Unauthorized	Token geçersiz
403 Forbidden	Yetki yok
404 Not Found	Kaynak yok
409 Conflict	Aynı odada ikinci giriş
500 Server Error	Beklenmeyen hata
------------------------------------------------------------
6) 📌 OBJECT SCHEMAS
------------------------------------------------------------
User
{
  "userId": "string",
  "name": "string",
  "avatar": "string",
  "createdAt": "2025-01-01T00:00:00Z"
}

Match
{
  "matchId": "string",
  "playerA": "string",
  "playerB": "string",
  "sets": [
    { "winner": "string", "scoreA": 25, "scoreB": 23 }
  ]
}

Tournament
{
  "tournamentId": "string",
  "name": "string",
  "round": 1,
  "players": ["u1","u2","u3"]
}

------------------------------------------------------------
7) 🔒 AUTH REQUIREMENTS
------------------------------------------------------------

Tüm endpoint’ler JWT ister.
Aşağıdaki hariç:

/auth/google

/auth/guest

WebSocket bağlantısı da JWT ile doğrulanır.

📌 SONUÇ

Bu API SPEC artık:

Backend ekiplerinin kodlamaya başlaması için yeterli

Frontend ekiplerinin tüm bağlantıyı kurabilmesi için tam uyumlu

Architect agent’ın planlama yapması için standart

DevOps agent’ın route ve environment hazırlaması için stabil