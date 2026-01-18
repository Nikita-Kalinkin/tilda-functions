# tilda-cart-1.1.min.js

Модуль корзины Tilda. Управляет добавлением товаров, отображением корзины, оформлением заказа, авторизацией и доставкой.

## Глобальные переменные

| Переменная             | Тип  | Описание                                                                   |
| -------------------------------- | ------- | ---------------------------------------------------------------------------------- |
| `window.tcart`                 | Object  | Основной объект корзины с товарами и суммами |
| `window.tcart.products`        | Array   | Массив товаров в корзине                                      |
| `window.tcart.prodamount`      | Number  | Сумма товаров без доставки                                  |
| `window.tcart.amount`          | Number  | Итоговая сумма с доставкой                                  |
| `window.tcart.total`           | Number  | Количество товаров                                                |
| `window.tcart.delivery`        | Object  | Информация о доставке                                           |
| `window.tcart.system`          | String  | Выбранная платёжная система                               |
| `window.tcart_minorder`        | Number  | Минимальная сумма заказа                                     |
| `window.tcart_mincntorder`     | Number  | Минимальное количество товаров                         |
| `window.tcart_fullscreen`      | Boolean | Полноэкранный режим корзины                               |
| `window.tcart_oneproduct`      | String  | Режим одного товара ("y"/"n")                                     |
| `window.tcart_dontstore`       | String  | Не сохранять в localStorage ("y"/"n")                                  |
| `window.tcart_maxstoredays`    | Number  | Макс. дней хранения корзины                                 |
| `window.tcart_isMobile`        | Boolean | Мобильное устройство                                            |
| `window.tcart_sendevent_onadd` | String  | Отправлять событие при добавлении ("y"/"n")          |
| `window.t_cart__discounts`     | Array   | Массив скидок                                                          |

## Основные функции

### Управление корзиной

#### `tcart__init()`

Инициализация корзины. Вызывается автоматически.

#### `tcart__openCart(restoreMode)`

Открывает корзину.

```javascript
// Пример: открыть корзину программно
tcart__openCart();

// Открыть в режиме восстановления
tcart__openCart(true);
```

#### `tcart__closeCart()`

Закрывает корзину (универсальный метод).

```javascript
tcart__closeCart();
```

#### `tcart__openCartSidebar(restoreMode)`

Открывает боковую панель корзины (fullscreen режим).

#### `tcart__closeCartSidebar()`

Закрывает боковую панель.

#### `tcart__openCartFullscreen()`

Открывает полноэкранную корзину.

#### `tcart__closeCartFullscreen()`

Закрывает полноэкранную корзину.

### Работа с товарами

#### `tcart__addProduct(product)`

Добавляет товар в корзину.

```javascript
// Структура объекта товара
var product = {
    name: "Название товара",
    price: 1000,
    quantity: 1,
    sku: "ART-001",
    uid: "unique-id-123",
    img: "https://example.com/image.jpg",
    url: "https://example.com/product",
    inv: 10, // остаток на складе
    unit: "PCE", // единица измерения
    portion: 0, // порция (для весовых товаров)
    options: [
        { option: "Цвет", variant: "Красный", price: 0 },
        { option: "Размер", variant: "XL", price: 100 }
    ]
};

tcart__addProduct(product);
```

#### `tcart__product__del(index)`

Удаляет товар из корзины по индексу.

```javascript
// Удалить первый товар
tcart__product__del(0);
```

#### `tcart__product__plus(index)`

Увеличивает количество товара на 1.

#### `tcart__product__minus(index)`

Уменьшает количество товара на 1.

#### `tcart__product__editquantity(index)`

Включает режим редактирования количества.

#### `tcart__product__updateQuantity(index, quantity)`

Обновляет количество товара.

```javascript
// Установить количество 5 для первого товара
tcart__product__updateQuantity(0, 5);
```

### Отрисовка и обновление

#### `tcart__reDrawProducts()`

Перерисовывает список товаров в корзине.

#### `tcart__reDrawTotal()`

Перерисовывает итоговую сумму.

#### `tcart__reDrawCartIcon()`

Обновляет иконку корзины (счётчик товаров).

#### `tcart__updateProductsPrice(force)`

Обновляет цены товаров (запрос к API).

```javascript
tcart__updateProductsPrice(true);
```

#### `tcart__updateTotalProductsinCartObj()`

Пересчитывает итоги в объекте корзины.

### Хранение данных

#### `tcart__loadLocalObj()`

Загружает корзину из localStorage.

#### `tcart__saveLocalObj()`

Сохраняет корзину в localStorage.

#### `tcart__syncProductsObject__LStoObj()`

Синхронизирует объект корзины с localStorage.

#### `tcart__nullObj()`

Возвращает пустой объект корзины.

```javascript
var emptyCart = tcart__nullObj();
// { products: [], prodamount: 0, amount: 0, system: "" }
```

### Скидки и промокоды

#### `tcart__calcAmountWithDiscounts()`

Рассчитывает сумму с учётом скидок.

#### `tcart__calcPromocode(amount)`

Применяет промокод к сумме.

```javascript
var finalAmount = tcart__calcPromocode(5000);
```

#### `tcart__loadDiscounts()`

Загружает скидки с сервера.

#### `tcart__getSubtotalDiscount()`

Получает скидку на подытог.

### Доставка

#### `tcart__initDelivery()`

Инициализирует модуль доставки.

#### `tcart__addDelivery()`

Добавляет информацию о доставке.

#### `tcart__updateDelivery()`

Обновляет данные доставки.

#### `tcart__processDelivery()`

Обрабатывает выбор способа доставки.

#### `tcart__setFreeDeliveryThreshold()`

Устанавливает порог бесплатной доставки.

```javascript
// Доступ к порогу
window.tcart.delivery.freedl = 5000;
```

#### `tcart__rerenderDeliveryServices()`

Перерисовывает варианты доставки.

### Авторизация

#### `tcart__auth__init()`

Инициализирует авторизацию. Возвращает Promise.

```javascript
tcart__auth__init().then(function() {
    console.log('Auth initialized');
});
```

#### `tcart__isAuthorized()`

Проверяет, авторизован ли пользователь.

```javascript
if (tcart__isAuthorized()) {
    // пользователь авторизован
}
```

#### `tcart__auth__getMauser()`

Получает данные пользователя. Возвращает Promise.

#### `tcart__auth__fillUserFields(userData)`

Заполняет поля формы данными пользователя.

```javascript
tcart__auth__fillUserFields({
    name: "Иван Иванов",
    email: "ivan@example.com",
    phone: "+79001234567"
});
```

#### `tcart__auth__clearUserFields()`

Очищает поля пользователя в форме.

### UI утилиты

#### `tcart__showBubble(message)`

Показывает всплывающее уведомление.

```javascript
tcart__showBubble("Товар добавлен в корзину!");
```

#### `tcart__closeBubble()`

Закрывает уведомление.

#### `tcart__showClearCartDialog()`

Показывает диалог очистки корзины.

#### `tcart__showWrongOrderPopup()`

Показывает popup с ошибкой заказа.

#### `tcart__preloader(show)`

Управляет прелоадером.

```javascript
tcart__preloader(true);  // показать
tcart__preloader(false); // скрыть
```

#### `tcart__lockScroll()`

Блокирует прокрутку страницы.

#### `tcart__unlockScroll()`

Разблокирует прокрутку страницы.

#### `tcart__blockCartUI()` / `tcart__unblockCartUI()`

Блокирует/разблокирует интерфейс корзины.

#### `tcart__blockSubmitButton()` / `tcart__unblockSubmitButton()`

Блокирует/разблокирует кнопку отправки.

### Вспомогательные функции

#### `tcart__showPrice(amount)`

Форматирует цену для отображения.

```javascript
var formatted = tcart__showPrice(1500); // "1 500 ₽"
```

#### `tcart__showWeight(weight, unit)`

Форматирует вес для отображения.

#### `tcart__roundPrice(price)`

Округляет цену.

```javascript
var rounded = tcart__roundPrice(99.999); // 100
```

#### `tcart__cleanPrice(priceString)`

Очищает строку цены от символов.

```javascript
var num = tcart__cleanPrice("1 500 ₽"); // 1500
```

#### `tcart__escapeHtml(string)`

Экранирует HTML-символы.

#### `tcart__decodeHtml(string)`

Декодирует HTML-сущности.

#### `tcart__isEmptyObject(obj)`

Проверяет, пустой ли объект.

#### `tcart__debounce(func, wait)`

Debounce функция.

#### `tcart__onFuncLoad(funcName, callback)`

Выполняет callback когда функция загружена.

```javascript
tcart__onFuncLoad("tcart__calcAmountWithDiscounts", function() {
    tcart__calcAmountWithDiscounts();
});
```

#### `tcart__getRootZone()`

Возвращает корневую зону CDN (com/ru).

## События (Events)

### События документа (document)

| Событие                | Описание                                           |
| ----------------------------- | ---------------------------------------------------------- |
| `tildaBuyerDashboardInited` | Инициализирован личный кабинет |
| `membersLogout`             | Пользователь вышел из системы    |
| `renderDeliveryServices`    | Отрисованы сервисы доставки       |
| `renderDeliveryAddresses`   | Отрисованы адреса доставки         |

### События body (document.body)

| Событие           | Описание                                         |
| ------------------------ | -------------------------------------------------------- |
| `popupShowed`          | Корзина/popup открыта                      |
| `popupHidden`          | Корзина/popup закрыта                      |
| `fullscreenCartOpened` | Полноэкранная корзина открыта |

### События формы

| Событие           | Описание                                      |
| ------------------------ | ----------------------------------------------------- |
| `tildaform:beforesend` | Перед отправкой формы заказа |
| `tildaform:aftererror` | После ошибки отправки формы   |

## Примеры использования

### Добавление товара программно

```javascript
document.addEventListener('DOMContentLoaded', function() {
    // Дождаться инициализации корзины
    tcart__onFuncLoad('tcart__addProduct', function() {
        var product = {
            name: "Тестовый товар",
            price: 1500,
            quantity: 2,
            sku: "TEST-001",
            uid: "test-uid-" + Date.now(),
            img: "",
            url: "",
            inv: 100,
            options: []
        };

        tcart__addProduct(product);
    });
});
```

### Отслеживание добавления в корзину

```javascript
// Переопределяем функцию показа bubble для отслеживания
var originalShowBubble = tcart__showBubble;
tcart__showBubble = function(message) {
    console.log('Товар добавлен:', message);

    // Отправка в аналитику
    if (window.dataLayer) {
        dataLayer.push({
            event: 'add_to_cart',
            message: message
        });
    }

    originalShowBubble(message);
};
```

### Отслеживание открытия/закрытия корзины

```javascript
document.body.addEventListener('popupShowed', function() {
    console.log('Корзина открыта');
    console.log('Товаров:', window.tcart.total);
    console.log('Сумма:', window.tcart.amount);
});

document.body.addEventListener('popupHidden', function() {
    console.log('Корзина закрыта');
});
```

### Программное открытие корзины

```javascript
// Открыть корзину по клику на кастомную кнопку
document.querySelector('.my-cart-button').addEventListener('click', function() {
    if (window.tcart && window.tcart.products.length > 0) {
        tcart__openCart();
    } else {
        alert('Корзина пуста');
    }
});
```

### Получение данных корзины

```javascript
function getCartData() {
    return {
        products: window.tcart.products,
        totalItems: window.tcart.total,
        subtotal: window.tcart.prodamount,
        total: window.tcart.amount,
        delivery: window.tcart.delivery,
        paymentSystem: window.tcart.system
    };
}

// Использование
var cartData = getCartData();
console.log('В корзине товаров:', cartData.totalItems);
console.log('Итого:', cartData.total);
```

### Очистка корзины программно

```javascript
function clearCart() {
    window.tcart = tcart__nullObj();
    tcart__saveLocalObj();
    tcart__reDrawCartIcon();

    // Если корзина открыта - перерисовать
    var cartWin = document.querySelector('.t706__cartwin_showed');
    if (cartWin) {
        tcart__reDrawProducts();
        tcart__reDrawTotal();
    }
}
```

### Мониторинг изменений корзины через localStorage

```javascript
window.addEventListener('storage', function(e) {
    if (e.key === 'tcart') {
        var newCart = JSON.parse(e.newValue);
        console.log('Корзина изменена в другой вкладке:', newCart);
    }
});
```

### Интеграция с внешней аналитикой

```javascript
// Отслеживание ecommerce событий
document.body.addEventListener('popupShowed', function() {
    // При открытии корзины
    if (window.tcart.products.length > 0) {
        var items = window.tcart.products.map(function(p) {
            return {
                item_name: p.name,
                item_id: p.sku || p.uid,
                price: p.price,
                quantity: p.quantity
            };
        });

        // Google Analytics 4
        if (typeof gtag === 'function') {
            gtag('event', 'view_cart', {
                currency: 'RUB',
                value: window.tcart.prodamount,
                items: items
            });
        }
    }
});
```

## Структура объекта товара

```javascript
{
    name: "Название товара",           // String, обязательно
    price: 1000,                       // Number, цена за единицу
    quantity: 1,                       // Number, количество
    amount: 1000,                      // Number, сумма (price * quantity)
    sku: "ART-001",                    // String, артикул
    uid: "unique-id",                  // String, уникальный ID
    img: "https://...",                // String, URL изображения
    url: "https://...",                // String, URL товара
    inv: 100,                          // Number, остаток на складе
    unit: "PCE",                       // String, единица измерения
    portion: 0,                        // Number, порция (для весовых)
    pack_m: 0,                         // Number, масса упаковки
    single: "n",                       // String, единичный товар ("y"/"n")
    ts: 1699999999,                    // Number, timestamp добавления
    options: [                         // Array, опции товара
        {
            option: "Цвет",
            variant: "Красный",
            price: 0
        }
    ],
    // Поля скидок (добавляются автоматически)
    discountid: "discount-1",          // ID примененной скидки
    amount_withdiscount: 900,          // Сумма со скидкой
    price_withdiscount: 900            // Цена со скидкой
}
```

## Единицы измерения (unit)

| Код  | Описание   |
| ------- | ------------------ |
| `PCE` | Штука         |
| `GRM` | Грамм         |
| `KGM` | Килограмм |
| `TNE` | Тонна         |
| `MTR` | Метр           |
| `CMT` | Сантиметр |
| `LTR` | Литр           |
| `MTK` | Кв. метр     |
