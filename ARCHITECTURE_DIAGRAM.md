# Quiz Automation Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          COURSERA QUIZ PAGE                              │
│  https://www.coursera.org/learn/course/quiz/attempt                     │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ Page Load
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     CHROME EXTENSION INJECTION                           │
│                                                                          │
│  manifest.json triggers content script injection at document_start      │
│                                                                          │
│  "content_scripts": [{                                                   │
│    "run_at": "document_start",                                          │
│    "matches": ["*://*.coursera.org/*"],                                 │
│    "js": ["dist/scripts/content.js", "dist/scripts/xyz.js"]            │
│  }]                                                                      │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ Scripts Load
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      CONTENT.JS INITIALIZATION                           │
│                          (Obfuscated)                                    │
│                                                                          │
│  1. Detect if current page is a quiz                                    │
│  2. Wait 10 seconds for questions to load                               │
│  3. Initialize MutationObserver for DOM changes                         │
│  4. Set up keyboard event listeners (Alt+Z, Alt+W)                      │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ After 10 sec wait
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         QUESTION DETECTION                               │
│                                                                          │
│  const questions = document.querySelectorAll('.quiz-question')          │
│                                                                          │
│  For each question found:                                               │
│  ┌────────────────────────────────────────────────────────────┐        │
│  │ Extract:                                                    │        │
│  │  • Question ID                                             │        │
│  │  • Question text                                           │        │
│  │  • Question type (radio/checkbox/text/dropdown)            │        │
│  │  • Available options                                       │        │
│  │  • Attempt number                                          │        │
│  └────────────────────────────────────────────────────────────┘        │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ Question data collected
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         ANSWER RETRIEVAL                                 │
│                                                                          │
│  Decision tree:                                                          │
│                                                                          │
│  ┌─────────────────────────────────────────────────┐                   │
│  │ Check chrome.storage.local for cached answer    │                   │
│  └─────────────────┬───────────────────────────────┘                   │
│                    │                                                     │
│        ┌───────────┴───────────┐                                        │
│        │                       │                                        │
│        ▼                       ▼                                        │
│   [Cache Hit]             [Cache Miss]                                  │
│        │                       │                                        │
│        │                       │                                        │
│        ▼                       ▼                                        │
│  ┌──────────┐         ┌────────────────┐                              │
│  │ Is it    │         │ First attempt? │                              │
│  │ correct? │         └───────┬────────┘                              │
│  └────┬─────┘                 │                                        │
│       │                       │                                        │
│   ┌───┴────┐           ┌──────┴────────┐                             │
│   │        │           │               │                             │
│   ▼        ▼           ▼               ▼                             │
│  Yes      No     [Yes, 1st]     [No, Reattempt]                       │
│   │        │           │               │                             │
│   │        │           │               │                             │
│   │        │           ▼               ▼                             │
│   │        │    ┌─────────────┐  ┌──────────────────┐               │
│   │        │    │  Fetch from │  │ Use feedback to  │               │
│   │        │    │  AI Service │  │ eliminate wrong  │               │
│   │        │    │  (Premium)  │  │ answers, then AI │               │
│   │        │    └─────────────┘  └──────────────────┘               │
│   │        │           │               │                             │
│   └────────┴───────────┴───────────────┘                             │
│                        │                                               │
│                        ▼                                               │
│              ┌──────────────────┐                                     │
│              │ Answer Retrieved │                                     │
│              └──────────────────┘                                     │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ Answer data ready
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         ANSWER SELECTION                                 │
│                                                                          │
│  Based on question type:                                                 │
│                                                                          │
│  ┌───────────────────────────────────────────────────────────┐         │
│  │ RADIO BUTTON (Multiple Choice)                            │         │
│  │  • Find: input[type="radio"][value="correct"]            │         │
│  │  • Action: element.click()                               │         │
│  │  • Trigger: change event                                 │         │
│  └───────────────────────────────────────────────────────────┘         │
│                                                                          │
│  ┌───────────────────────────────────────────────────────────┐         │
│  │ CHECKBOX (Multiple Correct)                               │         │
│  │  • Find: input[type="checkbox"] (multiple)               │         │
│  │  • Action: each.click() for correct answers              │         │
│  │  • Trigger: change event on each                         │         │
│  └───────────────────────────────────────────────────────────┘         │
│                                                                          │
│  ┌───────────────────────────────────────────────────────────┐         │
│  │ TEXT INPUT                                                 │         │
│  │  • Find: textarea or input[type="text"]                  │         │
│  │  • Action: element.value = answer                        │         │
│  │  • Trigger: input and change events                      │         │
│  └───────────────────────────────────────────────────────────┘         │
│                                                                          │
│  ┌───────────────────────────────────────────────────────────┐         │
│  │ DROPDOWN (Select)                                          │         │
│  │  • Find: select element                                   │         │
│  │  • Action: element.value = answer                        │         │
│  │  • Trigger: change event                                 │         │
│  └───────────────────────────────────────────────────────────┘         │
│                                                                          │
│  Visual Feedback:                                                        │
│  • Highlight selected answer in green                                   │
│  • Add "✓ Auto-selected" badge                                         │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ All questions answered
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         USER SUBMITS QUIZ                                │
│                                                                          │
│  User clicks "Submit" button                                            │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ Submission complete
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         FEEDBACK COLLECTION                              │
│                                                                          │
│  Wait 2 seconds for results to load                                     │
│                                                                          │
│  ┌────────────────────────────────────────────────────────────┐        │
│  │ For each question:                                          │        │
│  │  • Check if answer was correct/incorrect                   │        │
│  │  • Read feedback message                                   │        │
│  │  • Store in chrome.storage.local:                          │        │
│  │    {                                                        │        │
│  │      questionId: {                                          │        │
│  │        answer: selectedAnswer,                              │        │
│  │        correct: true/false,                                 │        │
│  │        feedback: "...",                                     │        │
│  │        attemptNumber: n                                     │        │
│  │      }                                                       │        │
│  │    }                                                        │        │
│  └────────────────────────────────────────────────────────────┘        │
│                                                                          │
│  This data is used on next attempt to improve accuracy                  │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ If user retakes quiz
                                    ▼
                        ┌─────────────────────────┐
                        │  Cycle repeats but with │
                        │  feedback data to guide │
                        │  better answer selection│
                        └─────────────────────────┘


═══════════════════════════════════════════════════════════════════════════
                         KEYBOARD SHORTCUTS
═══════════════════════════════════════════════════════════════════════════

┌─────────────────────────────────────────────────────────────────────────┐
│  Alt + Z  or  Ctrl + Shift + Z                                          │
│  ───────────────────────────────                                        │
│  • Clear cached answers                                                 │
│  • Reload page                                                          │
│  • Fetch fresh answers                                                  │
│  • Navigate to next question                                            │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│  Alt + W  or  Alt + B                                                   │
│  ───────────────────────                                                │
│  • Toggle extension popup                                               │
│  • Show extension controls                                              │
└─────────────────────────────────────────────────────────────────────────┘


═══════════════════════════════════════════════════════════════════════════
                         COMMUNICATION FLOW
═══════════════════════════════════════════════════════════════════════════

┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   Popup      │ Message │  Background  │ Message │   Content    │
│   popup.js   │◄───────►│ background.js│◄───────►│  content.js  │
└──────────────┘         └──────────────┘         └──────────────┘
      │                         │                         │
      │                         │                         │
      ▼                         ▼                         ▼
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│User controls │         │Manages state │         │DOM manip.    │
│Feature select│         │Permissions   │         │Quiz detection│
│Settings      │         │Storage       │         │Answer select │
└──────────────┘         └──────────────┘         └──────────────┘
                                                          │
                                                          │
                                                          ▼
                                                   ┌──────────────┐
                                                   │  AI Service  │
                                                   │  (External)  │
                                                   │  [Premium]   │
                                                   └──────────────┘


═══════════════════════════════════════════════════════════════════════════
                         DATA STORAGE STRUCTURE
═══════════════════════════════════════════════════════════════════════════

chrome.storage.local:
{
  "quizAnswers": {
    "question_id_1": {
      "questionText": "What is 2+2?",
      "selectedAnswer": 2,
      "answerText": "4",
      "correct": true,
      "attemptNumber": 1,
      "timestamp": 1234567890,
      "feedback": "Correct!"
    },
    "question_id_2": {
      "questionText": "What is the capital of France?",
      "selectedAnswer": 0,
      "answerText": "Paris",
      "correct": true,
      "attemptNumber": 2,
      "timestamp": 1234567891,
      "feedback": "Correct! Well done.",
      "previousAttempts": [
        {
          "answer": 1,
          "correct": false,
          "feedback": "Incorrect. Try again."
        }
      ]
    }
  },
  "settings": {
    "autoSubmit": false,
    "highlightAnswers": true,
    "waitTime": 10000,
    "premiumActive": true
  },
  "stats": {
    "questionsAnswered": 42,
    "correctAnswers": 38,
    "accuracy": 0.90
  }
}


═══════════════════════════════════════════════════════════════════════════
                         TYPICAL QUESTION HTML
═══════════════════════════════════════════════════════════════════════════

<div class="quiz-question" data-question-id="abc123">
  <div class="question-text">
    <p>What is 2 + 2?</p>
  </div>
  
  <div class="options">
    <div class="option" data-option-index="0">
      <input type="radio" name="q1" value="3" />
      <label>3</label>
    </div>
    
    <div class="option" data-option-index="1">
      <input type="radio" name="q1" value="4" />
      <label>4</label>  ← CORRECT (Extension clicks this)
    </div>
    
    <div class="option" data-option-index="2">
      <input type="radio" name="q1" value="5" />
      <label>5</label>
    </div>
  </div>
  
  <div class="feedback" style="display:none">
    <!-- Appears after submission -->
  </div>
</div>


═══════════════════════════════════════════════════════════════════════════
                         FILE SIZE & COMPLEXITY
═══════════════════════════════════════════════════════════════════════════

File                    Size        Lines    Complexity
─────────────────────────────────────────────────────────
content.js             1,072,503    0        VERY HIGH (Obfuscated)
background.js            129,703    0        HIGH (Obfuscated)
popup.js                  67,878    0        MEDIUM (Obfuscated)
xyz.js                    96,867    data     HIGH (Binary-like)
settings.js              283,572    0        HIGH (Obfuscated)
rdr.js                   180,515    0        HIGH (Obfuscated)

Note: "0 lines" means all code is on a single line (minified)
```
