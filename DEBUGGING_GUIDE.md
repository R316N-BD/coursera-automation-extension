# Practical Guide: Debugging Quiz Automation

## Quick Start: See the Code in Action

This guide shows you **exactly how to see the quiz automation code working in real-time**.

## Method 1: Chrome DevTools Inspection (Easiest)

### Step-by-Step Instructions

1. **Install the Extension**
   ```
   1. Open Chrome
   2. Go to chrome://extensions/
   3. Enable "Developer mode" (top right)
   4. Click "Load unpacked"
   5. Select this repository folder
   ```

2. **Open a Coursera Quiz**
   ```
   1. Go to any Coursera course
   2. Navigate to a quiz
   3. Click to start the quiz
   ```

3. **Open Chrome DevTools**
   ```
   Press F12 or Right-click → Inspect
   ```

4. **Navigate to Content Script**
   ```
   1. Click "Sources" tab
   2. Look for "Content scripts" section (left sidebar)
   3. Expand to find "content.js"
   4. Click on it to view the code
   ```

5. **Beautify the Code (Optional)**
   ```
   1. With content.js open
   2. Click {} icon at bottom left (Pretty print)
   3. Code will be reformatted (still obfuscated)
   ```

6. **Set Breakpoints**
   ```
   1. Search for keywords (Ctrl+F):
      - "click"
      - "querySelector"
      - "input"
      - "checked"
   
   2. Click line numbers to set breakpoints
   
   3. Reload the page (F5)
   
   4. Code will pause at breakpoints
   
   5. Use these controls:
      - F8: Resume execution
      - F10: Step over
      - F11: Step into
      - Shift+F11: Step out
   ```

7. **Inspect Variables**
   ```
   When paused at a breakpoint:
   
   1. Hover over variables to see values
   2. Check "Scope" panel (right) for all variables
   3. Use Console to evaluate expressions
   ```

### Example Debugging Session

```javascript
// In Console tab, while on quiz page:

// 1. Check if extension is running
console.log('Extension loaded:', typeof chrome !== 'undefined');

// 2. Inspect current page elements
console.log('Quiz questions:', document.querySelectorAll('[class*="quiz"]'));

// 3. Check storage
chrome.storage.local.get(null, data => {
  console.log('Extension storage:', data);
});

// 4. Monitor clicks
let clickCount = 0;
document.addEventListener('click', (e) => {
  clickCount++;
  console.log(`Click #${clickCount}:`, e.target);
}, true);

// 5. Find radio buttons
const radios = document.querySelectorAll('input[type="radio"]');
console.log(`Found ${radios.length} radio buttons`);
radios.forEach((radio, i) => {
  console.log(`Radio ${i}:`, {
    value: radio.value,
    checked: radio.checked,
    name: radio.name
  });
});

// 6. Watch for answer selection
setInterval(() => {
  const checked = document.querySelectorAll('input:checked');
  if (checked.length > 0) {
    console.log('Answers selected:', checked.length);
  }
}, 1000);
```

## Method 2: Network Traffic Analysis

### Monitor API Calls

1. **Open Network Tab**
   ```
   1. F12 → Network tab
   2. Check "Preserve log"
   3. Clear existing logs (🚫 icon)
   ```

2. **Filter Requests**
   ```
   1. Type in filter: "XHR" or "Fetch"
   2. This shows AJAX requests
   ```

3. **Load Quiz**
   ```
   1. Reload the quiz page
   2. Wait 10 seconds (extension's documented wait time)
   3. Watch for new network requests
   ```

4. **Inspect Answer Requests**
   ```
   Look for requests that might be fetching answers:
   
   - URL patterns like: /api/answers, /quiz/solution, etc.
   - Request payload with question text
   - Response with answer data
   
   Click on request to see:
   - Headers
   - Payload
   - Response
   - Timing
   ```

### Example Network Request to Look For

```
Request URL: https://api.example.com/quiz/answer
Method: POST
Status: 200

Request Payload:
{
  "questionId": "abc123",
  "questionText": "What is 2+2?",
  "options": ["3", "4", "5", "6"],
  "courseId": "ml-course-2024"
}

Response:
{
  "correctAnswer": 1,
  "confidence": 0.95,
  "explanation": "2+2 equals 4"
}
```

## Method 3: Storage Inspection

### View Extension Storage

```javascript
// Method A: Using Console

// Get all storage
chrome.storage.local.get(null, (result) => {
  console.log('All storage:', result);
});

// Get specific keys
chrome.storage.local.get(['quizAnswers', 'settings'], (result) => {
  console.log('Quiz answers:', result.quizAnswers);
  console.log('Settings:', result.settings);
});

// Set storage (for testing)
chrome.storage.local.set({
  testData: { hello: 'world' }
}, () => {
  console.log('Data saved');
});

// Clear storage
chrome.storage.local.clear(() => {
  console.log('Storage cleared');
});
```

### Using Chrome Storage Inspector

1. **Open DevTools**
2. **Go to Application tab**
3. **Expand "Storage" in sidebar**
4. **Click "Local Storage" or "Extension Storage"**
5. **View all key-value pairs**

## Method 4: Code Deobfuscation

### Online Tools

1. **JS Beautifier** (https://beautifier.io/)
   ```
   1. Copy content of dist/scripts/content.js
   2. Paste into beautifier
   3. Click "Beautify"
   4. Download beautified code
   ```

2. **de4js** (https://lelinhtinh.github.io/de4js/)
   ```
   More advanced deobfuscation:
   1. Paste obfuscated code
   2. Click "Auto Decode"
   3. Try different deobfuscation methods
   4. Download result
   ```

3. **JavaScript Deobfuscator** (https://deobfuscate.io/)
   ```
   1. Upload content.js file
   2. Wait for processing
   3. Download deobfuscated version
   ```

### Local Deobfuscation with Node.js

```javascript
// Save as deobfuscate.js
const fs = require('fs');

// Read obfuscated code
const code = fs.readFileSync('dist/scripts/content.js', 'utf8');

// Basic beautification
const beautified = code
  .replace(/;/g, ';\n')
  .replace(/{/g, '{\n')
  .replace(/}/g, '\n}')
  .replace(/,/g, ',\n');

// Save result
fs.writeFileSync('content-beautified.js', beautified);

console.log('Beautified code saved to content-beautified.js');
```

Run with:
```bash
node deobfuscate.js
```

## Method 5: Live DOM Observation

### Watch for DOM Changes

```javascript
// Run in Console on quiz page

// Create observer
const observer = new MutationObserver((mutations) => {
  mutations.forEach((mutation) => {
    // Log all DOM changes
    console.log('DOM changed:', {
      type: mutation.type,
      target: mutation.target,
      addedNodes: mutation.addedNodes.length,
      removedNodes: mutation.removedNodes.length
    });
    
    // Check for input changes
    if (mutation.target.tagName === 'INPUT') {
      console.log('Input changed:', {
        type: mutation.target.type,
        checked: mutation.target.checked,
        value: mutation.target.value
      });
    }
  });
});

// Observe entire document
observer.observe(document.body, {
  attributes: true,
  childList: true,
  subtree: true,
  attributeOldValue: true,
  characterData: true
});

// Stop observing after 60 seconds
setTimeout(() => {
  observer.disconnect();
  console.log('Observer stopped');
}, 60000);
```

### Watch for Specific Events

```javascript
// Monitor all clicks
monitorEvents(document, 'click');

// Monitor input changes
monitorEvents(document.querySelector('input'), 'change');

// Stop monitoring
unmonitorEvents(document);

// Custom event listener
document.addEventListener('click', (e) => {
  if (e.target.type === 'radio' || e.target.type === 'checkbox') {
    console.log('Quiz option clicked:', {
      type: e.target.type,
      name: e.target.name,
      value: e.target.value,
      checked: e.target.checked,
      timestamp: new Date().toISOString()
    });
  }
}, true); // Use capture phase
```

## Method 6: Performance Profiling

### Record Extension Activity

1. **Open Performance Tab** in DevTools
2. **Click Record** (⚫)
3. **Load quiz page and wait 10 seconds**
4. **Stop recording**
5. **Analyze timeline** to see:
   - JavaScript execution
   - DOM manipulation
   - Network requests
   - Event handlers

### Key Things to Look For

- **Large JavaScript execution blocks** (extension logic)
- **Repeated DOM queries** (finding questions)
- **Event dispatching** (clicking answers)
- **Network activity spikes** (fetching answers)

## Method 7: Intercept Network Requests

### Using Fetch/XHR Breakpoints

```javascript
// Override fetch to log all requests
const originalFetch = window.fetch;
window.fetch = function(...args) {
  console.log('Fetch request:', args);
  return originalFetch.apply(this, args)
    .then(response => {
      console.log('Fetch response:', response);
      return response;
    });
};

// Override XMLHttpRequest
const originalXHROpen = XMLHttpRequest.prototype.open;
const originalXHRSend = XMLHttpRequest.prototype.send;

XMLHttpRequest.prototype.open = function(method, url, ...args) {
  this._method = method;
  this._url = url;
  return originalXHROpen.apply(this, [method, url, ...args]);
};

XMLHttpRequest.prototype.send = function(...args) {
  console.log('XHR request:', {
    method: this._method,
    url: this._url,
    data: args[0]
  });
  
  this.addEventListener('load', function() {
    console.log('XHR response:', {
      status: this.status,
      response: this.response
    });
  });
  
  return originalXHRSend.apply(this, args);
};
```

## Method 8: Extract String Table

### Find Encoded Strings

```javascript
// Run in Node.js
const fs = require('fs');
const code = fs.readFileSync('dist/scripts/content.js', 'utf8');

// Extract all string literals
const strings = code.match(/'([^']*)'/g) || [];

// Remove duplicates
const uniqueStrings = [...new Set(strings)];

// Filter for meaningful strings (length > 3)
const meaningful = uniqueStrings.filter(s => {
  const content = s.slice(1, -1); // Remove quotes
  return content.length > 3 && /[a-zA-Z]/.test(content);
});

// Sort by length (longer = more meaningful)
meaningful.sort((a, b) => b.length - a.length);

// Output top 100
console.log('Top 100 meaningful strings:');
meaningful.slice(0, 100).forEach((s, i) => {
  console.log(`${i+1}. ${s}`);
});
```

## Common Patterns to Search For

### In Beautified Code

```javascript
// 1. DOM selection patterns
document.querySelector
document.querySelectorAll
getElementsBy
closest(
querySelector(

// 2. Event handling
addEventListener
dispatchEvent
new Event
click()
change()

// 3. Input manipulation
.checked =
.value =
.selected =

// 4. Storage access
chrome.storage
localStorage
sessionStorage

// 5. Network requests
fetch(
XMLHttpRequest
axios
$.ajax

// 6. Common quiz elements
'input[type="radio"]'
'input[type="checkbox"]'
'.quiz'
'.question'
'.answer'
'.option'
'[data-question-id]'
```

## Keyboard Shortcut for Quick Access

Create a bookmarklet for quick debugging:

```javascript
javascript:(function(){
  console.log('Quiz Debug Info:');
  console.log('Questions:', document.querySelectorAll('[class*="quiz"]').length);
  console.log('Radios:', document.querySelectorAll('input[type="radio"]').length);
  console.log('Checkboxes:', document.querySelectorAll('input[type="checkbox"]').length);
  console.log('Checked:', document.querySelectorAll('input:checked').length);
  chrome.storage.local.get(null, d => console.log('Storage:', d));
})();
```

Save this as a bookmark and click it on any quiz page!

## Safety Tips

⚠️ **Important Warnings:**

1. **Don't share your findings publicly** - could help others cheat
2. **Respect academic integrity** - understand ethical implications
3. **Backup your work** - extension may update and change
4. **Test in isolation** - don't affect real quiz scores
5. **Follow ToS** - be aware of Coursera's terms

## Troubleshooting

### Extension Not Working

```javascript
// Check if extension is loaded
console.log('Chrome runtime:', chrome.runtime);

// Check content scripts
console.log('Scripts:', document.querySelectorAll('script[src*="chrome-extension"]'));

// Reload extension
// Go to chrome://extensions/ and click reload button
```

### Can't See Changes

```javascript
// Clear cache and reload
location.reload(true);

// Or programmatically:
chrome.storage.local.clear(() => {
  location.reload();
});
```

### Breakpoints Not Hitting

```
1. Make sure "Pause on exceptions" is OFF
2. Disable other extensions that might conflict
3. Try "Debugger" statements in code instead
4. Check if code is actually running (add console.log)
```

## Next Steps

After exploring the code:

1. **Document your findings**
2. **Create flow diagrams**
3. **Identify key functions**
4. **Understand the logic**
5. **Consider ethical implications**

## Need Help?

- Read the other documentation files:
  - QUIZ_CODE_SUMMARY.md - Overview
  - CODE_ANALYSIS.md - Detailed analysis
  - ARCHITECTURE_DIAGRAM.md - Visual flow
  - QUIZ_AUTOMATION_ANALYSIS.md - Technical details

- Use Chrome DevTools documentation:
  - https://developer.chrome.com/docs/devtools/

- Learn about extension debugging:
  - https://developer.chrome.com/docs/extensions/mv3/tut_debugging/
