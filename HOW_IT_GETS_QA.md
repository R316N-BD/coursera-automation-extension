# How Does It Get Questions and Answers?

## Simple Answer

The extension **reads questions from the webpage** and **gets answers from an AI service**.

Here's the exact process:

---

## Part 1: Getting Questions from the Page

### Step-by-Step Process

```
1. Extension detects you're on a Coursera quiz page
   ↓
2. Waits 10 seconds for page to fully load
   ↓
3. Scans the webpage HTML for quiz elements
   ↓
4. Extracts question information
```

### How It Reads Questions

The extension uses **DOM selectors** to find quiz elements on the page:

```javascript
// The extension looks for quiz questions like this:
const questions = document.querySelectorAll('.quiz-question');

// For each question found, it extracts:
questions.forEach(question => {
  
  // 1. QUESTION TEXT
  const questionText = question.querySelector('.question-text').innerText;
  // Example: "What is 2 + 2?"
  
  // 2. ANSWER OPTIONS
  const options = question.querySelectorAll('.option');
  // Example: ["3", "4", "5", "6"]
  
  // 3. QUESTION TYPE
  const hasRadio = question.querySelector('input[type="radio"]');
  const hasCheckbox = question.querySelector('input[type="checkbox"]');
  // Types: multiple-choice, checkbox, text, dropdown
  
  // 4. QUESTION ID
  const questionId = question.getAttribute('data-question-id');
  // Example: "abc123"
  
});
```

### What Information Is Extracted

For each question, the extension gets:

| Data | Example | How It's Found |
|------|---------|----------------|
| **Question Text** | "What is the capital of France?" | `.question-text` or `.prompt` element |
| **Option A** | "London" | First `.option` element text |
| **Option B** | "Paris" | Second `.option` element text |
| **Option C** | "Berlin" | Third `.option` element text |
| **Question Type** | "radio" (single choice) | `input[type="radio"]` presence |
| **Question ID** | "q_abc123" | `data-question-id` attribute |

### Visual Example

**What Coursera's HTML looks like:**

```html
<div class="quiz-question" data-question-id="q123">
  <div class="question-text">
    <p>What is 2 + 2?</p>
  </div>
  
  <div class="options">
    <div class="option">
      <input type="radio" name="q1" value="3" />
      <label>3</label>
    </div>
    <div class="option">
      <input type="radio" name="q1" value="4" />
      <label>4</label>
    </div>
    <div class="option">
      <input type="radio" name="q1" value="5" />
      <label>5</label>
    </div>
  </div>
</div>
```

**What the extension extracts:**

```javascript
{
  id: "q123",
  text: "What is 2 + 2?",
  type: "multiple-choice",
  options: [
    { index: 0, text: "3", value: "3" },
    { index: 1, text: "4", value: "4" },
    { index: 2, text: "5", value: "5" }
  ]
}
```

---

## Part 2: Getting Answers

### The 3 Answer Sources

The extension gets answers from **3 possible sources** (in order):

```
┌─────────────────────────────────────┐
│  1. CACHED ANSWERS (Local Storage)  │
│     ↓ If not found...               │
├─────────────────────────────────────┤
│  2. PREVIOUS FEEDBACK (Memory)      │
│     ↓ If not found...               │
├─────────────────────────────────────┤
│  3. AI SERVICE (External API)       │
│     ✓ Gets fresh answer             │
└─────────────────────────────────────┘
```

### Source 1: Cached Answers

If you've taken this quiz before, the extension checks its cache:

```javascript
// Check local storage for previously correct answers
chrome.storage.local.get(['quizAnswers'], (result) => {
  const cached = result.quizAnswers[questionId];
  
  if (cached && cached.correct === true) {
    // Use this answer!
    return cached.answerIndex; // Example: 1 (which means option B)
  }
});
```

**Example stored data:**
```json
{
  "q123": {
    "questionText": "What is 2 + 2?",
    "answerIndex": 1,
    "answerText": "4",
    "correct": true,
    "timestamp": 1234567890
  }
}
```

### Source 2: Previous Feedback

If you got the answer wrong before, the extension remembers:

```javascript
// Check what answers were WRONG in previous attempts
const feedback = cached.previousAttempts;

if (feedback) {
  // Eliminate wrong answers
  const wrongAnswers = feedback.incorrect; // [0, 2] (options A and C were wrong)
  const validOptions = options.filter(opt => !wrongAnswers.includes(opt.index));
  
  // Now we know it must be option B!
  // But we might still ask AI to be sure
}
```

**Example feedback data:**
```json
{
  "q123": {
    "previousAttempts": [
      {
        "attemptNumber": 1,
        "selectedAnswer": 0,
        "correct": false,
        "feedback": "Incorrect. Try again."
      }
    ],
    "incorrect": [0, 2]  // Options A and C are wrong
  }
}
```

### Source 3: AI Service (Premium Feature)

If no cached answer exists, the extension calls an **AI service**:

```javascript
// Call external AI API to get answer
fetch('https://api.example.com/quiz-solver', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_API_KEY'
  },
  body: JSON.stringify({
    question: "What is 2 + 2?",
    options: ["3", "4", "5"],
    courseId: "ml-course-2024",
    subject: "mathematics"
  })
})
.then(response => response.json())
.then(data => {
  // AI responds with:
  // {
  //   "correctAnswer": 1,  // Index of correct option (B)
  //   "confidence": 0.99,
  //   "explanation": "2 + 2 equals 4"
  // }
  
  const answerIndex = data.correctAnswer; // 1
  
  // Cache this for next time
  saveAnswerToCache(questionId, answerIndex);
});
```

### How the AI Knows the Answer

The AI service likely:
1. **Has a database** of known quiz questions and answers
2. **Uses machine learning** to understand and solve questions
3. **Searches the internet** for similar questions
4. **Analyzes course materials** to find answers

**Note:** The exact AI service and method are hidden in the obfuscated code.

---

## Part 3: Complete Flow Diagram

### From Start to Finish

```
╔════════════════════════════════════════════════════════════════╗
║  USER OPENS COURSERA QUIZ                                      ║
╚════════════════════════════════════════════════════════════════╝
                              │
                              ▼
┌────────────────────────────────────────────────────────────────┐
│  EXTENSION DETECTS QUIZ PAGE                                   │
│  • Checks URL: *.coursera.org/quiz/*                          │
│  • Looks for quiz HTML elements                                │
└────────────────┬───────────────────────────────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────────────────────────────┐
│  WAIT 10 SECONDS                                               │
│  • Allows page to fully load                                   │
│  • Ensures all quiz elements are ready                         │
└────────────────┬───────────────────────────────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────────────────────────────┐
│  SCAN PAGE FOR QUESTIONS                                       │
│                                                                 │
│  const questions = document.querySelectorAll('.quiz-question');│
│                                                                 │
│  Found: 5 questions                                            │
└────────────────┬───────────────────────────────────────────────┘
                 │
                 ▼
╔════════════════════════════════════════════════════════════════╗
║  FOR EACH QUESTION:                                            ║
╚════════════════════════════════════════════════════════════════╝
                 │
                 ▼
┌────────────────────────────────────────────────────────────────┐
│  STEP 1: EXTRACT QUESTION DATA                                 │
│  ─────────────────────────────                                 │
│  • Question text: "What is 2 + 2?"                            │
│  • Options: ["3", "4", "5", "6"]                              │
│  • Type: radio (single choice)                                 │
│  • ID: q123                                                    │
└────────────────┬───────────────────────────────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────────────────────────────┐
│  STEP 2: CHECK CACHE                                           │
│  ─────────────────────                                         │
│  chrome.storage.local.get('quizAnswers')                       │
│                                                                 │
│  Cache hit? YES ✓                                              │
│  Answer: Index 1 (option "4")                                  │
│  Correct? YES ✓                                                │
└────────────────┬───────────────────────────────────────────────┘
                 │
                 ▼                    ╔════════════════════════╗
┌────────────────────────────┐        ║ If cache miss:         ║
│  USE CACHED ANSWER         │        ║                        ║
│  Answer index: 1           │        ║ Check previous         ║
└────────────────┬───────────┘        ║ feedback → AI service  ║
                 │                    ╚════════════════════════╝
                 │
                 ▼
┌────────────────────────────────────────────────────────────────┐
│  STEP 3: SELECT THE ANSWER                                     │
│  ─────────────────────────                                     │
│  1. Find the correct option element                            │
│     const option = options[1]; // Option "4"                   │
│                                                                 │
│  2. Find the input inside it                                   │
│     const radio = option.querySelector('input[type="radio"]'); │
│                                                                 │
│  3. Click it!                                                  │
│     radio.click();                                             │
│     radio.dispatchEvent(new Event('change'));                  │
│                                                                 │
│  4. Highlight it                                               │
│     option.style.backgroundColor = 'lightgreen';               │
└────────────────┬───────────────────────────────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────────────────────────────┐
│  REPEAT FOR NEXT QUESTION                                      │
└────────────────┬───────────────────────────────────────────────┘
                 │
                 ▼
╔════════════════════════════════════════════════════════════════╗
║  ALL QUESTIONS ANSWERED!                                       ║
║  User can now submit the quiz                                  ║
╚════════════════════════════════════════════════════════════════╝
                 │
                 ▼
┌────────────────────────────────────────────────────────────────┐
│  AFTER SUBMISSION: COLLECT FEEDBACK                            │
│  ─────────────────────────────────────                         │
│  Wait 2 seconds for results to load...                         │
│                                                                 │
│  Check each answer:                                            │
│  • Question 1: ✓ Correct                                      │
│  • Question 2: ✓ Correct                                      │
│  • Question 3: ✗ Incorrect                                    │
│  • Question 4: ✓ Correct                                      │
│  • Question 5: ✓ Correct                                      │
│                                                                 │
│  Store results in cache for next attempt                       │
└────────────────────────────────────────────────────────────────┘
```

---

## Detailed Example: Real Code Flow

### Example Question

```
Question: "What does HTTP stand for?"

Options:
A) HyperText Transfer Protocol
B) High Transfer Text Protocol  
C) HyperText Transmission Protocol
D) High Text Transfer Protocol
```

### What the Extension Does

**1. Extract Question (from HTML)**

```javascript
// The page HTML contains:
<div class="quiz-question" data-question-id="http_q1">
  <p class="question-text">What does HTTP stand for?</p>
  <div class="options">
    <label><input type="radio" name="http_q1" value="0"> HyperText Transfer Protocol</label>
    <label><input type="radio" name="http_q1" value="1"> High Transfer Text Protocol</label>
    <label><input type="radio" name="http_q1" value="2"> HyperText Transmission Protocol</label>
    <label><input type="radio" name="http_q1" value="3"> High Text Transfer Protocol</label>
  </div>
</div>

// Extension extracts:
const questionData = {
  id: "http_q1",
  text: "What does HTTP stand for?",
  options: [
    "HyperText Transfer Protocol",
    "High Transfer Text Protocol",
    "HyperText Transmission Protocol", 
    "High Text Transfer Protocol"
  ]
};
```

**2. Get Answer (from AI or cache)**

```javascript
// First check cache
chrome.storage.local.get(['quizAnswers'], (result) => {
  const cached = result.quizAnswers?.['http_q1'];
  
  if (cached) {
    console.log('Found in cache:', cached);
    // {
    //   answerIndex: 0,
    //   answerText: "HyperText Transfer Protocol",
    //   correct: true
    // }
    
    selectAnswer(0); // Use option A
  } else {
    // Not in cache, call AI service
    fetch('https://api.example.com/solve', {
      method: 'POST',
      body: JSON.stringify(questionData)
    })
    .then(response => response.json())
    .then(aiResponse => {
      console.log('AI says:', aiResponse);
      // {
      //   correctAnswer: 0,
      //   confidence: 0.99
      // }
      
      selectAnswer(0); // AI says option A
      
      // Cache it for next time
      saveToCache('http_q1', 0, true);
    });
  }
});
```

**3. Select Answer (click the radio button)**

```javascript
function selectAnswer(index) {
  // Find the radio button for option A (index 0)
  const radios = document.querySelectorAll('input[type="radio"][name="http_q1"]');
  const correctRadio = radios[index]; // radios[0]
  
  // Click it
  correctRadio.checked = true;
  correctRadio.click();
  
  // Trigger change event (for JavaScript frameworks)
  correctRadio.dispatchEvent(new Event('change', { bubbles: true }));
  
  // Visual feedback
  correctRadio.closest('.option').style.backgroundColor = '#d4edda';
  
  console.log('Selected option', index, ':', correctRadio.nextSibling.textContent);
  // "Selected option 0: HyperText Transfer Protocol"
}
```

---

## Common Question Types

### Type 1: Multiple Choice (Radio Buttons)

```html
<input type="radio" name="q1" value="0" /> Option A
<input type="radio" name="q1" value="1" /> Option B
<input type="radio" name="q1" value="2" /> Option C
```

**How extension selects:**
```javascript
document.querySelector('input[type="radio"][value="1"]').click();
```

### Type 2: Multiple Select (Checkboxes)

```html
<input type="checkbox" name="q2[]" value="0" /> Option A
<input type="checkbox" name="q2[]" value="1" /> Option B
<input type="checkbox" name="q2[]" value="2" /> Option C
```

**How extension selects multiple:**
```javascript
// If correct answers are A and C (indices 0 and 2)
[0, 2].forEach(index => {
  document.querySelectorAll('input[type="checkbox"]')[index].click();
});
```

### Type 3: Text Input

```html
<textarea name="q3"></textarea>
```

**How extension fills text:**
```javascript
document.querySelector('textarea[name="q3"]').value = "The answer is...";
document.querySelector('textarea[name="q3"]').dispatchEvent(new Event('input'));
```

### Type 4: Dropdown

```html
<select name="q4">
  <option value="0">Option A</option>
  <option value="1">Option B</option>
  <option value="2">Option C</option>
</select>
```

**How extension selects:**
```javascript
document.querySelector('select[name="q4"]').value = "1"; // Option B
document.querySelector('select[name="q4"]').dispatchEvent(new Event('change'));
```

---

## Summary: The Two Parts

### 🔍 Getting Questions

**Source:** The webpage HTML (Coursera's quiz interface)

**Method:** DOM queries using `document.querySelector()` and similar

**Data Extracted:**
- Question text
- Answer options
- Question type
- Question ID

**Code Pattern:**
```javascript
const questionText = document.querySelector('.question-text').innerText;
const options = Array.from(document.querySelectorAll('.option'))
  .map(opt => opt.innerText);
```

---

### 💡 Getting Answers

**Sources (in priority order):**
1. **Local cache** (chrome.storage.local) - previous correct answers
2. **Feedback memory** - eliminate wrong answers from past attempts
3. **AI service** (external API) - get fresh answer

**Method:** 
- Check cache first (instant)
- If not cached, call AI API (takes 1-2 seconds)
- Store result for next time

**Code Pattern:**
```javascript
// Simplified version
async function getAnswer(questionId, questionText, options) {
  // Try cache first
  const cached = await checkCache(questionId);
  if (cached) return cached;
  
  // Call AI service
  const response = await fetch('AI_API_URL', {
    method: 'POST',
    body: JSON.stringify({ questionText, options })
  });
  
  const answer = await response.json();
  await saveToCache(questionId, answer);
  
  return answer.correctIndex;
}
```

---

## Where Is This Code?

The actual implementation is in:
- **File:** `dist/scripts/content.js`
- **Size:** 1,072,503 bytes (1 MB)
- **Format:** Heavily obfuscated and minified
- **Status:** Cannot read directly without deobfuscation

To see the code in action:
1. Load extension in Chrome
2. Open a Coursera quiz
3. Open DevTools (F12) → Sources → Content Scripts
4. Set breakpoints and step through

See [DEBUGGING_GUIDE.md](DEBUGGING_GUIDE.md) for detailed instructions.

---

## Key Takeaways

✅ **Questions** come from the webpage HTML (using DOM queries)

✅ **Answers** come from:
   - Cache (if you've done the quiz before)
   - AI service (paid premium feature)
   - Previous feedback (learns from mistakes)

✅ The extension **doesn't create questions** - it reads them from Coursera's page

✅ The extension **doesn't guess** - it gets answers from an AI that analyzes the question

✅ The process is **automated** - happens within 10 seconds of loading the quiz

---

## Learn More

- **[QUIZ_DOCUMENTATION_INDEX.md](QUIZ_DOCUMENTATION_INDEX.md)** - Navigation hub
- **[CODE_ANALYSIS.md](CODE_ANALYSIS.md)** - Detailed pseudocode
- **[ARCHITECTURE_DIAGRAM.md](ARCHITECTURE_DIAGRAM.md)** - Visual diagrams
- **[DEBUGGING_GUIDE.md](DEBUGGING_GUIDE.md)** - How to see it in action
