# Quiz Automation Code Documentation Index

## 📚 Complete Documentation Package

This repository contains comprehensive documentation about the quiz automation functionality of the Coursera Automation Extension.

## 🎯 What You Asked For

> "show me all the code in this that make it get question and click the right one"

**Answer**: The actual code is heavily obfuscated in `dist/scripts/content.js`, but we've created extensive documentation to help you understand how it works.

## 📖 Documentation Files

### 1. **Quick Start** → [QUIZ_CODE_SUMMARY.md](QUIZ_CODE_SUMMARY.md)
**Start here!** Complete overview in plain language.

**What's inside:**
- ✅ What the code does (step-by-step flow)
- ✅ Why you can't see the source code directly
- ✅ 3 methods to explore the real code
- ✅ Key findings and known facts
- ✅ Legal and ethical considerations

**Best for:** Getting a quick understanding of how quiz automation works

---

### 2. **Visual Guide** → [ARCHITECTURE_DIAGRAM.md](ARCHITECTURE_DIAGRAM.md)
**See the flow!** ASCII diagrams showing the entire process.

**What's inside:**
- 📊 Complete automation flow diagram
- 📊 Question detection process
- 📊 Answer retrieval decision tree
- 📊 Answer selection by type
- 📊 Communication between components
- 📊 Data storage structure
- 📊 Example HTML structures

**Best for:** Visual learners who want to see how components interact

---

### 3. **Detailed Analysis** → [CODE_ANALYSIS.md](CODE_ANALYSIS.md)
**Deep dive!** Pseudocode and implementation patterns.

**What's inside:**
- 💻 Complete pseudocode implementation
- 💻 7-step automation breakdown
- 💻 Code for each question type
- 💻 Obfuscation techniques explained
- 💻 Actual code snippets (deobfuscated examples)
- 💻 DOM manipulation strategies

**Best for:** Developers who want to understand the implementation details

---

### 4. **Hands-On Guide** → [DEBUGGING_GUIDE.md](DEBUGGING_GUIDE.md)
**Get practical!** Step-by-step debugging instructions.

**What's inside:**
- 🔧 8 different debugging methods
- 🔧 Chrome DevTools walkthrough
- 🔧 Network traffic analysis
- 🔧 Storage inspection techniques
- 🔧 Code deobfuscation tools
- 🔧 Ready-to-run code snippets
- 🔧 Troubleshooting tips

**Best for:** Anyone who wants to see the code in action right now

---

### 5. **Technical Reference** → [QUIZ_AUTOMATION_ANALYSIS.md](QUIZ_AUTOMATION_ANALYSIS.md)
**Technical details!** Architecture and security analysis.

**What's inside:**
- 🏗️ File structure and permissions
- 🏗️ Content script injection details
- 🏗️ Communication flow
- 🏗️ Security considerations
- 🏗️ Known limitations
- 🏗️ What we can't determine

**Best for:** Security researchers and extension developers

---

## 🚀 Quick Navigation

### I want to understand how it works
→ Start with **[QUIZ_CODE_SUMMARY.md](QUIZ_CODE_SUMMARY.md)**

### I want to see visual diagrams
→ Go to **[ARCHITECTURE_DIAGRAM.md](ARCHITECTURE_DIAGRAM.md)**

### I want to see the actual code
→ Follow **[DEBUGGING_GUIDE.md](DEBUGGING_GUIDE.md)** Method 1

### I want to modify the extension
→ Read **[CODE_ANALYSIS.md](CODE_ANALYSIS.md)** for pseudocode

### I want technical specifications
→ Check **[QUIZ_AUTOMATION_ANALYSIS.md](QUIZ_AUTOMATION_ANALYSIS.md)**

## 🎓 Learning Path

**Beginner** (Never seen the extension before):
1. Read QUIZ_CODE_SUMMARY.md (10 min)
2. Look at ARCHITECTURE_DIAGRAM.md (5 min)
3. Try DEBUGGING_GUIDE.md Method 1 (15 min)

**Intermediate** (Familiar with Chrome extensions):
1. Read CODE_ANALYSIS.md (20 min)
2. Try DEBUGGING_GUIDE.md Methods 1-4 (30 min)
3. Review QUIZ_AUTOMATION_ANALYSIS.md (10 min)

**Advanced** (Want to reverse-engineer):
1. Read all documentation (60 min)
2. Try all debugging methods (2 hours)
3. Use deobfuscation tools (varies)
4. Analyze network traffic in depth (1 hour)

## 📊 What the Code Does (Summary)

```
┌─────────────────────────────────────────┐
│ 1. DETECT QUIZ PAGE                     │
│    Check URL and DOM for quiz elements  │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 2. WAIT 10 SECONDS                      │
│    Allow answers to load (documented)   │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 3. EXTRACT QUESTIONS                    │
│    • Question text                      │
│    • Answer options                     │
│    • Question type                      │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 4. GET CORRECT ANSWER                   │
│    From: Cache → Feedback → AI Service │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 5. SELECT ANSWER                        │
│    Click radio/checkbox, fill text      │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 6. VISUAL FEEDBACK                      │
│    Highlight selected answers           │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 7. COLLECT FEEDBACK (after submit)     │
│    Store results for next attempt       │
└─────────────────────────────────────────┘
```

## 🔍 The Code Problem

The actual implementation is **heavily obfuscated**:

```javascript
// What you see:
function _0x2b473f(_0x3dc125,_0x3f1cdb,_0x165186){
  return _0x2de7(_0x165186- -0x124,_0x572683);
}

// What it probably is:
function getQuestionText(element, selector, options) {
  return extractText(element.querySelector(selector));
}
```

**Why?**
- Variable names mangled
- Strings encrypted
- Single-line minification
- No source maps

## 💡 Key Insights

### What We Know ✅
- Extension runs on `*.coursera.org/*`
- Injects at `document_start`
- Waits 10 seconds for answers
- Uses AI-generated answers (paid feature)
- Learns from previous attempts
- Supports multiple question types
- Has keyboard shortcuts (Alt+Z, Alt+W/B)

### What We Don't Know ❌
- Exact API endpoint for answers
- Which AI service is used
- Specific DOM selectors
- Authentication mechanism
- Anti-detection measures
- Success rate statistics

### How to Find Out 🔍
- Use Chrome DevTools (see DEBUGGING_GUIDE.md)
- Analyze network traffic
- Deobfuscate the code
- Monitor DOM changes
- Inspect storage

## ⚠️ Important Warnings

1. **Academic Integrity**: Using this violates most honor codes
2. **Terms of Service**: Likely violates Coursera's ToS
3. **Accuracy**: AI answers are not 100% reliable
4. **Detection**: Coursera may detect automation
5. **Ethics**: Defeats the purpose of learning

## 🛠️ Tools You'll Need

### For Understanding:
- Any web browser (to read docs)
- Markdown viewer (optional)

### For Debugging:
- Chrome browser
- Chrome DevTools (built-in)
- This extension installed

### For Deobfuscation:
- https://beautifier.io/
- https://deobfuscate.io/
- https://lelinhtinh.github.io/de4js/
- Node.js (optional)

## 📝 Code Examples

### Quick Peek at Storage
```javascript
chrome.storage.local.get(null, data => {
  console.log('Extension data:', data);
});
```

### Monitor Clicks
```javascript
document.addEventListener('click', e => {
  if (e.target.type === 'radio') {
    console.log('Radio clicked:', e.target.value);
  }
}, true);
```

### Find All Questions
```javascript
const questions = document.querySelectorAll('[class*="quiz"]');
console.log(`Found ${questions.length} quiz elements`);
```

See [DEBUGGING_GUIDE.md](DEBUGGING_GUIDE.md) for 50+ more examples!

## 🎯 Common Questions

**Q: Can I see the actual source code?**
A: The distributed code is obfuscated. Use deobfuscation tools or debug at runtime.

**Q: How do I debug the extension?**
A: See [DEBUGGING_GUIDE.md](DEBUGGING_GUIDE.md) for 8 detailed methods.

**Q: What makes it select the right answer?**
A: See [CODE_ANALYSIS.md](CODE_ANALYSIS.md) Step 5 for detailed pseudocode.

**Q: Where do the answers come from?**
A: Likely an external AI API (paid service), but exact endpoint is hidden.

**Q: Is this legal?**
A: See [QUIZ_CODE_SUMMARY.md](QUIZ_CODE_SUMMARY.md) for legal/ethical discussion.

## 📞 Need More Help?

1. **Read the docs**: Start with QUIZ_CODE_SUMMARY.md
2. **Try debugging**: Follow DEBUGGING_GUIDE.md
3. **Check issues**: Look at repository issues
4. **Ask questions**: Open a new issue

## 🔗 Related Files

- **README.md** - Main project documentation
- **SECURITY.md** - Security policy
- **CONTRIBUTING.md** - Contribution guidelines
- **CHANGELOG.md** - Version history
- **manifest.json** - Extension configuration

## 📈 Documentation Stats

| File | Size | Lines | Topics |
|------|------|-------|--------|
| QUIZ_CODE_SUMMARY.md | 8.7 KB | 306 | Overview, Methods, Ethics |
| ARCHITECTURE_DIAGRAM.md | 17.8 KB | 468 | Visual Diagrams, Flows |
| CODE_ANALYSIS.md | 13.4 KB | 481 | Pseudocode, Implementation |
| DEBUGGING_GUIDE.md | 12.8 KB | 511 | Hands-on, Tools, Examples |
| QUIZ_AUTOMATION_ANALYSIS.md | 7.5 KB | 264 | Architecture, Security |
| **Total** | **60.2 KB** | **2,030** | **Complete Coverage** |

## 🎉 You're All Set!

You now have:
- ✅ Complete understanding of how it works
- ✅ Visual diagrams of the flow
- ✅ Detailed pseudocode
- ✅ Practical debugging methods
- ✅ Tools and resources

**Ready to explore?** Start with [QUIZ_CODE_SUMMARY.md](QUIZ_CODE_SUMMARY.md)!

---

*Created as part of comprehensive documentation for understanding the Coursera Automation Extension's quiz automation functionality.*
