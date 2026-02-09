# Quick Visual: How It Gets Questions & Answers

## In 3 Simple Diagrams

### 1️⃣ Getting Questions (From the Webpage)

```
┌─────────────────────────────────────────────────┐
│         COURSERA QUIZ WEBPAGE (HTML)            │
│                                                 │
│  <div class="quiz-question">                   │
│    <p class="question-text">                   │
│      What is 2 + 2?                            │
│    </p>                                        │
│    <div class="options">                       │
│      <input type="radio" value="0"> 3          │
│      <input type="radio" value="1"> 4          │
│      <input type="radio" value="2"> 5          │
│    </div>                                      │
│  </div>                                        │
└────────────────┬────────────────────────────────┘
                 │
                 │ Extension reads this HTML
                 │ using document.querySelector()
                 │
                 ▼
┌─────────────────────────────────────────────────┐
│         EXTRACTED QUESTION DATA                 │
│                                                 │
│  {                                              │
│    id: "q123",                                 │
│    text: "What is 2 + 2?",                    │
│    options: ["3", "4", "5"],                  │
│    type: "radio"                               │
│  }                                              │
└─────────────────────────────────────────────────┘
```

**HOW:** The extension scans the webpage HTML and extracts:
- Question text
- Answer options
- Question type
- Question ID

---

### 2️⃣ Getting Answers (From Cache or AI)

```
┌─────────────────────────────────────────────────┐
│  QUESTION: "What is 2 + 2?"                    │
│  OPTIONS: ["3", "4", "5"]                      │
└────────────────┬────────────────────────────────┘
                 │
                 │ Need answer!
                 │
                 ▼
         ╔═══════════════════╗
         ║ CHECK CACHE FIRST ║
         ╚═══════╤═══════════╝
                 │
        ┌────────┴────────┐
        │                 │
        ▼                 ▼
   ┌────────┐        ┌────────┐
   │ Found! │        │ Not    │
   │ Use it │        │ found  │
   └───┬────┘        └───┬────┘
       │                 │
       │                 ▼
       │         ┌──────────────────┐
       │         │ CALL AI SERVICE  │
       │         │ (External API)   │
       │         └────────┬─────────┘
       │                  │
       │                  ▼
       │         ┌──────────────────┐
       │         │ AI Response:     │
       │         │ Answer = 1       │
       │         │ (Option "4")     │
       │         └────────┬─────────┘
       │                  │
       │                  │ Save to cache
       │                  │
       └──────────────────┴──────────┐
                                     │
                                     ▼
                            ┌─────────────────┐
                            │ ANSWER: Index 1 │
                            │ (Option "4")    │
                            └─────────────────┘
```

**WHERE:** Answers come from:
1. 🔹 Cache (if you've done this quiz before)
2. 🔹 AI Service (external API call) - **paid feature**

---

### 3️⃣ Complete Process (Start to Finish)

```
┌──────────────────┐
│  User Opens Quiz │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Extension Loads │
│  Wait 10 seconds │
└────────┬─────────┘
         │
         ▼
┌────────────────────────────────────┐
│  PART 1: GET QUESTIONS             │
│  ─────────────────────             │
│  • Scan webpage HTML               │
│  • Find quiz elements              │
│  • Extract question text           │
│  • Extract options                 │
│  • Identify question type          │
│                                    │
│  Result: 5 questions found         │
└────────┬───────────────────────────┘
         │
         ▼
┌────────────────────────────────────┐
│  PART 2: GET ANSWERS               │
│  ────────────────────              │
│  For each question:                │
│                                    │
│  ① Check cache                     │
│     └─→ Found? → Use it! ✓        │
│                                    │
│  ② Not in cache?                   │
│     └─→ Call AI service            │
│         └─→ Get answer             │
│             └─→ Save to cache      │
│                                    │
│  Result: 5 answers retrieved       │
└────────┬───────────────────────────┘
         │
         ▼
┌────────────────────────────────────┐
│  PART 3: SELECT ANSWERS            │
│  ───────────────────               │
│  For each question:                │
│                                    │
│  • Find correct option element     │
│  • Click it                        │
│  • Highlight it green              │
│                                    │
│  Result: All answers selected      │
└────────┬───────────────────────────┘
         │
         ▼
┌──────────────────┐
│  User Submits    │
│  Quiz Complete!  │
└──────────────────┘
```

---

## Summary in One Sentence

> The extension **reads questions from the webpage HTML** and **gets answers from an AI service**, then automatically **clicks the correct options**.

---

## Two Main Parts

### 📄 Questions: From the Webpage
- **Source:** Coursera's HTML
- **Method:** DOM queries (`querySelector`)
- **What:** Text, options, type, ID

### 💡 Answers: From AI or Cache
- **Source 1:** Cache (fast, free)
- **Source 2:** AI API (slower, paid)
- **Method:** HTTP request to external service

---

## Want More Details?

📖 **Full Explanation:** [HOW_IT_GETS_QA.md](HOW_IT_GETS_QA.md) (18 KB)
- Step-by-step breakdown
- Code examples
- All question types
- Real examples

📚 **All Documentation:** [QUIZ_DOCUMENTATION_INDEX.md](QUIZ_DOCUMENTATION_INDEX.md)
- Complete guide
- 6 documents
- 70+ KB total

---

## The Code

The actual code is in `dist/scripts/content.js` but it's heavily obfuscated.

**To see it in action:**
1. Load extension in Chrome
2. Open a Coursera quiz
3. Press F12 → Sources → Content Scripts
4. Set breakpoints and debug

See [DEBUGGING_GUIDE.md](DEBUGGING_GUIDE.md) for instructions.

---

Made with ❤️ to help you understand the extension!
