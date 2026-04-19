# Обновление: Реальная погода с API и поиск городов 🌍

## Что нового (2026-04-19)

### ✅ Основные улучшения

1. **Интеграция реального API ключа**
   - Подключён API ключ OpenWeatherMap
   - Получение точных данных о погоде в реальном времени
   - Поддержка русского языка в описании погоды

2. **Отображение названия города**
   - В HUD показывается текущий город
   - На стартовом экране отображается выбранный город
   - Формат: "Москва 20°C ☀️"

3. **Ручной поиск города**
   - Поле ввода для названия города
   - Кнопка "🔍 Найти город"
   - Поддержка Enter для быстрого поиска
   - Обработка ошибок (город не найден)

4. **Выбор способа получения погоды**
   - 📍 **Моё местоположение** - автоматическое определение через геолокацию
   - 🔍 **Поиск по городу** - ручной ввод названия
   - 🎲 **Случайная погода** - fallback режим без API

---

## Как использовать

### Способ 1: Поиск по городу (Рекомендуется)

1. Откройте игру
2. Введите название города в поле ввода:
   - `Москва`
   - `Санкт-Петербург`
   - `London`
   - `New York`
3. Нажмите **"🔍 Найти город"** или клавишу **Enter**
4. Подождите загрузки данных (~1-2 секунды)
5. Нажмите **"Играть"** когда погода отобразится

### Способ 2: Геолокация

1. Откройте игру
2. Нажмите **"📍 Моё местоположение"**
3. Разрешите доступ к геолокации в браузере
4. Игра автоматически определит ваш город
5. Нажмите **"Играть"**

### Способ 3: Случайная погода

1. Откройте игру
2. Нажмите **"Играть со случайной погодой"**
3. Игра выберет случайную погоду
4. Мгновенный старт без ожидания

---

## Технические детали

### API Integration

**API Key:**
```javascript
this.API_KEY = '4158b1f227860e4af6d07780f070954b';
```

**Endpoints:**

1. **По координатам:**
```javascript
GET https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid={key}&units=metric&lang=ru
```

2. **По названию города:**
```javascript
GET https://api.openweathermap.org/data/2.5/weather?q={city}&appid={key}&units=metric&lang=ru
```

**Параметры:**
- `units=metric` - температура в Цельсиях
- `lang=ru` - описание погоды на русском языке

### WeatherService Methods

```javascript
// Получить погоду по координатам
async getCurrentWeatherByCoords(lat, lon)

// Получить погоду по названию города
async getCurrentWeatherByCity(cityName)

// Получить погоду через геолокацию (старый метод)
async getCurrentWeather()
```

### Response Data

```javascript
{
    temp: 20,                    // Температура (°C)
    weatherCode: 800,           // Код погоды WMO
    description: 'ясно',        // Описание на русском
    icon: '☀️',                  // Эмодзи иконка
    cityName: 'Москва'          // Название города
}
```

---

## UI Updates

### Start Screen

**Новые элементы:**
```html
<input type="text" id="cityInput" placeholder="Введите название города">
<button id="searchCityButton">🔍 Найти город</button>
<button id="useLocationButton">📍 Моё местоположение</button>
<div id="cityNameLarge">Москва</div>
```

### HUD

**Обновлённый формат:**
```
[☀️] [Москва] [20°C]          Score: 1234
```

---

## Error Handling

### Город не найден
```javascript
try {
    const data = await weatherService.getCurrentWeatherByCity(city);
} catch (error) {
    showNotification('Город не найден! Попробуйте ещё раз.');
}
```

### Геолокация отклонена
```javascript
try {
    const data = await weatherService.getCurrentWeather();
} catch (error) {
    showNotification('Не удалось определить местоположение');
}
```

### API недоступен
- Автоматический fallback на случайную погоду
- Консольное предупреждение для разработчиков

---

## Supported Cities

API OpenWeatherMap поддерживает **200,000+ городов** worldwide.

**Примеры:**
- Россия: Москва, Санкт-Петербург, Новосибирск, Екатеринбург
- Украина: Киев, Харьков, Одесса
- Беларусь: Минск, Гомель
- Казахстан: Алматы, Астана
- International: London, Paris, Berlin, New York, Tokyo

**Совет:** Используйте официальное название города на английском или родном языке.

---

## Privacy & Security

### Geolocation
- Запрашивается только при нажатии "Моё местоположение"
- Координаты не сохраняются
- Передаются только в OpenWeatherMap API
- Пользователь может отказать в доступе

### API Key
- Ключ встроен в клиентский код
- Для production рекомендуется использовать backend proxy
- Текущий ключ имеет лимит 60 запросов/минуту

### Data Storage
- Название города НЕ сохраняется между сессиями
- Каждый раз нужно вводить заново (privacy-friendly)

---

## Performance

### API Response Time
- Среднее время ответа: 200-500ms
- Зависит от местоположения пользователя
- Кэширование не используется (всегда свежие данные)

### Rate Limiting
- Free tier: 60 calls/minute
- Free tier: 1,000,000 calls/month
- Текущее использование: ~1 call per game start

---

## Future Improvements

### Planned Features
- [ ] Сохранение последнего города в localStorage
- [ ] Автодополнение названия города
- [ ] История поисков
- [ ] Избранные города
- [ ] Прогноз погоды на несколько дней
- [ ] Backend proxy для защиты API key

### Code Example: Save Last City
```javascript
// Сохранение
localStorage.setItem('lastCity', cityName);

// Загрузка
const lastCity = localStorage.getItem('lastCity');
if (lastCity) {
    document.getElementById('cityInput').value = lastCity;
}
```

---

## Troubleshooting

### Q: API не работает
**A:** Проверьте:
1. Интернет соединение
2. API key активен (проверьте на openweathermap.org)
3. Не превышен лимит запросов

### Q: Город не находится
**A:** Попробуйте:
1. Проверить правильное написание
2. Использовать английское название
3. Указать страну: "London, UK"

### Q: Геолокация не работает
**A:** 
1. Проверьте настройки браузера
2. Убедитесь что используете HTTPS
3. Разрешите доступ к геолокации

### Q: Температура не соответствует реальности
**A:** 
1. API показывает текущую температуру на метеостанции
2. Может отличаться от ощущаемой температуры
3. Данные обновляются каждые 10-20 минут

---

## Testing Checklist

- [ ] Поиск города работает
- [ ] Отображается название города в HUD
- [ ] Геолокация определяет город правильно
- [ ] Случайная погода работает как fallback
- [ ] Ошибки обрабатываются корректно
- [ ] Русский язык в описании погоды
- [ ] Enter работает для поиска
- [ ] Input field стилизован правильно

---

## API Documentation

### OpenWeatherMap Current Weather

**Base URL:** `https://api.openweathermap.org/data/2.5/weather`

**Parameters:**
| Parameter | Required | Description |
|-----------|----------|-------------|
| q | Optional* | City name |
| lat | Optional* | Latitude |
| lon | Optional* | Longitude |
| appid | Yes | API key |
| units | No | metric/imperial |
| lang | No | Language code |

*\* Either (q) OR (lat + lon) is required*

**Response Example:**
```json
{
    "name": "Moscow",
    "main": {
        "temp": 20.5
    },
    "weather": [
        {
            "id": 800,
            "description": "ясно"
        }
    ]
}
```

---

**Enjoy accurate weather in your game! 🌤️🎮**

*Added: 2026-04-19*  
*Version: 1.2.0 (Real Weather Update)*