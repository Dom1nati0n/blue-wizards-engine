# Contributing to Blue Wizards Engine

Thank you for your interest in contributing to the Blue Wizards Engine! This document provides guidelines for contributing to the project.

## Ways to Contribute

### 1. Example Programs
Write interesting programs that demonstrate language features:
- Educational examples for beginners
- Complex programs showcasing advanced features
- Game implementations
- Algorithmic demonstrations
- Creative/artistic code

**How to contribute examples:**
1. Create your program in the `examples/` directory
2. Use the `.bw` extension
3. Include comments explaining what the program does
4. Add a description to the examples README

### 2. Documentation
Help improve or expand documentation:
- Fix typos or clarify existing docs
- Add more detailed explanations
- Create tutorials for specific features
- Translate documentation to other languages
- Add diagrams or visualizations

### 3. Language Features
Propose or implement new opcodes:
- New operations that extend the language
- New data structures
- New event types
- New terrain or resource types

**Feature proposal process:**
1. Open an issue describing the feature
2. Discuss design and implementation
3. Submit a pull request with:
   - Implementation in the engine
   - Documentation updates
   - Example programs using the feature

### 4. Bug Fixes
Found a bug? Help us fix it:
1. Check if it's already reported in Issues
2. If not, open a new issue with:
   - Description of the bug
   - Minimal example that reproduces it
   - Expected vs actual behavior
3. Submit a pull request with the fix

### 5. Performance Improvements
Optimize the engine:
- Faster execution
- Better memory usage
- More efficient algorithms
- Profiling and benchmarking

### 6. Tools and Utilities
Create tools that make working with Blue Wizards easier:
- Syntax highlighters for editors
- Debuggers or visualizers
- IDE plugins
- Grid visualizers
- Program analyzers

## Development Setup

1. Install J (version 9.0 or higher)
2. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/blue-wizards-engine.git
   cd blue-wizards-engine
   ```
3. Load the engine:
   ```j
   load 'engine/blue_wizards_v29.ijs'
   ```

## Code Style

### J Code
- Follow standard J conventions
- Use descriptive names for defined verbs
- Comment complex operations
- Keep functions focused and modular

### Documentation
- Write in clear, simple language
- Include code examples
- Use proper markdown formatting
- Keep examples concise but complete

### Example Programs
- Include header comments explaining the program
- Comment non-obvious sections
- Use meaningful variable names (in comments)
- Demonstrate one clear concept per example

## Pull Request Process

1. **Create a branch** for your changes
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**
   - Write clear, focused commits
   - Include tests if applicable
   - Update documentation

3. **Test thoroughly**
   - Run existing examples to ensure nothing breaks
   - Test your new code
   - Verify documentation renders correctly

4. **Submit pull request**
   - Provide clear description of changes
   - Reference any related issues
   - Explain the motivation for the change

5. **Code review**
   - Respond to feedback
   - Make requested changes
   - Keep discussion focused and respectful

## Testing

When adding new features:
1. Create test programs in `examples/tests/` (if not exists)
2. Document expected behavior
3. Verify edge cases
4. Test interaction with existing features

## Documentation Standards

### Language Reference
- List each opcode with:
  - Stack effect notation
  - Clear description
  - At least one example
  - Any special considerations

### Tutorial Content
- Start simple, build complexity gradually
- Explain *why* not just *how*
- Include complete, runnable examples
- Anticipate common mistakes

### Example Programs
- Include header block explaining:
  - What the program does
  - What features it demonstrates
  - Expected output
- Use comments to explain tricky sections
- Keep examples focused on one main concept

## Community Guidelines

- Be respectful and inclusive
- Welcome newcomers and help them learn
- Give constructive feedback
- Assume good intentions
- Keep discussions focused on the project

## Questions?

- **General questions**: Open a discussion
- **Bug reports**: Open an issue
- **Feature ideas**: Open an issue for discussion first
- **Security issues**: Email maintainers directly

## Recognition

Contributors will be:
- Listed in the project README
- Credited in release notes
- Acknowledged in documentation they contribute to

## License

By contributing to Blue Wizards Engine, you agree that your contributions will be licensed under the MIT License.

---

Thank you for helping make Blue Wizards more magical! 🧙‍♂️✨
