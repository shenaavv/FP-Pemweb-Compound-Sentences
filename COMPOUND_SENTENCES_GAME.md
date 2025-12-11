# Compound Sentences Game - Integration Guide

## 📝 Overview
A fun, kid-friendly mini-game that helps children learn compound sentences by matching sentence parts together. The game features a colorful UI with immediate feedback and scoring.

## 🎨 Color Theme
The game uses a consistent, child-friendly color palette:
- **Orange** (`#E2852E`) - Primary accent, headers, icons
- **Yellow** (`#F5C857`) - Column headers, matched pairs
- **Light Yellow** (`#FFEE91`) - Background
- **Light Blue** (`#ABE0F0`) - Instructions card, results screen

## 📂 Files Created

### Backend Files

#### 1. Service Layer
**Location:** `FP-PemrogramanWebsite-BE-2025/src/api/game/compound-sentences/compound-sentences.service.ts`

Contains:
- 8 compound sentence pairs for kids
- Game data generation with shuffled sentences
- Answer validation logic
- Score calculation

Key Methods:
- `getGameData()` - Returns shuffled sentence parts
- `validateAnswers(request)` - Validates player answers and returns detailed results

#### 2. Controller
**Location:** `FP-PemrogramanWebsite-BE-2025/src/api/game/compound-sentences/compound-sentences.controller.ts`

API Endpoints:
- **GET** `/api/game/compound-sentences` - Fetch game data
- **POST** `/api/game/compound-sentences/validate` - Submit answers for validation

#### 3. Schema Validation
**Location:** `FP-PemrogramanWebsite-BE-2025/src/api/game/compound-sentences/schema/index.ts`

Zod schema for validating answer submissions.

#### 4. Router Integration
**Modified:** `FP-PemrogramanWebsite-BE-2025/src/api/game/game.controller.ts`

Added compound-sentences router to the main game controller.

### Frontend Files

#### 1. Main Game Page
**Location:** `FP-PemrogramanWebsite-FE-2025/src/pages/CompoundSentences.tsx`

Features:
- Three-column layout (First parts, Matches, Second parts)
- Interactive sentence matching
- Visual feedback for selections
- Match removal capability
- Results screen with detailed feedback
- Play again and home navigation

#### 2. Route Integration
**Modified:** `FP-PemrogramanWebsite-FE-2025/src/App.tsx`

Added route: `/compound-sentences`

## 🚀 How to Use

### Starting the Backend

```bash
cd FP-PemrogramanWebsite-BE-2025

# Development mode
bun run start:dev

# Production mode
bun run start:deploy:prod
```

The backend will be available at the configured port (check your `.env` file).

### Starting the Frontend

```bash
cd FP-PemrogramanWebsite-FE-2025

# Development mode
npm run dev
# or
bun dev

# Build for production
npm run build
# or
bun run build
```

### Accessing the Game

Once both servers are running:
1. Navigate to `http://localhost:5173/compound-sentences` (or your configured frontend URL)
2. The game will automatically load sentence data from the backend
3. Start matching sentences!

## 🎮 Game Flow

### 1. Game Start
- Game fetches 8 compound sentence pairs from backend
- First parts are displayed in order
- Second parts are shuffled for challenge

### 2. Playing
1. Click a sentence from the "First Part" column (left)
2. It highlights with yellow background
3. Click its matching part from "Second Part" column (right)
4. Match appears in the center "Your Matches" column
5. Matched items become grayed out
6. Repeat until all sentences are matched

### 3. Removing Matches
- Hover over a match in the center column
- Click the **✕** button that appears
- The match is removed and parts become available again

### 4. Submitting
- When all 8 sentences are matched, click "Submit Answers"
- Backend validates all answers
- Results screen appears with:
  - Total score (percentage)
  - Number of correct answers
  - Detailed feedback for each answer
  - Correct answers for incorrect matches

### 5. Results Screen
- Shows score and correct/incorrect indicators
- Green cards for correct answers (✅)
- Red cards for incorrect answers (❌) with correct answer shown
- Options to "Play Again" or return "Home"

## 🔌 API Reference

### GET /api/game/compound-sentences

Fetch game data with shuffled sentences.

**Response:**
```json
{
  "statusCode": 200,
  "message": "Game data fetched successfully",
  "data": {
    "firstParts": [
      {
        "id": "first-1",
        "text": "I wanted to play outside",
        "pairId": "1"
      },
      // ... more first parts
    ],
    "secondParts": [
      {
        "id": "second-3",
        "text": "yet he finished his homework",
        "pairId": "3"
      },
      // ... more second parts (shuffled)
    ],
    "pairs": [
      {
        "id": "1",
        "firstPart": "I wanted to play outside",
        "secondPart": "but it was raining"
      },
      // ... more pairs
    ]
  }
}
```

### POST /api/game/compound-sentences/validate

Submit player answers for validation.

**Request Body:**
```json
{
  "answers": [
    {
      "firstPartId": "first-1",
      "secondPartId": "second-1"
    },
    // ... more answers
  ]
}
```

**Response:**
```json
{
  "statusCode": 200,
  "message": "Answers validated successfully",
  "data": {
    "score": 87,
    "totalQuestions": 8,
    "correctAnswers": 7,
    "results": [
      {
        "firstPartId": "first-1",
        "secondPartId": "second-1",
        "isCorrect": true
      },
      {
        "firstPartId": "first-2",
        "secondPartId": "second-3",
        "isCorrect": false,
        "correctPair": {
          "firstPart": "She studied hard",
          "secondPart": "so she passed the test"
        }
      },
      // ... more results
    ]
  }
}
```

## 🎯 Sentence Data

The game includes 8 compound sentence pairs:

1. I wanted to play outside **+** but it was raining
2. She studied hard **+** so she passed the test
3. He was tired **+** yet he finished his homework
4. We can go to the park **+** or we can visit the museum
5. The dog barked loudly **+** and the cat ran away
6. I love ice cream **+** but my brother prefers cake
7. She woke up early **+** so she could catch the bus
8. The movie was long **+** yet it was very interesting

### Adding More Sentences

To add more sentences, edit:
`FP-PemrogramanWebsite-BE-2025/src/api/game/compound-sentences/compound-sentences.service.ts`

Add new pairs to the `SENTENCE_PAIRS` array:

```typescript
{
  id: '9',
  firstPart: 'Your first part',
  secondPart: 'your second part',
},
```

## 🎨 UI Components Used

- **Card** - Container components
- **Button** - Interactive buttons
- **Typography** - Text styling (h1, h2, h3, p, large, small)
- **Lucide Icons** - Sparkles, Trophy, RotateCcw, Home

## 📱 Responsive Design

The game is responsive:
- **Desktop:** Three-column layout
- **Mobile/Tablet:** Single column, stacked layout (via `grid-cols-1 lg:grid-cols-3`)

## 🔧 Customization

### Changing Colors

Update inline styles and className colors in:
`FP-PemrogramanWebsite-FE-2025/src/pages/CompoundSentences.tsx`

Search for:
- `#E2852E` - Orange accent
- `#F5C857` - Yellow
- `#FFEE91` - Light yellow background
- `#ABE0F0` - Light blue

### Modifying Game Logic

Backend logic is in:
`FP-PemrogramanWebsite-BE-2025/src/api/game/compound-sentences/compound-sentences.service.ts`

- Change scoring: Modify `validateAnswers()` method
- Add time limits: Add timestamp validation
- Difficulty levels: Create multiple sentence sets

## 🐛 Troubleshooting

### Game doesn't load
- Check if backend is running
- Verify `VITE_API_URL` in frontend `.env` file
- Check browser console for API errors

### TypeScript errors
- Run `npm install` or `bun install` in both directories
- Ensure all dependencies are installed

### Styling issues
- Verify Tailwind CSS is configured
- Check that color values are valid
- Clear browser cache

## ✅ Testing Checklist

- [ ] Backend API returns game data
- [ ] Sentences are shuffled on each load
- [ ] Clicking first part highlights it
- [ ] Clicking second part creates match
- [ ] Matched items are grayed out
- [ ] Remove match button works
- [ ] Submit button only active when all matched
- [ ] Validation returns correct score
- [ ] Results screen shows all answers
- [ ] Play Again resets game
- [ ] Home button navigates back
- [ ] Responsive on mobile devices

## 📊 Game Stats

- **Total Sentences:** 8 pairs
- **Difficulty:** Beginner (suitable for elementary school)
- **Average Play Time:** 2-5 minutes
- **Scoring:** Percentage based (0-100%)

## 🚀 Future Enhancements

Possible improvements:
- Add timer for speed challenges
- Multiple difficulty levels
- Leaderboard integration
- Sound effects and animations
- More sentence categories
- Hints system
- Progress tracking

## 📞 Support

If you encounter issues:
1. Check console logs (F12 in browser)
2. Verify all files are in correct locations
3. Ensure both servers are running
4. Check network tab for API call failures

---

**Game Created:** December 2025  
**Reference:** https://wordwall.net/resource/3595053/english/compound-sentences
