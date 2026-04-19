# Contributing to Weather Runner 🤝

Спасибо за интерес к проекту! Вот как вы можете помочь.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Coding Standards](#coding-standards)
- [Pull Request Process](#pull-request-process)
- [Community](#community)

## Code of Conduct

### Our Pledge
- Будьте уважительны к другим участникам
- Конструктивная критика приветствуется
- Фокусируйтесь на том, что лучше для сообщества
- Проявляйте эмпатию к другим пользователям

### Unacceptable Behavior
- Оскорбления или дискриминация
- Троллинг и провокации
- Публикация私人ной информации других людей
- Спам и реклама

## How Can I Contribute?

### 🐛 Reporting Bugs

**Before submitting a bug report:**
- Проверьте [existing issues](https://github.com/yourusername/weather-runner/issues)
- Попробуйте воспроизвести проблему в другом браузере
- Проверьте консоль браузера (F12) на ошибки

**When submitting a bug report, include:**
- Описание проблемы
- Шаги для воспроизведения
- Ожидаемое поведение
- Фактическое поведение
- Скриншоты (если возможно)
- Браузер и ОС
- Версия игры

**Example:**
```markdown
**Bug**: Персонаж проваливается сквозь землю в снежную погоду

**Steps to Reproduce:**
1. Дождитесь снежной погоды
2. Прыгните 3 раза подряд
3. Приземлитесь на сугроб

**Expected:** Персонаж стоит на сугробе
**Actual:** Персонаж проваливается ниже groundY

**Browser:** Chrome 120, Windows 11
**Version:** 1.0.0
```

### 💡 Suggesting Enhancements

**Before suggesting:**
- Проверьте existing issues и discussions
- Убедитесь, что идея соответствует концепции игры

**When suggesting, include:**
- Название улучшения
- Подробное описание
- Обоснование (почему это полезно)
- Возможные реализации
- Скриншоты/мокапы (если есть)

### 🔧 Your First Code Contribution

**Good first issues:**
- Исправление опечаток в документации
- Добавление комментариев к коду
- Улучшение UI/UX мелких элементов
- Оптимизация производительности

**Finding issues:**
- Ищите метки `good-first-issue` или `help-wanted`
- Начните с простых багфиксов
- Читайте DEVELOPER_NOTES.md перед началом

### 🚀 Pull Requests

**Process:**
1. Fork репозиторий
2. Создайте feature branch (`git checkout -b feature/amazing-feature`)
3. Внесите изменения
4. Протестируйте изменения
5. Закоммитьте изменения (`git commit -m 'Add amazing feature'`)
6. Запушьте branch (`git push origin feature/amazing-feature`)
7. Откройте Pull Request

**PR Checklist:**
- [ ] Код следует coding standards
- [ ] Добавлены комментарии для сложной логики
- [ ] Протестировано в Chrome и Firefox
- [ ] Обновлена документация (если нужно)
- [ ] Нет конфликтов с main branch
- [ ] Все тесты проходят (если есть)

## Development Setup

### Prerequisites
- Text editor (VS Code recommended)
- Modern browser (Chrome/Firefox)
- Basic JavaScript knowledge
- Git (для contribution)

### Local Development

1. **Clone repository**
   ```bash
   git clone https://github.com/yourusername/weather-runner.git
   cd weather-runner
   ```

2. **Open in browser**
   ```bash
   # Option 1: Double-click index.html
   # Option 2: Use local server (recommended)
   npx http-server -p 8080
   ```

3. **Open DevTools**
   - Press F12
   - Go to Console tab
   - Check for errors

4. **Make changes**
   - Edit index.html
   - Refresh browser (Ctrl+R)
   - Test thoroughly

5. **Test across browsers**
   - Chrome
   - Firefox
   - Safari (if on Mac)
   - Mobile devices

### Testing

**Manual testing checklist:**
- [ ] Game starts correctly
- [ ] Weather API works (or fallback)
- [ ] All weather types display correctly
- [ ] Player jumps with Space/tap
- [ ] Collisions detected properly
- [ ] Score increments
- [ ] High score saves
- [ ] Game over screen appears
- [ ] Restart button works

**Performance testing:**
- Open DevTools → Performance tab
- Record 10 seconds of gameplay
- Check FPS is stable at 60
- Memory usage < 50 MB
- No memory leaks

## Coding Standards

### JavaScript Style Guide

**General:**
- Use ES6+ features (classes, arrow functions, const/let)
- 2-space indentation
- Semicolons required
- Single quotes for strings
- Trailing commas in multi-line objects/arrays

**Naming:**
- Classes: PascalCase (`class PlayerController`)
- Variables: camelCase (`const playerSpeed`)
- Constants: UPPER_SNAKE_CASE (`const MAX_LIVES`)
- Private methods: prefix with underscore (`_internalMethod()`)

**Example:**
```javascript
// Good
class Player {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.maxJumps = 2;
  }

  jump() {
    if (this.jumpCount < this.maxJumps) {
      this.velocityY = this.JUMP_FORCE;
      this.jumpCount++;
    }
  }

  _checkGroundCollision() {
    // Internal method
  }
}

// Bad
class player {
  constructor(x,y) {
    this.x=x
    this.y=y
  }
  
  Jump() {
    // Wrong naming
  }
}
```

**Comments:**
- Use JSDoc for classes and public methods
- Inline comments for complex logic
- TODO comments for future work

```javascript
/**
 * Handles player movement and physics
 */
class Player {
  /**
   * Makes the player jump if conditions are met
   * @returns {boolean} Whether jump was successful
   */
  jump() {
    // Reset jump count when on ground
    if (this.isOnGround) {
      this.jumpCount = 0;
    }
    
    // TODO: Add double jump power-up
    if (this.jumpCount < this.maxJumps) {
      // ...
    }
  }
}
```

### HTML/CSS Standards

**HTML:**
- Semantic tags where possible
- Proper indentation (2 spaces)
- Lowercase tag names
- Quotes around attributes

**CSS:**
- BEM naming convention (`.block__element--modifier`)
- Mobile-first approach
- Use CSS variables for colors
- Group related properties

```css
/* Good */
.game-container {
  --primary-color: #4CAF50;
  --background-dark: #1a1a2e;
  
  position: relative;
  width: 800px;
  height: 400px;
}

.game-container__overlay--hidden {
  display: none;
}

/* Bad */
.container {
  position:relative;
  width:800px;
  color: #4CAF50; /* Magic number */
}
```

### Commit Messages

**Format:**
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples:**
```bash
# Good
feat(weather): add storm weather type with airborne lightning
fix(player): resolve collision detection edge case
docs(readme): update installation instructions

# Bad
update stuff
fixed bug
wip
```

## Pull Request Process

### Before Submitting

1. **Update documentation**
   - README.md if features changed
   - DEVELOPER_NOTES.md if architecture changed
   - CHANGELOG.md with your changes

2. **Test thoroughly**
   - Multiple browsers
   - Different weather conditions
   - Edge cases

3. **Clean up code**
   - Remove console.log statements
   - Remove commented-out code
   - Format consistently

### PR Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Tested in Chrome
- [ ] Tested in Firefox
- [ ] Manual testing completed
- [ ] Performance tested

## Screenshots (if applicable)
Add screenshots of UI changes

## Additional Notes
Any additional context or concerns
```

### Review Process

1. **Automated checks**
   - No syntax errors
   - File structure correct

2. **Maintainer review**
   - Code quality
   - Adherence to standards
   - Functionality verification

3. **Feedback incorporation**
   - Address review comments
   - Make requested changes
   - Re-request review

4. **Merge**
   - Squash commits if needed
   - Update CHANGELOG.md
   - Close related issues

## Community

### Getting Help
- Read documentation first (README, QUICKSTART, DEVELOPER_NOTES)
- Search existing issues
- Ask in discussions
- Be patient and respectful

### Recognition

Contributors will be added to:
- CONTRIBUTORS.md file
- Release notes
- Project README (top contributors)

### Communication

- **Issues**: Bug reports and feature requests
- **Discussions**: Questions and ideas
- **Pull Requests**: Code contributions
- **Email**: Security issues only

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

**Thank you for contributing to Weather Runner! 🎉**

Every contribution, no matter how small, makes the project better.