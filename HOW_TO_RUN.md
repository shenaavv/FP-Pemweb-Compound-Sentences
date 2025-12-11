# 🎮 COMPOUND SENTENCES GAME - CARA MENJALANKAN

## ✅ STATUS SAAT INI

### Backend
- **Status:** ✅ RUNNING
- **Port:** 3000
- **URL:** http://localhost:3000
- **API Base:** http://localhost:3000/api

### Frontend  
- **Status:** ✅ RUNNING
- **Port:** 3003
- **URL:** http://localhost:3003
- **Game URL:** http://localhost:3003/compound-sentences

### Database (PostgreSQL)
- **Container:** fp-database (System PostgreSQL - bukan Docker)
- **Database:** wordit_db
- **User:** admin
- **Password:** password123
- **Port:** 5432
- **Status:** ✅ CONNECTED

---

## 🚀 CARA MENJALANKAN

### 1. Start Backend
```bash
cd FP-PemrogramanWebsite-BE-2025
bun run start:dev
```

**Expected Output:**
```
Server is running on Port 3000 💫
```

### 2. Start Frontend
```bash
cd FP-PemrogramanWebsite-FE-2025
npm run dev
```

**Expected Output:**
```
VITE v7.2.2  ready in 310 ms
➜  Local:   http://localhost:3003/
```

### 3. Buka Browser
- **Game:** http://localhost:3003/compound-sentences
- **Homepage:** http://localhost:3003/

---

## 🧪 TESTING API DENGAN APIDOG

### Setup Apidog Collection

#### 1. Get All Games
**Method:** GET  
**URL:** `http://localhost:3000/api/game/compound-sentences`  
**Headers:** None required

**Response Example:**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Games fetched successfully",
  "data": [
    {
      "id": "cs-game-001",
      "name": "Basic Compound Sentences",
      "description": "Learn to match compound sentences for beginners",
      "thumbnail": "/placeholder-thumbnail.png",
      "totalPlayed": 0,
      "totalLikes": 0
    }
  ]
}
```

#### 2. Get Game Data (Start Game)
**Method:** GET  
**URL:** `http://localhost:3000/api/game/compound-sentences/cs-game-001`  
**Headers:** None required

**Response:** Returns game with shuffled sentence parts

#### 3. Submit Answers
**Method:** POST  
**URL:** `http://localhost:3000/api/game/compound-sentences/validate`  
**Headers:**
```
Content-Type: application/json
```

**Body (Raw JSON):**
```json
{
  "gameId": "cs-game-001",
  "answers": [
    {"firstPartId": "first-pair-1", "secondPartId": "second-pair-1"},
    {"firstPartId": "first-pair-2", "secondPartId": "second-pair-2"},
    {"firstPartId": "first-pair-3", "secondPartId": "second-pair-3"},
    {"firstPartId": "first-pair-4", "secondPartId": "second-pair-4"},
    {"firstPartId": "first-pair-5", "secondPartId": "second-pair-5"},
    {"firstPartId": "first-pair-6", "secondPartId": "second-pair-6"},
    {"firstPartId": "first-pair-7", "secondPartId": "second-pair-7"},
    {"firstPartId": "first-pair-8", "secondPartId": "second-pair-8"}
  ]
}
```

**Response Example (All Correct):**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Answers validated successfully",
  "data": {
    "score": 100,
    "totalQuestions": 8,
    "correctAnswers": 8,
    "results": [...]
  }
}
```

---

## 🧪 TESTING DENGAN CURL

### Test Get All Games
```bash
curl http://localhost:3000/api/game/compound-sentences
```

### Test Get Specific Game
```bash
curl http://localhost:3000/api/game/compound-sentences/cs-game-001
```

### Test Submit Answers
```bash
curl -X POST http://localhost:3000/api/game/compound-sentences/validate \
  -H "Content-Type: application/json" \
  -d '{
    "gameId": "cs-game-001",
    "answers": [
      {"firstPartId": "first-pair-1", "secondPartId": "second-pair-1"},
      {"firstPartId": "first-pair-2", "secondPartId": "second-pair-2"},
      {"firstPartId": "first-pair-3", "secondPartId": "second-pair-3"},
      {"firstPartId": "first-pair-4", "secondPartId": "second-pair-4"},
      {"firstPartId": "first-pair-5", "secondPartId": "second-pair-5"},
      {"firstPartId": "first-pair-6", "secondPartId": "second-pair-6"},
      {"firstPartId": "first-pair-7", "secondPartId": "second-pair-7"},
      {"firstPartId": "first-pair-8", "secondPartId": "second-pair-8"}
    ]
  }'
```

---

## 🗄️ DATABASE COMMANDS

### Connect to Database
```bash
PGPASSWORD=password123 psql -h localhost -U admin -d wordit_db
```

### Check Data
```sql
-- Check all games
SELECT id, name, is_published FROM "Games";

-- Check game templates
SELECT id, name, slug FROM "GameTemplates";

-- Check game details
SELECT name, game_json FROM "Games" WHERE id = 'cs-game-001';

-- Check play count
SELECT name, total_played FROM "Games";
```

### Manual Data Reset (if needed)
```bash
cd FP-PemrogramanWebsite-BE-2025
PGPASSWORD=password123 psql -h localhost -U admin -d wordit_db -f prisma/manual-seed.sql
```

---

## 🔧 TROUBLESHOOTING

### Backend tidak start
```bash
# Check if port 3000 is in use
lsof -i :3000

# Kill process if needed
kill -9 $(lsof -t -i:3000)

# Restart backend
cd FP-PemrogramanWebsite-BE-2025
bun run start:dev
```

### Frontend tidak start
```bash
# Kill existing processes
pkill -f vite

# Restart frontend
cd FP-PemrogramanWebsite-FE-2025
npm run dev
```

### Database connection error
```bash
# Check PostgreSQL status
sudo systemctl status postgresql

# Restart if needed
sudo systemctl restart postgresql

# Grant permissions
sudo -u postgres psql -d wordit_db -c "GRANT ALL ON SCHEMA public TO admin;"
```

### CORS Error
Backend sudah dikonfigurasi dengan `origin: '*'`, jadi CORS seharusnya tidak masalah.

### API returns 404
Pastikan routes sudah terdaftar dengan benar:
```bash
# Check game controller
cat FP-PemrogramanWebsite-BE-2025/src/api/game/game.controller.ts | grep compound-sentences
```

---

## 📊 MONITORING

### Check Backend Logs
```bash
cd FP-PemrogramanWebsite-BE-2025
tail -f backend.log
```

### Check Frontend Logs  
```bash
cd FP-PemrogramanWebsite-FE-2025
tail -f frontend.log
```

### Check Running Processes
```bash
# Backend
lsof -i :3000

# Frontend
lsof -i :3003

# Database
lsof -i :5432
```

---

## 📝 FILE STRUCTURE

```
FP-Pemweb-Compound-Sentences/
├── FP-PemrogramanWebsite-BE-2025/
│   ├── src/api/game/
│   │   ├── game.controller.ts (MODIFIED - added router)
│   │   └── compound-sentences/
│   │       ├── compound-sentences.controller.ts (NEW)
│   │       ├── compound-sentences.service.ts (NEW)
│   │       └── schema/index.ts (NEW)
│   ├── prisma/
│   │   ├── schema.prisma
│   │   ├── manual-seed.sql (NEW - for manual seeding)
│   │   └── seeder/
│   │       ├── data/
│   │       │   ├── game-templates.data.csv (MODIFIED)
│   │       │   ├── compound-sentences-games.data.csv (NEW)
│   │       │   └── compound-sentences-pairs.data.csv (NEW)
│   │       └── seed/
│   │           ├── compound-sentences.seed.ts (NEW)
│   │           └── index.ts (MODIFIED)
│   └── .env.development
│
├── FP-PemrogramanWebsite-FE-2025/
│   ├── src/
│   │   ├── pages/
│   │   │   └── CompoundSentences.tsx (NEW)
│   │   └── App.tsx (MODIFIED - added route)
│   └── .env (NEW)
│
├── APIDOG_DOCUMENTATION.md (NEW)
└── HOW_TO_RUN.md (THIS FILE)
```

---

## 🎨 GAME FEATURES

✅ **Database Integration** - PostgreSQL dengan Prisma  
✅ **Game Templates System** - Extensible game template architecture  
✅ **Dynamic Loading** - Games loaded from database  
✅ **Answer Validation** - Server-side validation dengan detail feedback  
✅ **Score Calculation** - Automatic scoring (0-100%)  
✅ **Play Count Tracking** - Auto-increment setiap game dimainkan  
✅ **Kids-Friendly UI** - Warna cerah, rounded borders, big buttons  
✅ **RESTful API** - Clean REST architecture  
✅ **CORS Enabled** - Frontend-backend communication ready  

---

## 🎯 QUICK ACCESS

- **Frontend Game:** http://localhost:3003/compound-sentences
- **API Endpoint:** http://localhost:3000/api/game/compound-sentences
- **Health Check:** http://localhost:3000/health

---

**Last Updated:** December 11, 2025  
**Backend:** ✅ Running on Port 3000  
**Frontend:** ✅ Running on Port 3003  
**Database:** ✅ PostgreSQL Connected  
**Status:** 🟢 FULLY OPERATIONAL
