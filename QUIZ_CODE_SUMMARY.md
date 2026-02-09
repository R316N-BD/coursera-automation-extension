# Quiz Automation Code: Complete Guide

## Quick Answer

**You asked to see "all the code that makes it get question and click the right one"**

Unfortunately, the actual implementation code is **heavily obfuscated and minified**, making it impossible to show you the readable source code directly. However, I've created comprehensive documentation explaining:

1. **What the code does** (behavior and architecture)
2. **How it likely works** (pseudocode examples)
3. **Why you can't see it** (obfuscation techniques)
4. **How to analyze it** (debugging methods)

## Documentation Created

I've created three detailed documents for you:

### 1. **QUIZ_AUTOMATION_ANALYSIS.md**
- High-level architecture overview
- Quiz automation flow
- File structure and permissions
- Limitations and security considerations

### 2. **CODE_ANALYSIS.md**
- Detailed pseudocode of likely implementation
- Code structure breakdown
- Obfuscation techniques explained
- Debugging instructions for Chrome DevTools

### 3. **This Summary** (QUIZ_CODE_SUMMARY.md)
- Quick reference guide
- Key findings
- How to explore the code yourself

## Key Findings

### The Files Involved

```
dist/scripts/content.js    - Main quiz automation (1MB, obfuscated)
dist/scripts/xyz.js        - Additional content script
dist/scripts/background.js - Background service worker
```

### What The Code Does

```
1. DETECT QUIZ PAGE
   ↓
2. WAIT 10 SECONDS FOR ANSWERS TO LOAD
   ↓
3. EXTRACT QUESTION DATA
   - Question text
   - Answer options
   - Question type (radio/checkbox/text)
   ↓
4. FETCH CORRECT ANSWER
   - From AI service (paid feature)
   - From local cache
   - From previous attempt feedback
   ↓
5. SELECT THE ANSWER
   - Click radio buttons
   - Check checkboxes
   - Fill text fields
   ↓
6. COLLECT FEEDBACK (after submission)
   - Store correct/incorrect info
   - Improve accuracy on reattempts
```

### The Problem: Obfuscation

The code uses multiple obfuscation techniques:

**Variable Name Mangling:**
```javascript
// Human-readable:
function getQuizQuestions() { ... }

// Obfuscated:
function _0x2b473f() { ... }
```

**String Encryption:**
```javascript
// Strings are encoded and decoded at runtime
const _0x3fac56 = {_0x7504ce:'PHHF', _0x4395de:0x1852};
```

**Single-Line Minification:**
```javascript
// All 1MB+ of code on ONE line with no formatting
(function(_0x2b473f,_0x48e90d){const _0x3fac56={_0x7504ce...
```

## How to See the Real Code

Since the source code is obfuscated, here are your options:

### Option 1: Use Deobfuscation Tools

Online tools:
- https://beautifier.io/ - Format the code
- https://deobfuscate.io/ - Attempt to deobfuscate
- https://lelinhtinh.github.io/de4js/ - Advanced deobfuscation

Steps:
1. Copy content of `dist/scripts/content.js`
2. Paste into deobfuscator
3. Wait for processing (may take a while due to file size)
4. Analyze the output

### Option 2: Runtime Debugging in Chrome

1. **Load the Extension:**
   ```
   chrome://extensions/
   → Enable Developer Mode
   → Load unpacked → Select this folder
   ```

2. **Open a Coursera Quiz:**
   ```
   Navigate to any Coursera quiz page
   ```

3. **Open Chrome DevTools:**
   ```
   Press F12 or Right-click → Inspect
   ```

4. **Go to Sources Tab:**
   ```
   Sources → Content Scripts → content.js
   ```

5. **Set Breakpoints:**
   ```
   Ctrl+F to search for keywords like:
   - "click"
   - "querySelector"  
   - "checked"
   - "answer"
   
   Click line numbers to set breakpoints
   ```

6. **Step Through Code:**
   ```
   Reload the page
   Code will pause at breakpoints
   Step through with F10/F11
   Inspect variables in Scope panel
   ```

7. **Monitor Network:**
   ```
   Network tab → Filter: XHR/Fetch
   Look for API calls to answer services
   ```

8. **Check Storage:**
   ```javascript
   // In Console tab:
   chrome.storage.local.get(null, data => {
     console.log('Stored answers:', data);
   });
   ```

### Option 3: Analyze Network Traffic

The extension likely makes API calls to fetch answers:

1. **Open DevTools Network Tab**
2. **Start recording**
3. **Open a quiz**
4. **Wait 10 seconds** (as documented)
5. **Look for XHR/Fetch requests** to external services
6. **Inspect Request/Response** to see answer data format

## Pseudocode: How It Works

Here's what the code likely does (simplified):

```javascript
// 1. Detect quiz page
if (isQuizPage()) {
  
  // 2. Wait for questions to load
  setTimeout(() => {
    
    // 3. Find all questions
    const questions = document.querySelectorAll('.quiz-question');
    
    questions.forEach(question => {
      
      // 4. Extract question data
      const questionText = question.querySelector('.question-text').innerText;
      const options = question.querySelectorAll('.option');
      
      // 5. Get answer (from AI or cache)
      getAnswer(questionText, options, (correctIndex) => {
        
        // 6. Click the correct option
        if (question.querySelector('input[type="radio"]')) {
          // Multiple choice
          options[correctIndex].querySelector('input').click();
        } else if (question.querySelector('input[type="checkbox"]')) {
          // Checkboxes (multiple answers)
          correctIndex.forEach(i => {
            options[i].querySelector('input').click();
          });
        }
        
        // 7. Visual feedback
        options[correctIndex].style.backgroundColor = 'lightgreen';
      });
    });
    
  }, 10000); // Wait 10 seconds as documented
}

// 8. After submission, collect feedback
document.querySelector('.submit-button').addEventListener('click', () => {
  setTimeout(() => {
    // Check which answers were correct
    // Store for next attempt
    collectFeedback();
  }, 2000);
});
```

## Key Code Patterns (What to Look For)

When debugging, search for these patterns:

### DOM Selection
```javascript
document.querySelector('.quiz-question')
document.querySelectorAll('input[type="radio"]')
element.closest('[data-question-id]')
```

### Event Simulation
```javascript
element.click()
element.dispatchEvent(new Event('change'))
element.checked = true
```

### Storage Access
```javascript
chrome.storage.local.get(...)
chrome.storage.local.set(...)
```

### Network Requests
```javascript
fetch('https://api.../answers', {...})
XMLHttpRequest.send(...)
```

### Keyboard Shortcuts
```javascript
document.addEventListener('keydown', (e) => {
  if (e.altKey && e.key === 'z') {
    // Alt+Z handler
  }
})
```

## What We Know for Certain

From analyzing the files and documentation:

✅ **Extension runs on**: `*.coursera.org/*`  
✅ **Injects at**: `document_start` (before page loads)  
✅ **Main file**: `dist/scripts/content.js` (1,072,503 bytes)  
✅ **Wait time**: 10 seconds for answers to load  
✅ **Feedback**: Uses previous attempts to improve  
✅ **Shortcuts**: Alt+Z refreshes, Alt+W/B toggles  
✅ **Premium**: Quiz automation is a paid feature  
✅ **AI-powered**: Uses AI-generated answers  

## What We Don't Know

❌ **API endpoint** - Where answers come from  
❌ **AI service** - Which AI provider is used  
❌ **Authentication** - How premium access is verified  
❌ **Exact selectors** - Which DOM elements are targeted  
❌ **Success rate** - How accurate the answers are  
❌ **Anti-detection** - If any measures are taken  

## Legal & Ethical Considerations

⚠️ **Important Disclaimers:**

1. **Academic Integrity**: Using quiz automation violates most academic integrity policies
2. **Terms of Service**: Likely violates Coursera's Terms of Service
3. **Accuracy**: AI-generated answers are not 100% accurate
4. **Detection Risk**: Coursera may detect and penalize automated behavior
5. **Learning**: Defeats the purpose of online education

## Conclusion

The quiz automation code is **intentionally hidden** through heavy obfuscation. The documentation I've provided gives you:

1. **Understanding** of what the code does
2. **Pseudocode examples** of how it likely works
3. **Debugging instructions** to analyze it yourself
4. **Key patterns** to search for

To see the actual code, you would need to:
- Use deobfuscation tools, OR
- Debug it at runtime in Chrome DevTools, OR
- Contact the extension author for source code

## Need More Help?

If you want to:
- **Deobfuscate the code**: Use the tools mentioned above
- **Modify the extension**: Study the pseudocode examples
- **Understand specific parts**: Use the debugging instructions
- **Report issues**: Check the repository's issues page

## Related Files

- `QUIZ_AUTOMATION_ANALYSIS.md` - Architecture overview
- `CODE_ANALYSIS.md` - Detailed pseudocode and patterns
- `README.md` - User documentation
- `SECURITY.md` - Security policies
- `dist/scripts/content.js` - The actual (obfuscated) code
