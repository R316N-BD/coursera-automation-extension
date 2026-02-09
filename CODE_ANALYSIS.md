# Code Visualization: Quiz Automation Implementation

## What We Know About The Code

### File: `dist/scripts/content.js`

**File Statistics:**
- Size: ~1.07 MB (1,072,503 bytes)
- Format: Single-line minified JavaScript
- Obfuscation: Heavy variable name mangling + string encoding
- Lines: 0 (all code on one line)

### Obfuscation Techniques Used

1. **Variable Name Mangling**
   ```javascript
   // Original might be something like:
   function getQuizQuestions() { ... }
   
   // Obfuscated to:
   function _0x2b473f() { ... }
   ```

2. **Hex-Encoded Values**
   ```javascript
   // Numbers are encoded as hex
   0x1852, 0x1014, 0xf93, 0x9cc
   ```

3. **String Encryption**
   ```javascript
   // Strings are encrypted and decoded at runtime
   const _0x3fac56 = {
     _0x7504ce: 'PHHF',
     _0x4395de: 0x1852,
     // ...
   };
   ```

4. **Self-Modifying Code**
   ```javascript
   // Uses array manipulation to reorder execution
   while(!![]){
     try{
       const _0x23b2e7 = parseInt(...);
       if(_0x23b2e7 === _0x48e90d) break;
       else _0x3d8cdf['push'](_0x3d8cdf['shift']());
     }catch(_0x2fd198){
       _0x3d8cdf['push'](_0x3d8cdf['shift']());
     }
   }
   ```

## Reverse Engineering Attempts

### Method 1: String Extraction

Running string extraction reveals some meaningful identifiers:

**DOM-Related Strings Found:**
- `'label'` - Likely used for form labels
- `'acceptNode'` - DOM tree walker
- `'iframeTitle'` - Iframe handling
- `'typeName'` - Element type detection
- `'timeCommitment'` - Course metadata
- `'ignoreCase'` - String comparison
- `'prototype'` - JavaScript prototypes
- `'querySelector'` - DOM selection
- `'addEventListener'` - Event handling

**Standard JavaScript:**
- `'push'`, `'shift'` - Array operations
- `'charAt'`, `'indexOf'` - String operations
- `'toString'`, `'fromCharCode'` - Type conversion
- `'random'`, `'round'` - Math operations

### Method 2: Structure Analysis

The code follows this general structure:

```javascript
(function(_0x2b473f, _0x48e90d) {
  // Self-executing anonymous function (IIFE)
  
  const _0x3fac56 = {
    // Encrypted string table
  };
  
  function _0x2de7(_0x158d4e, _0x4f820c) {
    // String decoder function
    // Decodes encrypted strings at runtime
  }
  
  // Multiple helper functions with obfuscated names
  function _0x23c8fb(...) { ... }
  function _0x17b733(...) { ... }
  function _0x1a917a(...) { ... }
  function _0x98c819(...) { ... }
  function _0x1ff685(...) { ... }
  
  // Main logic (heavily obfuscated)
  // ...
  
})(_0x5592, /* magic number */);
```

## Likely Implementation (Pseudocode)

Based on the extension's documented behavior and typical quiz automation patterns, here's what the code likely does:

### Step 1: Initialization

```javascript
// When content script loads on Coursera page
(function() {
  'use strict';
  
  // Check if we're on a quiz page
  if (window.location.href.includes('/quiz/') || 
      document.querySelector('[data-test="quiz-container"]')) {
    initQuizAutomation();
  }
})();
```

### Step 2: Question Detection

```javascript
function initQuizAutomation() {
  // Wait for DOM to be ready
  waitForElement('[data-test="quiz-question"]', (questions) => {
    questions.forEach(question => {
      processQuestion(question);
    });
  });
}

function waitForElement(selector, callback, timeout = 10000) {
  // MutationObserver to watch for quiz elements
  const observer = new MutationObserver((mutations) => {
    const element = document.querySelector(selector);
    if (element) {
      observer.disconnect();
      callback(document.querySelectorAll(selector));
    }
  });
  
  observer.observe(document.body, {
    childList: true,
    subtree: true
  });
  
  // Timeout after 10 seconds (as documented)
  setTimeout(() => observer.disconnect(), timeout);
}
```

### Step 3: Extract Question Data

```javascript
function processQuestion(questionElement) {
  // Extract question metadata
  const questionData = {
    id: questionElement.getAttribute('data-question-id'),
    type: getQuestionType(questionElement),
    text: getQuestionText(questionElement),
    options: getQuestionOptions(questionElement),
    attemptNumber: getAttemptNumber()
  };
  
  // Fetch answer from storage or AI service
  getAnswer(questionData, (answer) => {
    selectAnswer(questionElement, answer);
  });
}

function getQuestionType(element) {
  // Detect question type
  if (element.querySelector('input[type="radio"]')) {
    return 'multiple-choice';
  } else if (element.querySelector('input[type="checkbox"]')) {
    return 'checkbox';
  } else if (element.querySelector('textarea')) {
    return 'text';
  } else if (element.querySelector('select')) {
    return 'dropdown';
  }
  return 'unknown';
}

function getQuestionText(element) {
  // Extract the actual question text
  const questionTextEl = element.querySelector('.question-text') ||
                         element.querySelector('[data-test="question-prompt"]');
  return questionTextEl ? questionTextEl.innerText.trim() : '';
}

function getQuestionOptions(element) {
  // Extract all available options
  const options = [];
  element.querySelectorAll('.option, [data-test="quiz-option"]').forEach((opt, index) => {
    options.push({
      index: index,
      text: opt.innerText.trim(),
      value: opt.getAttribute('data-value') || opt.querySelector('input')?.value,
      element: opt
    });
  });
  return options;
}
```

### Step 4: Fetch Answer

```javascript
function getAnswer(questionData, callback) {
  // First, check local storage for cached answers
  chrome.storage.local.get(['quizAnswers'], (result) => {
    const cached = result.quizAnswers?.[questionData.id];
    
    if (cached && cached.correct) {
      // Use cached correct answer
      callback(cached.answer);
    } else if (cached && questionData.attemptNumber > 1) {
      // On reattempt, use feedback to improve
      getAnswerFromFeedback(questionData, cached.feedback, callback);
    } else {
      // Fetch from AI service (paid feature)
      fetchAnswerFromAI(questionData, callback);
    }
  });
}

function fetchAnswerFromAI(questionData, callback) {
  // Make API call to AI service
  // This is likely a paid/premium service endpoint
  fetch('https://api.example.com/quiz-solver', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer ' + getApiKey()
    },
    body: JSON.stringify({
      question: questionData.text,
      options: questionData.options.map(o => o.text),
      type: questionData.type
    })
  })
  .then(response => response.json())
  .then(data => {
    callback(data.answer);
    // Cache the answer
    cacheAnswer(questionData.id, data.answer);
  })
  .catch(error => {
    console.error('Failed to fetch answer:', error);
    callback(null);
  });
}

function getAnswerFromFeedback(questionData, feedback, callback) {
  // Use previous attempt feedback to eliminate wrong answers
  const validOptions = questionData.options.filter(opt => {
    return !feedback.incorrect.includes(opt.index);
  });
  
  // If feedback indicates correct answer, use it
  if (feedback.correct !== undefined) {
    callback(feedback.correct);
  } else if (validOptions.length === 1) {
    callback(validOptions[0].index);
  } else {
    // Need AI to choose from remaining options
    fetchAnswerFromAI({
      ...questionData,
      options: validOptions
    }, callback);
  }
}
```

### Step 5: Select Answer

```javascript
function selectAnswer(questionElement, answerIndex) {
  const questionType = getQuestionType(questionElement);
  
  if (answerIndex === null) {
    console.warn('No answer available');
    return;
  }
  
  switch (questionType) {
    case 'multiple-choice':
      selectRadioOption(questionElement, answerIndex);
      break;
    case 'checkbox':
      selectCheckboxOptions(questionElement, answerIndex);
      break;
    case 'text':
      fillTextAnswer(questionElement, answerIndex);
      break;
    case 'dropdown':
      selectDropdownOption(questionElement, answerIndex);
      break;
  }
  
  // Visual feedback
  highlightSelectedAnswer(questionElement, answerIndex);
}

function selectRadioOption(element, index) {
  const radio = element.querySelectorAll('input[type="radio"]')[index];
  if (radio) {
    // Simulate user click
    radio.checked = true;
    radio.dispatchEvent(new Event('change', { bubbles: true }));
    radio.click(); // Some forms need actual click event
    
    // Trigger React/Vue change detection
    const nativeInputValueSetter = Object.getOwnPropertyDescriptor(
      window.HTMLInputElement.prototype, 
      'checked'
    ).set;
    nativeInputValueSetter.call(radio, true);
    radio.dispatchEvent(new Event('input', { bubbles: true }));
  }
}

function selectCheckboxOptions(element, indices) {
  // For multiple correct answers
  if (!Array.isArray(indices)) {
    indices = [indices];
  }
  
  const checkboxes = element.querySelectorAll('input[type="checkbox"]');
  checkboxes.forEach((checkbox, i) => {
    const shouldCheck = indices.includes(i);
    if (checkbox.checked !== shouldCheck) {
      checkbox.checked = shouldCheck;
      checkbox.dispatchEvent(new Event('change', { bubbles: true }));
      checkbox.click();
    }
  });
}

function fillTextAnswer(element, text) {
  const textarea = element.querySelector('textarea') || 
                   element.querySelector('input[type="text"]');
  if (textarea) {
    textarea.value = text;
    textarea.dispatchEvent(new Event('input', { bubbles: true }));
    textarea.dispatchEvent(new Event('change', { bubbles: true }));
  }
}

function highlightSelectedAnswer(element, index) {
  // Add visual indicator that answer was auto-selected
  element.style.backgroundColor = '#e8f5e9';
  element.querySelector('.auto-selected-badge')?.remove();
  
  const badge = document.createElement('span');
  badge.className = 'auto-selected-badge';
  badge.textContent = '✓ Auto-selected';
  badge.style.cssText = 'color: green; font-weight: bold; margin-left: 10px;';
  element.querySelector('.question-text')?.appendChild(badge);
}
```

### Step 6: Feedback Collection

```javascript
function collectFeedback() {
  // After quiz submission, collect feedback
  setTimeout(() => {
    const feedbackElements = document.querySelectorAll('.feedback');
    feedbackElements.forEach((feedback, index) => {
      const isCorrect = feedback.classList.contains('correct');
      const questionId = feedback.closest('[data-question-id]')
                                 .getAttribute('data-question-id');
      
      // Store feedback for next attempt
      chrome.storage.local.get(['quizAnswers'], (result) => {
        const answers = result.quizAnswers || {};
        answers[questionId] = {
          ...answers[questionId],
          feedback: {
            correct: isCorrect,
            message: feedback.innerText
          }
        };
        chrome.storage.local.set({ quizAnswers: answers });
      });
    });
  }, 2000);
}
```

### Step 7: Keyboard Shortcuts

```javascript
// Handle Alt+Z refresh
document.addEventListener('keydown', (e) => {
  // Alt+Z or Ctrl+Shift+Z
  if ((e.altKey && e.key === 'z') || 
      (e.ctrlKey && e.shiftKey && e.key === 'Z')) {
    e.preventDefault();
    
    // Clear cache and reload answers
    chrome.storage.local.remove(['quizAnswers'], () => {
      location.reload();
    });
  }
});
```

## Actual Code Example (Deobfuscated Snippet)

Here's what a small portion of the actual code might look like after deobfuscation:

```javascript
// Before deobfuscation:
function _0x2de7(_0x158d4e,_0x4f820c){
  const _0x3554b8=_0x5592();
  return _0x2de7=function(_0x547adf,_0x1d6007){
    _0x547adf=_0x547adf-(-0xa25+-0x16d0+0x21d8);
    let _0x2c007b=_0x3554b8[_0x547adf];
    // ...
  };
}

// After deobfuscation (likely):
function decodeString(index, key) {
  const stringTable = getStringTable();
  return function(offset, modifier) {
    offset = offset - 0x183; // Decode offset
    let decodedString = stringTable[offset];
    // ... decoding logic
    return decodedString;
  };
}
```

## Chrome DevTools Debugging

To explore the actual code at runtime:

```javascript
// In browser console on Coursera quiz page:

// 1. List all content scripts
console.log(chrome.runtime.getManifest().content_scripts);

// 2. Inspect stored data
chrome.storage.local.get(null, data => console.log('Stored data:', data));

// 3. Monitor network requests
// Open Network tab in DevTools
// Look for XHR/Fetch requests when quiz loads

// 4. Set breakpoints
// Open Sources tab > Content Scripts > content.js
// Search for keywords and set breakpoints

// 5. Watch for click events
monitorEvents(document, 'click');
```

## Summary

The actual quiz automation code is **intentionally obfuscated** and difficult to analyze without:
1. Professional deobfuscation tools
2. Dynamic runtime analysis
3. Significant reverse engineering effort

The pseudocode above represents the **likely implementation** based on:
- Standard web scraping patterns
- Chrome extension best practices
- Documented behavior from README
- Typical quiz automation architectures

**To see the real code in action**, you would need to:
1. Load the extension in Chrome
2. Open DevTools on a Coursera quiz page
3. Set breakpoints in the minified code
4. Step through execution to understand the logic
