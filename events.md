# tilda-events-1.0.min.js

Модуль аналитики и событий Tilda. Интегрирует Google Analytics, Яндекс.Метрику, Facebook Pixel, VK Pixel, Mail.ru и внутреннюю статистику Tilda. Отправляет e-commerce события.

## Глобальные переменные

| Переменная               | Тип  | Описание                                                           |
| ---------------------------------- | ------- | -------------------------------------------------------------------------- |
| `window.Tilda`                   | Object  | Основной объект Tilda с методами аналитики |
| `window.dataLayer`               | Array   | Google Tag Manager dataLayer                                               |
| `window._tmr`                    | Array   | Mail.ru Top-100 tracker                                                    |
| `window.mainTracker`             | String  | Основной трекер ("gtag", "tilda", etc.)                      |
| `window.gtagTrackerID`           | String  | ID Google Analytics (UA-xxx или G-xxx)                                  |
| `window.mainMetrikaId`           | Number  | ID Яндекс.Метрики                                             |
| `window.mainMetrika`             | String  | Имя объекта Метрики                                       |
| `window.mainMailruId`            | String  | ID Mail.ru счётчика                                                |
| `window.tildaForm`               | Object  | Данные формы для статистики                        |
| `window.Tilda.isLoadGAEcommerce` | Boolean | Загружен ли GA E-commerce                                        |

## Основные функции (window.Tilda.*)

### `Tilda.sendEcommerceEvent(action, products)`

Отправляет e-commerce событие во все подключённые системы аналитики.

**Параметры:**

- `action` - тип события: `"add"`, `"remove"`, `"purchase"`, `"detail"`
- `products` - массив товаров

```javascript
// Добавление в корзину
Tilda.sendEcommerceEvent("add", [{
    name: "Название товара",
    price: 1500,
    quantity: 1,
    id: "product-123",      // или uid
    uid: "unique-id",
    sku: "ART-001",
    recid: 123456789,       // ID блока
    lid: 1,                 // ID листинга
    options: [
        { option: "Цвет", variant: "Красный" }
    ]
}]);

// Удаление из корзины
Tilda.sendEcommerceEvent("remove", [{
    name: "Название товара",
    price: 1500,
    quantity: 1
}]);

// Просмотр товара
Tilda.sendEcommerceEvent("detail", [{
    name: "Название товара",
    price: 1500,
    id: "product-123"
}]);

// Покупка
Tilda.sendEcommerceEvent("purchase", [{
    name: "Товар 1",
    price: 1500,
    quantity: 2
}, {
    name: "Товар 2",
    price: 2000,
    quantity: 1
}]);
```

### `Tilda.sendEventToStatistics(path, title, data, value)`

Отправляет произвольное событие во все системы аналитики.

**Параметры:**

- `path` - путь события (начинается с "/" для pageview, иначе - event)
- `title` - заголовок/метка события
- `data` - дополнительные данные (опционально)
- `value` - числовое значение (опционально)

```javascript
// Отправка pageview события
Tilda.sendEventToStatistics("/tilda/click/rec123456789/", "Клик по кнопке");

// Отправка goal/event
Tilda.sendEventToStatistics("button_click", "Заказать звонок", null, 0);

// Событие с e-commerce данными
Tilda.sendEventToStatistics("/tilda/cart/add/123456789", "Товар добавлен", {
    ecommerce: {
        add: {
            products: [{
                name: "Товар",
                price: 1000,
                quantity: 1
            }]
        }
    }
}, 1000);

// Событие покупки
Tilda.sendEventToStatistics("/tilda/rec123456789/payment/", "Оплата", {
    ecommerce: {
        purchase: {
            actionField: {
                id: "ORDER-123",
                revenue: 5000
            },
            products: [...]
        }
    }
}, 5000);
```

### `Tilda.saveUTM()`

Сохраняет UTM-метки из URL в cookie `TILDAUTM` на 30 дней.

```javascript
// Автоматически вызывается при загрузке страницы
// Парсит utm_source, utm_medium, utm_campaign, etc.
Tilda.saveUTM();

// Сохраняет в cookie формат: utm_source=google|||utm_medium=cpc|||
```

## Поддерживаемые системы аналитики

### Google Analytics 4 (gtag)

```javascript
// Автоматически отправляется:
gtag("event", "add_to_cart", { items: [...] });
gtag("event", "remove_from_cart", { items: [...] });
gtag("event", "view_item", { items: [...] });
gtag("event", "purchase", {
    transaction_id: "...",
    value: 5000,
    currency: "RUB",
    shipping: {...},
    items: [...]
});
```

### Universal Analytics (ga)

```javascript
// Автоматически загружается Enhanced Ecommerce:
ga("require", "ec");
ga("ec:addProduct", {...});
ga("ec:setAction", "add");
ga("send", "pageview");
```

### Яндекс.Метрика

```javascript
// Автоматически отправляется:
ym(COUNTER_ID, "hit", "/path", { title: "...", params: {...} });
ym(COUNTER_ID, "reachGoal", "goal_name", { order_price: 5000, currency: "RUB" });
```

### Facebook Pixel

```javascript
// Автоматически отправляется:
fbq("track", "AddToCart", {
    content_ids: ["id1", "id2"],
    content_type: "product",
    value: 1500,
    currency: "RUB"
});
fbq("track", "ViewContent", {...});
fbq("track", "InitiateCheckout", {...});
fbq("track", "Lead", {...});
```

### VK Pixel

```javascript
// Автоматически отправляется:
VK.Retargeting.Event("add_to_cart");
VK.Retargeting.ProductEvent(priceListId, "add_to_cart", {
    products: [{ id: "...", price: 1500 }],
    currency_code: "RUB",
    total_price: 1500
});
VK.Goal("purchase");
VK.Goal("lead");
```

### Mail.ru Top-100

```javascript
// Автоматически отправляется в _tmr:
_tmr.push({
    id: "COUNTER_ID",
    type: "pageView",
    url: "/path",
    value: 5000
});
_tmr.push({
    id: "COUNTER_ID",
    type: "reachGoal",
    goal: "goal_name",
    value: 5000
});
```

### Google Tag Manager (dataLayer)

```javascript
// Автоматически пушится в dataLayer:
dataLayer.push({
    event: "addToCart",
    ecommerce: {
        add: {
            products: [...]
        }
    }
});

dataLayer.push({
    event: "purchase",
    ecommerce: {
        purchase: {
            actionField: { id: "...", revenue: 5000 },
            products: [...]
        }
    }
});

dataLayer.push({
    event: "pageView",
    eventAction: "/tilda/click/...",
    title: "...",
    value: 0
});
```

## События E-commerce

| Событие | Описание                       | GA4 Event            | Метрика    |
| -------------- | -------------------------------------- | -------------------- | ----------------- |
| `add`        | Добавление в корзину | `add_to_cart`      | hit + reachGoal   |
| `remove`     | Удаление из корзины   | `remove_from_cart` | hit               |
| `detail`     | Просмотр товара          | `view_item`        | hit               |
| `purchase`   | Покупка                         | `purchase`         | hit + order_price |

## Data-атрибуты

| Атрибут                 | Элемент  | Описание                           |
| ------------------------------ | --------------- | ------------------------------------------ |
| `data-tilda-cookie`          | #allrecords     | "no" - не сохранять UTM         |
| `data-fb-event`              | #allrecords     | "nosend" - не отправлять в FB |
| `data-vk-event`              | #allrecords     | "nosend" - не отправлять в VK |
| `data-vk-price-list-id`      | #allrecords     | ID прайс-листа VK                |
| `data-tilda-currency`        | #allrecords     | Код валюты (RUB, USD)             |
| `data-project-currency-code` | .t706           | Код валюты проекта         |
| `data-tilda-event-name`      | a.js-click-stat | Кастомное имя события   |

## CSS классы для отслеживания

| Класс              | Описание                                        |
| ----------------------- | ------------------------------------------------------- |
| `.js-click-stat`      | Ссылка с отслеживанием кликов |
| `.js-click-zero-stat` | Zero-block элемент с отслеживанием |

## Примеры использования

### Отслеживание кастомных событий

```javascript
// Отправка события при клике на кнопку
document.querySelector('.my-button').addEventListener('click', function() {
    Tilda.sendEventToStatistics("custom_button_click", "Моя кнопка", null, 0);
});
```

### Отслеживание добавления в корзину

```javascript
// Перехват добавления товара
var originalAddProduct = tcart__addProduct;
tcart__addProduct = function(product) {
    // Отправляем в свою аналитику
    Tilda.sendEcommerceEvent("add", [{
        name: product.name,
        price: product.price,
        quantity: product.quantity || 1,
        id: product.uid || product.sku
    }]);

    return originalAddProduct.apply(this, arguments);
};
```

### Отправка события покупки

```javascript
// После успешной оплаты
function onPaymentSuccess(orderData) {
    var products = orderData.items.map(function(item) {
        return {
            name: item.name,
            price: item.price,
            quantity: item.quantity,
            id: item.id
        };
    });

    Tilda.sendEventToStatistics(
        "/tilda/rec" + orderData.recId + "/payment/",
        "Покупка",
        {
            ecommerce: {
                purchase: {
                    actionField: {
                        id: orderData.orderId,
                        revenue: orderData.total
                    },
                    products: products
                }
            }
        },
        orderData.total
    );
}
```

### Отслеживание просмотра товара

```javascript
// При открытии popup товара
document.addEventListener('popupShowed', function() {
    var popup = document.querySelector('.t-store__prod-popup__container');
    if (!popup) return;

    var productName = popup.querySelector('.t-store__prod-popup__name')?.textContent;
    var productPrice = popup.querySelector('.t-store__prod-popup__price')?.textContent;

    Tilda.sendEcommerceEvent("detail", [{
        name: productName,
        price: parseFloat(productPrice.replace(/[^\d.]/g, '')) || 0
    }]);
});
```

### Получение UTM-меток из cookie

```javascript
function getUTMFromCookie() {
    var cookie = document.cookie.match(/TILDAUTM=([^;]+)/);
    if (!cookie) return {};

    var utm = {};
    var params = decodeURIComponent(cookie[1]).split('|||');
    params.forEach(function(param) {
        var parts = param.split('=');
        if (parts[0] && parts[1]) {
            utm[parts[0]] = parts[1];
        }
    });
    return utm;
}

// Использование
var utmData = getUTMFromCookie();
console.log(utmData.utm_source); // "google"
console.log(utmData.utm_medium); // "cpc"
```

### Отключение отправки в Facebook/VK

```html
<!-- Добавить атрибут к #allrecords -->
<div id="allrecords" data-fb-event="nosend" data-vk-event="nosend">
    ...
</div>
```

### Кастомная интеграция с GTM

```javascript
// Подписка на события dataLayer
window.dataLayer = window.dataLayer || [];

// Proxy для отслеживания push
var originalPush = window.dataLayer.push;
window.dataLayer.push = function() {
    var result = originalPush.apply(this, arguments);

    // Обработка события
    var data = arguments[0];
    if (data && data.event === 'addToCart') {
        console.log('Товар добавлен:', data.ecommerce.add.products);
    }

    return result;
};
```

## Структура объекта tildaForm (для покупок)

```javascript
window.tildaForm = {
    orderIdForStat: "ORDER-123456",      // ID заказа
    amountForStat: 5000,                  // Сумма заказа
    arProductsForStat: [                  // Массив товаров
        {
            name: "Товар 1",
            price: 1500,
            quantity: 2,
            id: "product-1"
        }
    ],
    tildapayment: {
        promocode: "SALE10"               // Промокод (если есть)
    }
};
```
