# Compound Sentences Game API - Apidog Collection

## 🎮 API Endpoints untuk Testing di Apidog

### Base URL
```
http://localhost:3000
```

---

## 📋 **1. Get All Compound Sentences Games**

**Endpoint:** `GET /api/game/compound-sentences`

**Description:** Mendapatkan daftar semua game Compound Sentences yang sudah dipublish

**Request:**
```
GET http://localhost:3000/api/game/compound-sentences
```

**Headers:**
```
Content-Type: application/json
```

**Response (200 OK):**
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

---

## 🎯 **2. Get Game Data (Start Game)**

**Endpoint:** `GET /api/game/compound-sentences/:gameId`

**Description:** Mendapatkan data game specific dengan sentence parts yang sudah di-shuffle

**Request:**
```
GET http://localhost:3000/api/game/compound-sentences/cs-game-001
```

**Headers:**
```
Content-Type: application/json
```

**Response (200 OK):**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Game data fetched successfully",
  "data": {
    "gameId": "cs-game-001",
    "gameName": "Basic Compound Sentences",
    "gameDescription": "Learn to match compound sentences for beginners",
    "firstParts": [
      {
        "id": "first-pair-1",
        "text": "I wanted to play outside",
        "pairId": "pair-1"
      },
      {
        "id": "first-pair-2",
        "text": "She studied hard",
        "pairId": "pair-2"
      }
      // ... 8 total pairs
    ],
    "secondParts": [
      {
        "id": "second-pair-3",
        "text": "he finished his homework",
        "pairId": "pair-3"
      },
      {
        "id": "second-pair-1",
        "text": "it was raining",
        "pairId": "pair-1"
      }
      // ... shuffled order
    ],
    "pairs": [
      {
        "id": "pair-1",
        "firstPart": "I wanted to play outside",
        "secondPart": "it was raining",
        "conjunction": "but"
      }
      // ... all pairs
    ]
  }
}
```

---

## ✅ **3. Validate Answers (Submit Game)**

**Endpoint:** `POST /api/game/compound-sentences/validate`

**Description:** Validasi jawaban player dan return score dengan feedback detail

**Request:**
```
POST http://localhost:3000/api/game/compound-sentences/validate
```

**Headers:**
```
Content-Type: application/json
```

**Body (JSON):**
```json
{
  "gameId": "cs-game-001",
  "answers": [
    {
      "firstPartId": "first-pair-1",
      "secondPartId": "second-pair-1"
    },
    {
      "firstPartId": "first-pair-2",
      "secondPartId": "second-pair-2"
    },
    {
      "firstPartId": "first-pair-3",
      "secondPartId": "second-pair-3"
    },
    {
      "firstPartId": "first-pair-4",
      "secondPartId": "second-pair-4"
    },
    {
      "firstPartId": "first-pair-5",
      "secondPartId": "second-pair-5"
    },
    {
      "firstPartId": "first-pair-6",
      "secondPartId": "second-pair-6"
    },
    {
      "firstPartId": "first-pair-7",
      "secondPartId": "second-pair-7"
    },
    {
      "firstPartId": "first-pair-8",
      "secondPartId": "second-pair-8"
    }
  ]
}
```

**Response (200 OK) - All Correct:**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Answers validated successfully",
  "data": {
    "score": 100,
    "totalQuestions": 8,
    "correctAnswers": 8,
    "results": [
      {
        "firstPartId": "first-pair-1",
        "secondPartId": "second-pair-1",
        "isCorrect": true,
        "correctPair": {
          "firstPart": "I wanted to play outside",
          "secondPart": "it was raining"
        }
      }
      // ... all results
    ]
  }
}
```

**Response (200 OK) - Some Wrong:**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Answers validated successfully",
  "data": {
    "score": 75,
    "totalQuestions": 8,
    "correctAnswers": 6,
    "results": [
      {
        "firstPartId": "first-pair-1",
        "secondPartId": "second-pair-2",
        "isCorrect": false,
        "correctPair": {
          "firstPart": "I wanted to play outside",
          "secondPart": "it was raining"
        }
      }
      // ... other results
    ]
  }
}
```

**Error Response (400 Bad Request):**
```json
{
  "success": false,
  "statusCode": 400,
  "message": "Validation error",
  "errors": [
    {
      "field": "gameId",
      "message": "Game ID is required"
    }
  ]
}
```

**Error Response (404 Not Found):**
```json
{
  "success": false,
  "statusCode": 404,
  "message": "Game not found"
}
```

---

## 🔧 **Cara Import ke Apidog:**

### Method 1: Manual Creation
1. Buka Apidog
2. Create New Project: "Compound Sentences Game API"
3. Untuk setiap endpoint:
   - Klik "New Request"
   - Set Method (GET/POST)
   - Masukkan URL
   - Tambahkan Headers jika perlu
   - Untuk POST, masukkan Body (JSON)
   - Save Request

### Method 2: Import dari File (OpenAPI/Swagger)
1. Save file ini sebagai reference
2. Manually create requests berdasarkan spec di atas

---

## 🧪 **Testing Scenarios di Apidog:**

### Scenario 1: Complete Game Flow
1. **Get All Games** → Dapatkan list games
2. **Get Game Data** → Load game dengan ID dari step 1
3. **Submit Answers** → Submit jawaban (semua benar)
4. **Verify Score** → Check score = 100%

### Scenario 2: Wrong Answers
1. **Get Game Data** → cs-game-001
2. **Submit Wrong Answers** → Mix up the second parts
3. **Check Feedback** → Verify detail feedback untuk setiap wrong answer

### Scenario 3: Validation Errors
1. **Submit Without gameId** → Should return 400 error
2. **Submit Empty Answers** → Should return 400 error
3. **Submit Invalid Game ID** → Should return 404 error

---

## 📊 **Database Info:**

**Container Name:** `fp-database` (PostgreSQL - System)  
**Database:** `wordit_db`  
**User:** `admin`  
**Password:** `password123`  
**Port:** `5432` (localhost)

### Direct PostgreSQL Connection:
```bash
PGPASSWORD=password123 psql -h localhost -U admin -d wordit_db
```

### Check Data:
```sql
-- Check games
SELECT id, name, is_published FROM "Games";

-- Check game templates
SELECT id, name, slug FROM "GameTemplates";

-- Check game JSON data
SELECT name, game_json FROM "Games" WHERE id = 'cs-game-001';
```

---

## 🚀 **Quick Start Commands:**

### Start Backend:
```bash
cd FP-PemrogramanWebsite-BE-2025
bun run start:dev
```

### Start Frontend:
```bash
cd FP-PemrogramanWebsite-FE-2025
npm run dev
```

### Test API dengan curl:
```bash
# Get all games
curl http://localhost:3000/api/game/compound-sentences

# Get specific game
curl http://localhost:3000/api/game/compound-sentences/cs-game-001

# Submit answers
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

## 🎨 **Color Theme (Frontend):**
- **#E2852E** - Orange (Primary)
- **#F5C857** - Yellow (Secondary)
- **#FFEE91** - Light Yellow (Background)
- **#ABE0F0** - Light Blue (Cards/Instructions)

---

## ✨ **Features:**
✅ PostgreSQL Database Integration  
✅ Game Templates System  
✅ Dynamic Game Loading  
✅ Answer Validation with Detailed Feedback  
✅ Score Calculation  
✅ Play Count Tracking  
✅ Kids-Friendly UI  
✅ RESTful API Design  

---

**Last Updated:** December 11, 2025  
**Backend Status:** ✅ Running on Port 3000  
**Database:** ✅ PostgreSQL Connected  
**Frontend:** Ready to Start
