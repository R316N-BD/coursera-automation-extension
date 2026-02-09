### Contribution Guidelines for Coursera Automation Extension

Thank you for considering contributing to the Coursera Automation Extension! Here are some guidelines to help you get started:

## How to Contribute

1. **Fork the Repository**
   - Clone your fork to your local machine.
   - Add the original repository as an upstream remote to keep your fork updated.

2. **Create a Branch**
   - Create a new branch for your feature or bugfix.
   - Use a descriptive name for your branch (e.g., `feature/add-new-feature` or `bugfix/fix-issue`).

3. **Make Changes**
   - Write clear and concise commit messages.
   - Follow the coding style and conventions used in the project.
   - Test your changes thoroughly before committing.
   - Ensure your code follows these standards:
     - Use meaningful variable and function names
     - Add comments for complex logic
     - Keep functions small and focused
     - Avoid code duplication
     - Maintain consistent indentation (2 spaces for JS/HTML/CSS)
     - Use modern JavaScript (ES6+) features where appropriate

4. **Push Changes**
   - Push your changes to your forked repository.
   - Create a pull request to the main repository.
   - Provide a clear description of your changes and the problem they solve.

## Guidelines for Reporting Issues

1. **Search Existing Issues**
   - Before opening a new issue, search the existing issues to avoid duplicates.

2. **Create a Detailed Report**
   - Provide a clear and descriptive title.
   - Describe the steps to reproduce the issue.
   - Include any relevant logs, screenshots, or code snippets.

3. **Label Your Issues**
   - Use appropriate labels to categorize your issues (e.g., bug, enhancement, question).

## Code Review Process

All submissions require review. We use GitHub pull requests for this purpose:

1. **Automated Checks**: Your PR will be automatically checked by CI/CD workflows
2. **Code Review**: Maintainers will review your code for quality and compatibility
3. **Testing**: Ensure your changes don't break existing functionality
4. **Documentation**: Update relevant documentation if needed

## Development Setup

1. Clone the repository
2. Make your changes in the `dist` directory or source files
3. Test the extension locally:
   - Open Chrome and go to `chrome://extensions/`
   - Enable "Developer mode"
   - Click "Load unpacked" and select the repository folder
4. Verify your changes work as expected

## Commit Message Guidelines

- Use present tense ("Add feature" not "Added feature")
- Use imperative mood ("Move cursor to..." not "Moves cursor to...")
- Limit the first line to 72 characters or less
- Reference issues and pull requests after the first line

Example:
```
Add quiz retry functionality

- Implement automatic retry on failed questions
- Add user notification for retry status
- Update documentation

Fixes #123
```

## Questions?

Feel free to ask questions by opening an issue with the "question" label.
