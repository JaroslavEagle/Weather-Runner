# Обновление: Двойной прыжок и Комикс-фразы 🦘💬

## Что нового (2026-04-19)

### ✅ Система двойного прыжка

Теперь игрок может прыгать **дважды** в воздухе!

#### Механика:
- **Первый прыжок**: Стандартный прыжок с земли
- **Второй прыжок**: Дополнительный прыжок в воздухе
- **Максимум**: 2 прыжка перед приземлением
- **Сброс**: Счётчик сбрасывается при касании земли

#### Технические детали:
```javascript
class Player {
    constructor() {
        this.jumpCount = 0;      // Текущее количество прыжков
        this.maxJumps = 2;       // Максимум прыжков
    }
    
    jump() {
        if (this.jumpCount < this.maxJumps) {
            this.velocityY = this.jumpForce;
            this.isOnGround = false;
            this.jumpCount++;
            
            // Первая фраза для обычного прыжка
            if (this.jumpCount === 1) {
                this.showRandomPhrase();
            }
            // Особая фраза для двойного прыжка
            else if (this.jumpCount === 2) {
                const doubleJumpPhrases = ['Двойной!', 'Супер!', 'Мега!', 'Максимум!'];
                this.currentPhrase = doubleJumpPhrases[Math.floor(Math.random() * doubleJumpPhrases.length)];
            }
        }
    }
    
    update() {
        // При приземлении сбрасываем счётчик
        if (this.y + this.height >= this.groundY) {
            this.jumpCount = 0;
        }
    }
}
```

---

### ✅ Система комикс-фраз

Игрок теперь говорит **весёлые фразы** при прыжках, которые появляются в **комикс-пузырях** над ним!

#### Обычные фразы (первый прыжок):
- "Вау!"
- "Ого!"
- "Ух ты!"
- "Круто!"
- "Лечу!"
- "Выше!"
- "Ещё!"
- "Прыг!"
- "Хоп!"
- "Вжух!"
- "Небо!"
- "Свобода!"
- "Полёт!"
- "Мощь!"
- "Бум!"

#### Особые фразы (двойной прыжок):
- "Двойной!"
- "Супер!"
- "Мега!"
- "Максимум!"

---

### 🎨 Визуальный дизайн комикс-пузырей

#### Характеристики:
- **Форма**: Закруглённый прямоугольник с хвостом
- **Цвет**: Белый фон с чёрной обводкой
- **Размер**: Адаптивный под текст
- **Позиция**: Над головой игрока (-40px)
- **Эффект затухания**: Плавное исчезновение за 1 секунду
- **Шрифт**: Bold Arial 14px
- **Хвост**: Треугольник, указывающий на игрока

#### Анимация:
```javascript
// Fade out эффект
const alpha = this.phraseTimer / this.phraseDuration;

// Постепенное уменьшение прозрачности
ctx.fillStyle = `rgba(255, 255, 255, ${alpha})`;
ctx.strokeStyle = `rgba(0, 0, 0, ${alpha})`;
ctx.fillText(this.currentPhrase, bubbleX, bubbleY);
```

---

## Технические детали

### Структура данных Player

```javascript
class Player {
    constructor(x, groundY) {
        // ... existing properties ...
        
        // Double jump system
        this.jumpCount = 0;
        this.maxJumps = 2;
        
        // Comic phrase system
        this.phrases = [
            'Вау!', 'Ого!', 'Ух ты!', 'Круто!', 'Лечу!',
            'Выше!', 'Ещё!', 'Прыг!', 'Хоп!', 'Вжух!',
            'Небо!', 'Свобода!', 'Полёт!', 'Мощь!', 'Бум!'
        ];
        this.currentPhrase = null;
        this.phraseTimer = 0;
        this.phraseDuration = 60; // 1 second at 60fps
    }
}
```

### Метод drawPhraseBubble

```javascript
drawPhraseBubble(ctx) {
    const bubbleX = this.x + this.width / 2;
    const bubbleY = this.y - 40;
    const fontSize = 14;
    
    ctx.save();
    
    // Calculate text width for adaptive bubble size
    ctx.font = `bold ${fontSize}px Arial`;
    const textMetrics = ctx.measureText(this.currentPhrase);
    const textWidth = textMetrics.width;
    const padding = 10;
    const bubbleWidth = textWidth + padding * 2;
    const bubbleHeight = fontSize + padding * 2;
    
    // Fade out effect based on timer
    const alpha = this.phraseTimer / this.phraseDuration;
    
    // Draw rounded rectangle bubble
    this.drawRoundedRect(ctx, bubbleX, bubbleY, bubbleWidth, bubbleHeight, radius, alpha);
    
    // Draw tail pointing to player
    this.drawTail(ctx, bubbleX, bubbleY, bubbleHeight, alpha);
    
    // Draw text centered in bubble
    ctx.fillStyle = `rgba(0, 0, 0, ${alpha})`;
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.fillText(this.currentPhrase, bubbleX, bubbleY);
    
    ctx.restore();
}
```

### Логика обновления таймера

```javascript
update(currentGravity) {
    // ... existing update logic ...
    
    // Update phrase timer
    if (this.phraseTimer > 0) {
        this.phraseTimer--;
        if (this.phraseTimer <= 0) {
            this.currentPhrase = null; // Clear phrase when timer expires
        }
    }
}
```

---

## Геймплейные улучшения

### 1. Enhanced Mobility (Улучшенная мобильность)
- **До**: Один прыжок, ограниченная высота
- **После**: Два прыжка, доступ к высоким бонусам
- **Преимущество**: Больше стратегических возможностей

### 2. Visual Feedback (Визуальная обратная связь)
- **До**: Тихий прыжок без эмоций
- **После**: Весёлые фразы в комикс-стиле
- **Эффект**: Игра становится более живой и персонализированной

### 3. Personality (Характер персонажа)
- **До**: Безликий квадрат
- **После**: Персонаж с характером и эмоциями
- **Результат**: Игрок привязывается к персонажу

### 4. Skill Expression (Выражение мастерства)
- **До**: Простой геймплей
- **После**: Возможность показать навыки двойного прыжка
- **Глубина**: Timing и positioning становятся важнее

---

## Примеры использования

### Сценарий 1: Обычный прыжок
```
Игрок нажимает Пробел → Персонаж прыгает
Над головой появляется: "Вау!"
Пузырь плавно исчезает за 1 секунду
```

### Сценарий 2: Двойной прыжок
```
Игрок нажимает Пробел (первый раз) → "Лечу!"
Игрок нажимает Пробел (второй раз в воздухе) → "Двойной!"
Персонаж достигает максимальной высоты
Оба пузыря появляются последовательно
```

### Сценарий 3: Сбор высокого бонуса
```
Бонус находится высоко → Недоступен с одним прыжком
Игрок делает двойной прыжок → "Мега!"
Бонус собран → "+50 Bonus!"
```

---

## Балансировка

### Jump Physics
| Параметр | Значение | Описание |
|----------|----------|----------|
| jumpForce | -10 px/frame | Сила прыжка |
| gravity | 0.5 px/frame² | Гравитация |
| maxJumps | 2 | Максимум прыжков |
| jumpReset | On ground | Сброс при приземлении |

### Phrase System
| Параметр | Значение | Описание |
|----------|----------|----------|
| phraseDuration | 60 frames | Длительность фразы (1 сек) |
| fadeOut | Linear | Линейное затухание |
| bubbleOffset | -40px | Смещение над головой |
| fontSize | 14px | Размер текста |

---

## UI/UX Улучшения

### Comic Bubble Design
```
    ┌─────────────┐
    │   Вау!      │ ← Закруглённый прямоугольник
    └──────┬──────┘
           ▼        ← Хвост к игроку
         [👤]       ← Игрок
```

### Animation Timeline
```
Frame 0:   Phrase appears (alpha = 1.0) ██████████
Frame 30:  Half visible (alpha = 0.5)   █████
Frame 60:  Disappears (alpha = 0.0)     
```

---

## Testing Checklist

### Двойной прыжок:
- [ ] Первый прыжок работает с земли
- [ ] Второй прыжок работает в воздухе
- [ ] Третий прыжок блокируется
- [ ] Счётчик сбрасывается при приземлении
- [ ] Можно достичь высоких бонусов
- [ ] Физика плавная и отзывчивая

### Комикс-фразы:
- [ ] Фразы появляются при первом прыжке
- [ ] Особые фразы при двойном прыжке
- [ ] Пузырь правильно позиционируется
- [ ] Текст центрирован в пузыре
- [ ] Fade out эффект работает плавно
- [ ] Хвост указывает на игрока
- [ ] Разные фразы появляются случайно

### Визуал:
- [ ] Пузырь не перекрывает игрока
- [ ] Текст читаемый
- [ ] Обводка чёткая
- [ ] Закругления smooth
- [ ] Прозрачность корректная

---

## Future Improvements

### Потенциальные улучшения:

#### Двойной прыжок:
- [ ] Тройной прыжок как power-up
- [ ] Wall jump (прыжок от стен)
- [ ] Variable jump height (сила зависит от нажатия)
- [ ] Air dash (рывок в воздухе)

#### Комикс-фразы:
- [ ] Фразы при столкновении с врагами
- [ ] Фразы при сборе power-ups
- [ ] Фразы при достижении рекорда
- [ ] Контекстные фразы по погоде
- [ ] Эмоции персонажа (смайлики)

#### Визуал:
- [ ] Анимированные пузыри (пульсация)
- [ ] Разные цвета для разных фраз
- [ ] Particle effects при появлении
- [ ] Sound effects для фраз
- [ ] Motion lines при быстром движении

---

## Code Quality

### Best Practices Applied:
✅ Separation of concerns (отдельные методы для логики и отрисовки)  
✅ DRY principle (переиспользование кода)  
✅ Single responsibility (каждый метод делает одну вещь)  
✅ Clear naming (понятные имена переменных)  
✅ Comments (документирование сложной логики)  

### Performance Considerations:
✅ Timer-based cleanup (автоматическая очистка фраз)  
✅ Minimal state (только необходимые свойства)  
✅ Efficient rendering (рисуем только когда нужно)  
✅ No memory leaks (garbage collection friendly)  

---

## Player Psychology

### Joy of Movement (Радость движения)
- Двойной прыжок даёт чувство свободы
- Игрок чувствует контроль над персонажем
- Mastery через практику timing

### Personality Connection (Связь с персонажем)
- Фразы создают эмоциональную связь
- Игрок "слышит" голос персонажа
- Юмор делает игру запоминающейся

### Achievement Satisfaction (Удовлетворение достижений)
- Достижение высоких бонусов = радость
- Особые фразы = награда за навык
- Progression через mastery механик

---

## Comparison: Before vs After

### До обновления:
- ❌ Один прыжок
- ❌ Безликий персонаж
- ❌ Нет визуальной обратной связи
- ❌ Ограниченная вертикальность
- ❌ Скучный геймплей

### После обновления:
- ✅ Двойной прыжок
- ✅ Персонаж с характером
- ✅ Комикс-фразы при прыжках
- ✅ Доступ к высоким зонам
- ✅ Динамичный геймплей
- ✅ Эмоциональная связь
- ✅ Skill expression

---

**Enjoy the enhanced mobility and personality! 🦘💬✨**

*Added: 2026-04-19*  
*Version: 1.5.0 (Double Jump & Comic Phrases Update)*