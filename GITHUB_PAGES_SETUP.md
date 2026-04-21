# 🌐 Настройка GitHub Pages для Weather Runner

## Что такое GitHub Pages?

**GitHub Pages** - это бесплатный хостинг статических сайтов от GitHub. Ваш игра будет доступна по ссылке вида:
```
https://jaroslaveagle.github.io/Weather-Runner/
```

**Преимущества:**
- ✅ **Бесплатно** навсегда
- ✅ **SSL сертификат** (HTTPS) автоматически
- ✅ **CDN** - быстрая загрузка по всему миру
- ✅ **Автоматический деплой** при push в репозиторий
- ✅ **Неограниченное количество игроков** (каждый играет свою копию)

---

## 🚀 Пошаговая настройка

### Шаг 1: Включите GitHub Pages

1. Откройте ваш репозиторий: https://github.com/JaroslavEagle/Weather-Runner

2. Перейдите в **Settings** (Настройки)

3. В левом меню найдите **Pages**

4. В разделе **Source** выберите:
   - Branch: **main**
   - Folder: **/(root)**

5. Нажмите **Save**

6. Подождите 1-2 минуты

7. GitHub покажет ссылку вида:
   ```
   https://jaroslaveagle.github.io/Weather-Runner/
   ```

---

### Шаг 2: Проверьте работу

Откройте полученную ссылку в браузере. Игра должна загрузиться и работать!

**Что проверить:**
- [ ] Игра загружается
- [ ] Canvas отображается
- [ ] Кнопки работают
- [ ] Погода определяется
- [ ] Можно играть

---

### Шаг 3: Поделитесь ссылкой

Теперь вы можете отправить ссылку друзьям:

```
🎮 Weather Runner - Беги под реальной погодой!
https://jaroslaveagle.github.io/Weather-Runner/

Особенности:
✅ Реальная погода в вашем городе
✅ Двойной прыжок и комикс-фразы
✅ Враги и power-ups
✅ Работает на ПК и мобильных
```

---

## 📊 Как это работает для нескольких игроков

### Сценарий использования:

```
Вы отправляете ссылку 10 друзьям
         │
         ▼
┌────────────────────────────────┐
│  Каждый открывает ссылку       │
│  в своём браузере              │
└────────────────────────────────┘
         │
         ▼
┌────────────────────────────────┐
│  GitHub Pages отдаёт файлы     │
│  каждому независимо            │
└────────────────────────────────┘
         │
         ▼
    ┌────┴────┬─────────┬──────┐
    ▼         ▼         ▼      ▼
 Игрок 1  Игрок 2  Игрок 3 ... Игрок 10
    │         │         │           │
    ▼         ▼         ▼           ▼
 [Своя    [Своя    [Своя       [Своя
  копия]   копия]   копия]      копия]
    │         │         │           │
    ▼         ▼         ▼           ▼
 Score:   Score:   Score:      Score:
 1500     2300      800        3100
```

**Каждый игрок:**
- ✅ Видит только свой экран
- ✅ Играет независимо
- ✅ Имеет свой localStorage (High Score)
- ✅ Может выбрать свой город
- ✅ Не видит других игроков

---

## ⚠️ Ограничения текущего подхода

### Что НЕ возможно без backend:

❌ **Нельзя видеть других игроков в реальном времени**  
❌ **Нельзя соревноваться одновременно**  
❌ **Нельзя чатиться**  
❌ **High Score только локальный** (не глобальный)  

### Что возможно сейчас:

✅ **Каждый играет бесплатно по ссылке**  
✅ **Можно сравнивать рекорды口头но** ("У меня 3000, а у тебя?")  
✅ **Работает на любом устройстве**  
✅ **Не нужно ничего устанавливать**  

---

## 🎯 Улучшение: Глобальная таблица лидеров

Если хотите добавить **глобальный leaderboard**, вот простой способ с Firebase:

### 1. Создайте Firebase проект

1. Перейдите: https://console.firebase.google.com/
2. Нажмите "Add project"
3. Назовите: "weather-runner-leaderboard"
4. Отключите Google Analytics (не нужно)
5. Нажмите "Create project"

### 2. Создайте Realtime Database

1. В меню слева выберите **Build** → **Realtime Database**
2. Нажмите **Create Database**
3. Выберите location (us-central1)
4. Выберите **Start in test mode** (для начала)
5. Нажмите **Enable**

### 3. Получите конфигурацию

1. Project Settings (шестерёнка) → General
2. Прокрутите до "Your apps"
3. Нажмите иконку web `</>`
4. Зарегистрируйте app (название: "Weather Runner")
5. Скопируйте `firebaseConfig`

### 4. Добавьте код в index.html

Перед закрывающим тегом `</body>` добавьте:

```html
<!-- Firebase SDK -->
<script type="module">
    import { initializeApp } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js';
    import { getDatabase, ref, set, onValue, query, orderByChild, limitToLast } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js';

    const firebaseConfig = {
        apiKey: "YOUR_API_KEY",
        authDomain: "your-project.firebaseapp.com",
        databaseURL: "https://your-project-default-rtdb.firebaseio.com",
        projectId: "your-project",
        storageBucket: "your-project.appspot.com",
        messagingSenderId: "123456789",
        appId: "1:123456789:web:abcdef"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    // Сохранение рекорда
    window.saveScoreToLeaderboard = function(name, score) {
        const newScoreRef = push(ref(db, 'scores'));
        set(newScoreRef, {
            name: name || 'Anonymous',
            score: score,
            timestamp: Date.now()
        });
    };

    // Загрузка топ-10
    window.getTopScores = function(callback) {
        const scoresRef = query(
            ref(db, 'scores'),
            orderByChild('score'),
            limitToLast(10)
        );
        
        onValue(scoresRef, (snapshot) => {
            const scores = [];
            snapshot.forEach((childSnapshot) => {
                scores.push(childSnapshot.val());
            });
            callback(scores.reverse()); // Sort descending
        });
    };
</script>
```

### 5. Обновите gameOver экран

Добавьте input для имени и кнопку сохранения:

```html
<div class="overlay hidden" id="gameOverScreen">
    <h1>Game Over!</h1>
    <div>Score: <span id="finalScore">0</span></div>
    
    <!-- Добавлено: Input для имени -->
    <div style="margin: 20px 0;">
        <input type="text" id="playerName" placeholder="Ваше имя" 
               maxlength="20" style="padding: 10px; font-size: 16px;">
        <button id="saveScoreButton" style="margin-left: 10px;">
            💾 Сохранить рекорд
        </button>
    </div>
    
    <!-- Добавлено: Таблица лидеров -->
    <div id="leaderboard" style="margin: 20px 0; max-height: 200px; overflow-y: auto;">
        <h3>🏆 Топ игроков:</h3>
        <div id="leaderboardList">Загрузка...</div>
    </div>
    
    <button id="restartButton">🔄 Играть снова</button>
    <button id="menuButton">🏠 В меню</button>
</div>
```

### 6. Добавьте обработчики

```javascript
// В setupEventListeners()
document.getElementById('saveScoreButton').addEventListener('click', () => {
    const name = document.getElementById('playerName').value || 'Anonymous';
    const score = Math.floor(this.score / 10);
    
    if (typeof saveScoreToLeaderboard === 'function') {
        saveScoreToLeaderboard(name, score);
        this.showNotification('Рекорд сохранён!');
    }
});

// Показать leaderboard при game over
gameOver() {
    
    if (typeof getTopScores === 'function') {
        getTopScores((scores) => {
            const list = document.getElementById('leaderboardList');
            if (scores.length === 0) {
                list.innerHTML = '<p>Пока нет рекордов. Будьте первым!</p>';
            } else {
                list.innerHTML = scores.map((s, i) => 
                    `<div>${i + 1}. ${s.name}: ${s.score}</div>`
                ).join('');
            }
        });
    }
}
```

---

## 📈 Сравнение подходов

| Функция | Только GitHub Pages | + Firebase Leaderboard | Полноценный сервер |
|---------|---------------------|------------------------|-------------------|
| Стоимость | Бесплатно | Бесплатно* | $5-10/мес |
| Сложность | Очень легко | Легко | Сложно |
| Multiplayer | ❌ Нет | ❌ Нет | ✅ Да |
| Leaderboard | ❌ Локальный | ✅ Глобальный | ✅ Глобальный |
| Real-time | ❌ Нет | ❌ Нет | ✅ Да |
| Chat | ❌ Нет | ❌ Нет | ✅ Да |
| Настройка | 5 минут | 30 минут | Несколько дней |

\* Firebase бесплатен до определённых лимитов (более чем достаточно для инди-игры)

---

## 🎮 Рекомендация

### Для начала:
1. ✅ **Настройте GitHub Pages** (5 минут)
2. ✅ **Поделитесь ссылкой** с друзьями
3. ✅ **Соберите feedback**

### Если понравится:
4. ➕ **Добавьте Firebase Leaderboard** (30 минут)
5. ➕ **Получите глобальную таблицу рекордов**

### Если нужно больше:
6. 🚀 **Создайте полноценный сервер** с WebSocket
7. 🚀 **Добавьте real-time multiplayer**

---

## 🔗 Полезные ссылки

- **GitHub Pages Docs**: https://pages.github.com/
- **Firebase Console**: https://console.firebase.google.com/
- **Firebase Pricing**: https://firebase.google.com/pricing (бесплатный тариф щедрый!)
- **Ваш репозиторий**: https://github.com/JaroslavEagle/Weather-Runner

---

## ❓ FAQ

**Q: Сколько игроков могут играть одновременно?**  
A: Неограниченное количество! Каждый получает свою копию игры.

**Q: Нужно ли платить за GitHub Pages?**  
A: Нет, полностью бесплатно для публичных репозиториев.

**Q: Будет ли работать на мобильных?**  
A: Да! GitHub Pages отдаёт те же файлы, что и локально.

**Q: Как часто обновляется страница?**  
A: При каждом push в ветку main обновление происходит за 1-2 минуты.

**Q: Можно использовать свой домен?**  
A: Да! GitHub Pages поддерживает кастомные домены.

---

**Начните с GitHub Pages - это быстро, бесплатно и просто! 🚀**

*Created: 2026-04-19*