# Coursera Automation Extension

![Version](https://img.shields.io/badge/version-3.6.4-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Chrome-blue)
![CI Status](https://github.com/R316N-BD/coursera-automation-extension/workflows/Extension%20CI/badge.svg)

> 🚀 **Visit the Official Website:** [👉 algoplug.com/products/coursera-automation](https://algoplug.com/products/coursera-automation)

Using this extension, complete Coursera courses within seconds. It can be used for free to complete lectures, dialogue, reading materials, ungraded plugins, discussions, shareable link and course backup. `Note:` Quiz completion, Skip Video+ and Course backup is a paid feature.

`Disclaimer:` Quiz Automation provides AI-generated answers, which may not always be accurate.

## Table of Contents

- [Demo](#demo)
- [Features](#features-status)
- [Installation](#installation)
- [How to use](#how-to-use)
- [Shareable Link](#shareable-link)
- [Skip Video+](#skipvideoplus)
- [Quiz Automation](#quiz-automation)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Author](#author)
- [License](#license)

## Demo

<div align="center">
  <a href="https://youtu.be/jKkWUVLRLnE"><img src="https://i.ytimg.com/vi/jKkWUVLRLnE/hqdefault.jpg" alt="Coursera Automation Extension"></a>
</div>

## Features Status

| Feature                              | Status     | Notes                                                                           |
| ------------------------------------ | ---------- | ------------------------------------------------------------------------------- |
| Complete quizzes                     | 🟢 Working | [Watch Demo](https://youtu.be/TrJqspKdKlw)                                      |
| Complete lectures                    | 🟢 Working |                                                                                 |
| Complete reading materials           | 🟢 Working |                                                                                 |
| Complete ungraded plugins (dialogue) | 🟢 Working |                                                                                 |
| Complete discussions                 | 🟢 Working |                                                                                 |
| Shareable link                       | 🟢 Working |                                                                                 |
| Quiz automation                      | 🟢 Working | [Watch Demo](https://youtu.be/TrJqspKdKlw) Feedback-based accuracy improvement. |
| Skip Video+                          | 🟢 Working | [Watch Demo](https://youtu.be/3OZZ5n2eS8s)                                      |
| Course Backup                        | 🟢 Working | [Watch Demo](https://youtu.be/KpmoToGLD-I)                                      |

## Installation

1. Clone or download the repository.
2. Open the Chrome browser.
3. Go to `chrome://extensions/`.
4. Enable `Developer mode`.
5. Click on `Load unpacked`.
6. Select the downloaded folder.
7. The extension will be added to the browser.

## How to use

1. Open your Coursera course.
2. Click on the Extension icon.
3. Choose the task you want to automate.
4. Sit back while the Extension completes the task.

## Shareable Link

To generate a **Shareable Link** for your assignment submission:

1. **Navigate to the Submission Page**: Ensure you're on the "My Submission" tab of your assignment.

2. **Use the Extension**: Click the "Shareable Link" button provided by the extension.

## Skip Video+

Automatically skip Coursera video lectures and mark them as completed.

- Remove the existing extension, then Reinstall the extension.

## Quiz Automation

Extension will automatically get the solutions and mark the correct options.

`Note:` When you Reattempt the quiz, it's using the previous attempt feedback to improve the accuracy of the answers.

1. Open the quiz you want to complete.
2. **Wait 10 seconds** for answers to load.
3. If answers aren't marked correctly, press Alt + Z for a refresh.
4. Submit your answers.

## Keyboard Shortcuts

- `Alt + W` or `Alt + B`: Toggle the Extension
- `Alt + Z` or `Ctrl + Shift + Z`: Refresh the answers or Navigate to the next question(mark as correct)

## Troubleshooting

### Extension Not Working

1. **Refresh the Page**: Try refreshing the Coursera page after installing the extension
2. **Check Permissions**: Ensure the extension has permission to access Coursera.org
3. **Update Extension**: Make sure you're using the latest version
4. **Clear Cache**: Clear your browser cache and reload the extension
5. **Reinstall**: Remove and reinstall the extension if issues persist

### Quiz Automation Issues

- **Wait Time**: Allow at least 10 seconds for answers to load before submitting
- **Incorrect Answers**: Use `Alt + Z` to refresh if answers aren't marked correctly
- **Reattempts**: When retaking a quiz, the extension uses previous feedback to improve accuracy

### Video Not Skipping

- **Reinstall Extension**: Remove the existing extension and reinstall for Skip Video+ feature to work properly
- **Check Settings**: Verify Skip Video+ is enabled in the extension settings

### Common Errors

| Error | Solution |
|-------|----------|
| Extension icon not visible | Enable the extension in chrome://extensions/ |
| Buttons not appearing | Ensure you're on a valid Coursera course page |
| Keyboard shortcuts not working | Check for conflicts with other extensions |

### Getting Help

If you encounter issues not covered here:
1. Check existing [GitHub Issues](https://github.com/R316N-BD/coursera-automation-extension/issues)
2. Create a new issue with detailed information about your problem
3. Include browser version, extension version, and steps to reproduce

## Browser Compatibility

| Browser | Status | Notes |
|---------|--------|-------|
| Chrome | ✅ Fully Supported | Recommended browser |
| Edge (Chromium) | ✅ Supported | Works with Chrome extensions |
| Brave | ⚠️ Partially Supported | May require additional permissions |
| Firefox | ❌ Not Supported | Requires Manifest V2 port |
| Safari | ❌ Not Supported | Different extension format required |

## Privacy & Security

- **No Data Collection**: This extension does not collect or transmit any personal data
- **Local Processing**: All operations are performed locally in your browser
- **Open Source**: The code is publicly available for review
- **Secure**: Regular security updates and vulnerability checks

See [SECURITY.md](SECURITY.md) for security policies and reporting vulnerabilities.

## FAQ

### Is this extension free to use?

Partially. Basic features (lectures, reading materials, dialogues, discussions) are free. Premium features (Quiz automation, Skip Video+, Course backup) require payment.

### Will using this extension violate Coursera's Terms of Service?

This extension automates certain tasks on Coursera. Users should review Coursera's Terms of Service and use this tool responsibly. We recommend using it for review purposes and personal productivity.

### How accurate is the Quiz Automation feature?

The Quiz automation provides AI-generated answers that may not always be 100% accurate. Accuracy improves with each reattempt as the extension learns from previous feedback.

### Can I use this extension on mobile devices?

No, this extension is designed for desktop Chrome browsers only. Mobile browsers do not support Chrome extensions.

### Does this extension work with all Coursera courses?

The extension works with most Coursera courses, but some newer course formats or special course types may not be fully supported.

### How do I update the extension?

If installed via Chrome Web Store, it updates automatically. If installed manually (developer mode), you need to pull the latest code and reload the extension.

### Is my data safe?

Yes. The extension operates entirely locally in your browser and does not send any data to external servers.

### How does the quiz automation code work?

The quiz automation code is heavily obfuscated in the distributed files. We've created comprehensive documentation to help you understand how it works:

- **[QUIZ_CODE_SUMMARY.md](QUIZ_CODE_SUMMARY.md)** - Quick overview of quiz automation
- **[CODE_ANALYSIS.md](CODE_ANALYSIS.md)** - Detailed pseudocode and implementation patterns
- **[ARCHITECTURE_DIAGRAM.md](ARCHITECTURE_DIAGRAM.md)** - Visual flow diagrams
- **[DEBUGGING_GUIDE.md](DEBUGGING_GUIDE.md)** - Practical debugging instructions
- **[QUIZ_AUTOMATION_ANALYSIS.md](QUIZ_AUTOMATION_ANALYSIS.md)** - Technical architecture

These documents explain the quiz detection, answer retrieval, and answer selection mechanisms.

## Support the Developer

If you find this extension helpful, consider supporting its development to keep it alive and growing! Every contribution makes a difference. ❤️

- Donate via UPI: devjs@jio
- [Sponsor me on GitHub](https://github.com/sponsors/sauravhathi)
- [Buy me a coffee](https://www.buymeacoffee.com/sauravhathi)
- [Donate via PayPal](https://paypal.me/sauravkumar680)

## Author

- [Saurav Hathi](https://github.com/sauravhathi)

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/sauravhathi/coursera-automation-extension/blob/master/LICENSE) file for details.

---

> ⚠️ This extension is an independent tool designed for personal learning productivity and is not affiliated with or endorsed by Coursera.

---
