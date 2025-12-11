# 🎮 Compound Sentences Game - Complete Setup Guide

## ✅ What Was Done

### Backend Integration
1. **Database Schema** (Prisma/PostgreSQL)
   - Added `compound-sentences` to game templates
   - Game data stored in `Games` table with JSON structure
   
2. **API Endpoints Created**
   - `GET /api/game/compound-sentences` - List all published games
   - `GET /api/game/compound-sentences/:gameId` - Get game data
   - `POST /api/game/compound-sentences/validate` - Submit answers & get score

3. **Files Created/Modified**
   ```
   Backend:
   ├── prisma/seeder/data/
   │   ├── game-templates.data.csv (MODIFIED - added compound-sentences)
   │   ├── compound-sentences-games.data.csv (NEW)
   │   └── compound-sentences-pairs.data.csv (NEW)
   ├── prisma/seeder/seed/
   │   ├── compound-sentences.seed.ts (NEW)
   │   └── index.ts (MODIFIED - exported seeder)
   ├── prisma/seeder/seeder.ts (MODIFIED - added seeder call)
   └── src/api/game/compound-sentences/
       ├── compound-sentences.service.ts (MODIFIED - DB integration)
       ├── compound-sentences.controller.ts (MODIFIED - added endpoints)
       └── schema/index.ts (MODIFIED - added gameId validation)
   ```

4. **Frontend Integration**
   - Updated to work with database-backed API
   - Added game selector UI
   - Dynamic game loading
   
5. **Color Theme Applied**
   - #E2852E (Orange) - Headers, icons
   - #F5C857 (Yellow) - Buttons, highlights  
   - #FFEE91 (Light Yellow) - Background
   - #ABE0F0 (Light Blue) - Cards, instructions

---

## 🚀 How to Run

### Prerequisites
- Bun installed (Backend runtime)
- Node.js & npm (Frontend)
- PostgreSQL (via Docker)

### Step 1: Backend Setup

#### 1.1 Create Environment File
```bash
cd FP-PemrogramanWebsite-BE-2025

# Create .env.development file
cat > .env.development << 'EOF'
NODE_ENV=development
PORT=3000
DATABASE_URL=postgresql://admin:password123@localhost:5432/wordit_db

# PostgreSQL Docker config
POSTGRES_NAME=wordit_db
POSTGRES_USER=admin
POSTGRES_PASSWORD=password123
POSTGRES_PORT=5432

# JWT Secret (change in production!)
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
EOF
```

#### 1.2 Start PostgreSQL Database
```bash
# Start Docker container
bun run docker:up:dev

# Wait 5 seconds for database to initialize
sleep 5
```

#### 1.3 Run Migrations & Seed Data
```bash
# Generate Prisma Client
bun run generate

# Run migrations to create tables
bun run migrate:dev

# Seed all data (including compound sentences)
bun run seed:dev
```

#### 1.4 Start Backend Server
```bash
bun run start:dev
```

Backend will run at: **http://localhost:3000**

---

### Step 2: Frontend Setup

#### 2.1 Create Environment File
```bash
cd ../FP-PemrogramanWebsite-FE-2025

# Create .env file
cat > .env << 'EOF'
VITE_API_URL=http://localhost:3000/api
EOF
```

#### 2.2 Install Dependencies & Start
```bash
# Install dependencies (if not done)
npm install

# Start development server
npm run dev
```

Frontend will run at: **http://localhost:5173**

---

### Step 3: Play the Game! 🎯

1. Open browser: **http://localhost:5173/compound-sentences**
2. Select a game (if multiple available)
3. Click on first part of sentence
4. Click on matching second part
5. Repeat until all matched
6. Click "Submit Answers"
7. See your score and feedback!

---

## 🔧 Troubleshooting

### Database Connection Issues
```bash
# Check if Docker container is running
docker ps | grep fp-database

# If not running, start it
cd FP-PemrogramanWebsite-BE-2025
bun run docker:up:dev

# Check database logs
docker logs fp-database
```

### Backend Not Starting
```bash
# Check if port 3000 is already in use
lsof -i :3000

# Kill process if needed
kill -9 <PID>

# Reinstall dependencies
bun install
bun run start:dev
```

### Frontend API Errors
```bash
# Verify backend is running
curl http://localhost:3000/api/game/compound-sentences

# Check .env file exists and has correct API URL
cat .env

# Clear cache and restart
rm -rf node_modules/.vite
npm run dev
```

### No Games Showing
```bash
# Re-run seeder
cd FP-PemrogramanWebsite-BE-2025
bun run seed:dev

# Check database directly
docker exec -it fp-database psql -U admin -d wordit_db -c "SELECT * FROM \"Games\" WHERE game_template_id = 'f1a2b3c4-d5e6-7f8g-9h0i-1j2k3l4m5n6o';"
```

---

## 📊 API Testing

### Test Endpoints with curl

```bash
# 1. List all compound sentences games
curl http://localhost:3000/api/game/compound-sentences

# 2. Get specific game data
curl http://localhost:3000/api/game/compound-sentences/cs-game-001

# 3. Validate answers
curl -X POST http://localhost:3000/api/game/compound-sentences/validate \
  -H "Content-Type: application/json" \
  -d '{
    "gameId": "cs-game-001",
    "answers": [
      {"firstPartId": "first-pair-1", "secondPartId": "second-pair-1"},
      {"firstPartId": "first-pair-2", "secondPartId": "second-pair-2"}
    ]
  }'
```

---

## 🎨 Adding More Games

### Create a New Compound Sentences Game

1. **Add to CSV file**:
   Edit `prisma/seeder/data/compound-sentences-games.data.csv`:
   ```csv
   cs-game-002,f1a2b3c4-d5e6-7f8g-9h0i-1j2k3l4m5n6o,user-admin-001,"Advanced Compound Sentences","For advanced learners",compound-thumb-2.png,true
   ```

2. **Add sentence pairs**:
   Edit `prisma/seeder/data/compound-sentences-pairs.data.csv`:
   ```csv
   cs-game-002,"I went to the store","I forgot my wallet","but"
   cs-game-002,"It was raining heavily","we stayed indoors","so"
   ```

3. **Re-seed database**:
   ```bash
   cd FP-PemrogramanWebsite-BE-2025
   bun run seed:dev
   ```

4. **Test in browser** - New game will appear in selector!

---

## 🏗️ Project Structure

```
FP-Pemweb-Compound-Sentences/
├── FP-PemrogramanWebsite-BE-2025/          # Backend (Bun + Prisma + PostgreSQL)
│   ├── src/api/game/compound-sentences/    # Game API endpoints
│   ├── prisma/
│   │   ├── schema.prisma                   # Database schema
│   │   └── seeder/                         # Seed data
│   └── docker-compose.yml                  # PostgreSQL container
│
└── FP-PemrogramanWebsite-FE-2025/          # Frontend (React + Vite)
    ├── src/pages/CompoundSentences.tsx     # Game page
    └── src/App.tsx                         # Routes
```

---

## 🎯 Game Features

✅ **Database-Driven**: All games stored in PostgreSQL  
✅ **Multiple Games**: Support for multiple compound sentence sets  
✅ **Real-time Validation**: Instant feedback on answers  
✅ **Score Tracking**: Automatic play count increment  
✅ **Shuffle Algorithm**: Second parts randomized each game  
✅ **Kid-Friendly UI**: Rounded corners, big buttons, fun colors  
✅ **Responsive Design**: Works on all screen sizes  
✅ **RESTful API**: Clean, standard endpoints  

---

## 📝 Database Schema

### GameTemplates Table
```sql
id: f1a2b3c4-d5e6-7f8g-9h0i-1j2k3l4m5n6o
slug: compound-sentences
name: Compound Sentences
description: Match sentence parts to create complete compound sentences
```

### Games Table (JSON Structure)
```json
{
  "pairs": [
    {
      "id": "pair-1",
      "first_part": "I wanted to play outside",
      "second_part": "it was raining",
      "conjunction": "but"
    }
  ]
}
```

---

## 🔐 Security Notes

- Change `JWT_SECRET` in production
- Use strong database passwords
- Never commit `.env` files
- API validates all inputs with Zod schemas

---

## 📈 Performance

- Game data cached in memory during session
- Database queries optimized with Prisma
- Frontend uses React state management
- Minimal API calls (only on game start/submit)

---

## 🐛 Known Issues

None currently! If you find bugs, check:
1. Backend logs: Terminal where `bun run start:dev` is running
2. Frontend logs: Browser DevTools Console
3. Database: Check Docker logs or query directly

---

## 🎓 Learning Resources

- **Prisma Docs**: https://www.prisma.io/docs
- **Bun Runtime**: https://bun.sh/docs
- **React + TypeScript**: https://react-typescript-cheatsheet.netlify.app/

---

## 👥 Support

If you encounter issues:
1. Check this guide's Troubleshooting section
2. Verify all environment variables are set
3. Ensure Docker is running (for database)
4. Check that all ports are available (3000 for BE, 5173 for FE, 5432 for DB)

---

**✨ Happy Gaming! ✨**
