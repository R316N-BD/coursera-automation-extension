# Quiz Automation Code Analysis

## Overview

This document explains how the Coursera Automation Extension handles quiz automation - getting questions and selecting the correct answers.

## ⚠️ Important Notice

The actual implementation code is **heavily obfuscated and minified** in the distribution files. This analysis is based on:
- Observable behavior from the manifest and documentation
- File structure analysis
- Known Chrome extension patterns
- User-facing features documented in README

## Architecture

### File Structure

```
dist/
├── scripts/
│   ├── content.js      # Main quiz automation logic (1MB+, minified)
│   ├── background.js   # Background service worker
│   ├── xyz.js          # Additional content script
│   ├── popup.js        # Extension popup interface
│   └── rdr.js          # Web-accessible resource
├── popup.html          # Extension popup UI
└── settings.html       # Settings page
```

### Content Scripts

From `manifest.json`:
```json
"content_scripts": [
  {
    "run_at": "document_start",
    "matches": ["*://*.coursera.org/*"],
    "js": ["dist/scripts/content.js", "dist/scripts/xyz.js"],
    "css": ["dist/ui/content.css"]
  }
]
```

**Key Points:**
- Runs at `document_start` - injects before page fully loads
- Only runs on Coursera domains (`*.coursera.org`)
- Two JavaScript files injected: `content.js` and `xyz.js`

## Quiz Automation Flow (Documented Behavior)

### 1. Question Detection

The extension detects when a user is on a Coursera quiz page by:
- Monitoring the URL pattern
- Detecting quiz-specific DOM elements
- Listening for page state changes

### 2. Answer Retrieval

According to the documentation, the extension:
- Uses **AI-generated answers** (implementation details obscured)
- Requires a **10-second wait period** for answers to load
- Can access **previous attempt feedback** to improve accuracy
- Improves with each reattempt using feedback

**From README.md:**
```markdown
## Quiz Automation

Extension will automatically get the solutions and mark the correct options.

`Note:` When you Reattempt the quiz, it's using the previous attempt 
feedback to improve the accuracy of the answers.

1. Open the quiz you want to complete.
2. **Wait 10 seconds** for answers to load.
3. If answers aren't marked correctly, press Alt + Z for a refresh.
4. Submit your answers.
```

### 3. Answer Selection

The extension automatically:
- Identifies question types (multiple choice, checkbox, text input, etc.)
- Selects/clicks the correct options
- Marks answers visually for user review

### 4. User Interaction

**Keyboard Shortcuts:**
- `Alt + Z` or `Ctrl + Shift + Z` - Refresh answers or navigate to next question
- `Alt + W` or `Alt + B` - Toggle the extension popup

## Technical Implementation (What We Can Determine)

### Content Script Injection

```javascript
// The extension injects at document_start
// This allows early DOM manipulation before Coursera's scripts run
```

### Likely Implementation Pattern

Based on typical Chrome extension quiz automation patterns, the code likely:

1. **DOM Observation:**
   ```javascript
   // Watches for quiz elements to appear
   const observer = new MutationObserver((mutations) => {
     // Detect quiz questions
     // Find answer containers
   });
   ```

2. **Question Parsing:**
   ```javascript
   // Extracts question text and options
   // Identifies question type (radio, checkbox, text)
   // Gathers metadata (question ID, attempt number)
   ```

3. **Answer Fetching:**
   ```javascript
   // Makes API call or local storage lookup
   // Uses AI service (external or embedded)
   // Retrieves correct answer(s)
   ```

4. **Answer Selection:**
   ```javascript
   // Finds correct option elements
   // Simulates click events
   // Updates visual indicators
   document.querySelector('input[type="radio"]').click();
   ```

### Communication Flow

```
┌─────────────────┐
│  Coursera Page  │
└────────┬────────┘
         │
         │ (Content Scripts Inject)
         │
         ▼
┌─────────────────┐       ┌──────────────┐
│   content.js    │◄─────►│ background.js│
│   (Quiz Logic)  │       │  (Messages)  │
└────────┬────────┘       └──────────────┘
         │
         │ (DOM Manipulation)
         │
         ▼
┌─────────────────┐
│  Quiz Elements  │
│  - Questions    │
│  - Options      │
│  - Checkboxes   │
└─────────────────┘
```

## Code Obfuscation

The actual code in `dist/scripts/content.js` is:

1. **Minified**: All on a single line (65536+ characters)
2. **Obfuscated**: Variable names are mangled (e.g., `_0x2b473f`, `_0x48e90d`)
3. **Encrypted strings**: String literals are encoded
4. **No source maps**: Cannot reverse-engineer easily

**Example of obfuscated code:**
```javascript
(function(_0x2b473f,_0x48e90d){const _0x3fac56={_0x7504ce:'PHHF',_0x4395de:0x1852,
_0x2f2ee2:0x1014,_0x2bd49f:0xf93,_0xd7f97:'tKwR',...
```

## What We Cannot Determine

Due to obfuscation, we cannot identify:

1. ❌ **Exact API endpoints** used for fetching answers
2. ❌ **AI service provider** or algorithm
3. ❌ **Data storage mechanism** (local storage keys, format)
4. ❌ **DOM selectors** used to find questions/answers
5. ❌ **Error handling** and retry logic
6. ❌ **Rate limiting** or anti-detection measures
7. ❌ **Authentication** method for premium features

## Features (From Documentation)

### Free Features
- Complete lectures
- Complete reading materials
- Complete ungraded plugins (dialogue)
- Complete discussions
- Generate shareable links

### Premium Features (Paid)
- **Quiz automation** - AI-generated answers
- **Skip Video+** - Automatic video completion
- **Course backup** - Download course materials

## Security Considerations

From the manifest, the extension has these permissions:
```json
"permissions": [
  "storage",      // Local data storage
  "activeTab",    // Current tab access
  "scripting",    // Script injection
  "tabs",         // Tab management
  "commands",     // Keyboard shortcuts
  "downloads"     // File downloads
]
```

## Limitations

1. **Accuracy**: AI-generated answers may not be 100% correct
2. **Coursera Changes**: Extension may break if Coursera updates their UI
3. **Ethics**: Using automation may violate academic integrity policies
4. **Detection**: Coursera may detect and block automated behavior

## Development Notes

### To Understand the Code Further:

1. **Deobfuscate**: Use tools like:
   - JS Beautifier
   - de4js
   - JavaScript Deobfuscator

2. **Debug**: Run in Chrome DevTools:
   ```javascript
   // Open DevTools on Coursera quiz page
   // Inspect content script execution
   // Monitor network requests
   // Set breakpoints in minified code
   ```

3. **Monitor Storage**:
   ```javascript
   // Check chrome.storage for cached answers
   chrome.storage.local.get(null, (data) => console.log(data));
   ```

4. **Network Analysis**:
   - Monitor XHR/Fetch requests
   - Look for API calls to external services
   - Check for WebSocket connections

## Disclaimer

This extension automates Coursera quizzes, which may violate:
- Coursera's Terms of Service
- Academic integrity policies
- Course completion requirements

Users should understand the ethical implications and potential consequences of using automated quiz completion tools.

---

**Note**: This analysis is based on the distributed/minified code. The actual source code is not available in this repository. To fully understand the implementation, deobfuscation would be required, which is beyond the scope of this documentation.
