# tilda-products-1.0.min.js

Модуль обработки товаров на страницах Tilda (не в каталоге). Управляет ценами товаров с опциями в обычных блоках страницы.

## Основные функции

### `t_prod__init(recId)`

Инициализирует все товары в блоке по recId.

```javascript
// Автоматически вызывается для блоков с товарами
t_prod__init(123456789);
```

### `t_prod__initProduct(productEl)`

Инициализирует один товар.

```javascript
var productEl = document.querySelector('.js-product');
t_prod__initProduct(productEl);
```

### `t_prod__initPrice(productEl)`

Инициализирует базовую цену товара. Сохраняет начальную цену в data-атрибуты.

```javascript
t_prod__initPrice(productEl);

// После инициализации элемент .js-product-price будет иметь:
// data-product-price-def="1500"        (числовое значение)
// data-product-price-def-str="1 500 ₽"  (форматированная строка)
```

### `t_prod__updatePrice(productEl)`

Пересчитывает и обновляет цену товара с учётом выбранных опций.

```javascript
// Вызывается автоматически при изменении опций
t_prod__updatePrice(productEl);
```

### `t_prod__addEvents__options(productEl)`

Добавляет обработчики событий на опции товара (select, checkbox).

```javascript
t_prod__addEvents__options(productEl);
```

### `t_prod__showPrice(price, options)`

Форматирует цену для отображения (разделители тысяч, десятичные).

```javascript
var formatted = t_prod__showPrice(1500.50);
// "1 500,50"

// С опциями валюты
var formatted = t_prod__showPrice(1500, {
    currencyDecimal: "00",      // "00" - без копеек
    currencySeparator: "."      // "." - точка как разделитель
});
// "1 500.00"
```

### `t_prod__cleanPrice(priceString)`

Очищает строку цены, извлекая числовое значение.

```javascript
var num = t_prod__cleanPrice("1 500,50 ₽");
// 1500.50
```

### `t_prod__roundPrice(price)`

Округляет цену до 2 знаков после запятой.

```javascript
var rounded = t_prod__roundPrice(99.999);
// 100.00
```

### `t_prod__saveUserInputInPrice(price, formattedPrice, currentText)`

Сохраняет пользовательское форматирование цены при обновлении.

## События

| Событие       | Элемент                    | Описание                                            |
| -------------------- | --------------------------------- | ----------------------------------------------------------- |
| `change`           | `.js-product-option-variants`   | Изменение опции (select)                      |
| `change`           | `.js-product-multioption input` | Изменение мультиопции (checkbox)        |
| `twishlist_addbtn` | `document.body`                 | Триггерится при изменении опций |

## CSS классы

| Класс                      | Описание                          |
| ------------------------------- | ----------------------------------------- |
| `.js-product`                 | Контейнер товара           |
| `.js-product-price`           | Элемент с ценой              |
| `.js-product-option`          | Опция товара                   |
| `.js-product-multioption`     | Мультиопция (checkbox)         |
| `.js-product-option-variants` | Select с вариантами опции |

## Data-атрибуты

| Атрибут                 | Описание                                 |
| ------------------------------ | ------------------------------------------------ |
| `data-product-price-def`     | Базовая цена (число)             |
| `data-product-price-def-str` | Базовая цена (строка)           |
| `data-product-option-price`  | Дополнительная цена опции |

## Примеры использования

### HTML структура товара

```html
<div class="js-product">
    <div class="js-product-price"
         data-product-price-def="1500"
         data-product-price-def-str="1 500 ₽">
        1 500 ₽
    </div>

    <!-- Опция с вариантами -->
    <select class="js-product-option-variants">
        <option value="S" data-product-option-price="0">S</option>
        <option value="M" data-product-option-price="100">M (+100 ₽)</option>
        <option value="L" data-product-option-price="200">L (+200 ₽)</option>
    </select>

    <!-- Мультиопция -->
    <div class="js-product-multioption">
        <input type="checkbox" data-product-option-price="500"> Подарочная упаковка
    </div>
</div>
```

### Программное обновление цены

```javascript
// Получить товар
var product = document.querySelector('.js-product');

// Изменить базовую цену программно
var priceEl = product.querySelector('.js-product-price');
priceEl.setAttribute('data-product-price-def', '2000');
priceEl.setAttribute('data-product-price-def-str', '2 000 ₽');

// Пересчитать цену с учётом опций
t_prod__updatePrice(product);
```

### Отслеживание изменения цены

```javascript
// Перехват обновления цены
var originalUpdatePrice = t_prod__updatePrice;
t_prod__updatePrice = function(productEl) {
    var result = originalUpdatePrice.apply(this, arguments);

    var priceEl = productEl.querySelector('.js-product-price');
    console.log('Новая цена:', priceEl.textContent);

    return result;
};
```

### Кастомное форматирование цены

```javascript
// Переопределить форматирование
var originalShowPrice = t_prod__showPrice;
t_prod__showPrice = function(price, options) {
    var formatted = originalShowPrice.apply(this, arguments);

    // Добавить валюту, если её нет
    if (formatted && formatted.indexOf('₽') === -1) {
        formatted += ' ₽';
    }

    return formatted;
};
```

## Интеграция с корзиной (window.tcart)

Модуль использует настройки из `window.tcart`:

```javascript
// Настройки форматирования валюты
window.tcart = {
    currency_dec: "00",    // Десятичные знаки ("00" - без копеек)
    currency_sep: "."      // Разделитель ("." или ",")
};
```
