# tilda-catalog-1.1.min.js

Модуль каталога товаров Tilda. Управляет отображением карточек товаров, фильтрами, пагинацией, popup товара, галереями и вариантами (editions).

## Глобальные переменные

| Переменная                       | Тип         | Описание                                                      |
| ------------------------------------------ | -------------- | --------------------------------------------------------------------- |
| `window.tStoreInit`                      | Object         | Объект инициализации каталогов по recId |
| `window.tStoreDict`                      | Object         | Словарь переводов                                     |
| `window.tStoreBrowserLang`               | String         | Язык браузера                                             |
| `window.tStoreIsMobile`                  | Boolean        | Мобильное устройство                               |
| `window.tStoreIsSearchBot`               | Boolean        | Поисковый бот                                             |
| `window.t_store_endpoint`                | String         | Базовый URL API                                                |
| `window.tStoreFilters`                   | Object         | Активные фильтры                                       |
| `window.tStoreOptionsList`               | Object         | Список опций товаров                                |
| `window.tStoreDisabledOptionsList`       | Object         | Недоступные опции                                     |
| `window.tStoreDefaultSort`               | String         | Сортировка по умолчанию                          |
| `window.tStoreCustomUrlParams`           | Object         | Кастомные URL параметры                             |
| `window.tStoreProductsRequested`         | Boolean        | Запрос товаров выполнен                          |
| `window.tStoreSingleProdsObj`            | Object         | Объект одиночных товаров                        |
| `window.tStoreSingleProductsIsRequested` | Boolean        | Запрошены одиночные товары                    |
| `window.tStoreXHR`                       | XMLHttpRequest | Текущий XHR запрос                                       |
| `window.tStoreDefPackObj`                | Object         | Параметры упаковки по умолчанию           |
| `window.tStoreDetailEvent`               | String         | Событие открытия деталей                        |
| `window.urlBeforePopupOpen`              | String         | URL до открытия popup                                       |
| `window.titleBeforePopupOpen`            | String         | Title до открытия popup                                     |

## Константы событий

```javascript
t_store_POPUP_SHOWED_EVENT_NAME = "popupShowed"
t_store_POPUP_CLOSED_EVENT_NAME = "popupHidden"
```

## Основные функции

### Инициализация

#### `t_store_init(recId, options)`

Инициализация каталога. Вызывается автоматически для каждого блока каталога.

```javascript
// Автоматический вызов из Tilda
t_store_init(123456789, {
    storepart: "category-1",
    sidebar: true,
    // ... другие опции
});
```

#### `t_store_lazyInit(recId, options)`

Ленивая инициализация (при появлении в viewport).

#### `t_store_initRouting()`

Инициализация роутинга для popup товаров и History API.

#### `t_store_initPopup(recId, options)`

Инициализация popup окна товара.

### Загрузка товаров

#### `t_store_loadProducts(recId, options, page)`

Загружает товары из API.

```javascript
t_store_loadProducts(123456789, options, 1);
```

#### `t_store_loadProducts_byId(recId, productId, options)`

Загружает один товар по ID.

```javascript
t_store_loadProducts_byId(123456789, "product-uid", options);
```

#### `t_store_loadOneProduct(recId, productUid, options)`

Загружает данные одного товара для popup.

#### `t_store_process(recId, products, options, isLoadMore)`

Обрабатывает загруженные товары.

#### `t_store_process_appendAndShowProducts(recId, products, options)`

Добавляет и отображает товары в сетке.

### Popup товара

#### `t_store_openProductPopup(recId, productEl, options)`

Открывает popup с детальной информацией о товаре.

```javascript
// Открыть popup программно
var productCard = document.querySelector('.t-store__card[data-product-uid="123"]');
t_store_openProductPopup(recId, productCard, options);
```

#### `t_store_showPopup(recId, options)`

Показывает popup (анимация появления).

#### `t_store_closePopup(recId, options)`

Закрывает popup товара.

```javascript
t_store_closePopup(recId, options);
```

#### `t_store_closePopup_routing(recId)`

Закрывает popup с обновлением URL (History API).

#### `t_store_drawProdPopup(recId, productData, options)`

Рендерит содержимое popup товара.

#### `t_store_drawProdPopup_drawGallery(recId, options)`

Рисует галерею изображений в popup.

#### `t_store_fixedPopupButton(recId, options)`

Фиксирует кнопку "Купить" при скролле popup.

### Карточки товаров (HTML генерация)

#### `t_store_get_productCard_html(product, options, recId)`

Генерирует HTML карточки товара.

```javascript
var html = t_store_get_productCard_html(productData, options, recId);
container.innerHTML += html;
```

#### `t_store_get_productCard_img_html(product, options)`

HTML изображения карточки.

#### `t_store_get_productCard_txt_html(product, options)`

HTML текста карточки (название, описание).

#### `t_store_get_productCard_Price_html(product, options)`

HTML цены товара.

#### `t_store_get_productCard_btn_html(product, options, recId)`

HTML кнопки "Купить".

#### `t_store_get_productCard_mark_html(product, options)`

HTML метки товара (sale, new, etc).

#### `t_store_get_productCard_link(product, options)`

Генерирует ссылку на товар.

#### `t_store_get_productPopup_html(recId, options)`

Генерирует HTML структуры popup.

#### `t_store_get_productPopup_buyBtn_html(product, options, recId)`

HTML кнопки покупки в popup.

### Фильтры

#### `t_store_loadFilters(recId, options)`

Загружает и инициализирует фильтры.

#### `t_store_filters_init(recId, options)`

Инициализирует логику фильтров.

#### `t_store_filters_render_selected(recId, options)`

Отображает выбранные фильтры.

#### `t_store_filters_prodsNumber_update(recId, count)`

Обновляет счётчик найденных товаров.

#### `t_store_filters_opts_sort(options)`

Сортирует опции фильтров.

### Пагинация

#### `t_store_pagination_draw(recId, options, totalPages, currentPage)`

Рисует пагинацию.

```javascript
t_store_pagination_draw(recId, options, 10, 1);
```

#### `t_store_pagination_display(recId, show)`

Показывает/скрывает пагинацию.

#### `t_store_loadMoreBtn_display(recId, show)`

Показывает/скрывает кнопку "Загрузить ещё".

### Варианты товаров (Editions)

#### `t_store_product_initEditions(recId, container, product, options)`

Инициализирует варианты товара (размеры, цвета и т.д.).

```javascript
t_store_product_initEditions(recId, popupEl, productData, options);
```

#### `t_store_product_updateEdition(recId, container, product, options)`

Обновляет выбранный вариант (цена, изображение, наличие).

#### `t_store_product_addEditionControls(container, product, options)`

Добавляет контролы выбора вариантов.

#### `t_store_product_detectEditionByControls(container, product)`

Определяет текущий вариант по выбранным контролам.

#### `t_store_product_selectAvailableEdition(container, product, options)`

Выбирает первый доступный вариант.

#### `t_store_product_disableUnavaileOptions(container, product, options)`

Деактивирует недоступные опции.

#### `t_store_product_getEditionOptionsArr(product, options)`

Получает массив опций вариантов.

#### `t_store_product_getFirstAvailableEditionData(product, options)`

Получает данные первого доступного варианта.

#### `t_store_product_triggerSoldOutMsg(container, show)`

Показывает сообщение "Нет в наличии".

### Опции товара

#### `t_store_addProductOptions(recId, container, product, options)`

Добавляет опции товара (множественный выбор).

#### `t_store_product_addOneOptionsControl(container, option, options)`

Добавляет один контрол опции.

#### `t_store_option_handleOnChange(recId, container, optionEl, options)`

Обработчик изменения опции.

#### `t_store_option_getOptionsData(container)`

Получает данные всех выбранных опций.

```javascript
var selectedOptions = t_store_option_getOptionsData(popupEl);
// [{option: "Цвет", variant: "Красный", price: 0}, ...]
```

#### `t_store_option_getColorValue(optionName, variantName)`

Получает цвет для визуального отображения опции.

### Количество товара

#### `t_store_addProductQuantity(recId, container, product, options)`

Добавляет контрол количества товара.

#### `t_store_addProductQuantityEvents(container, options)`

Привязывает события +/- количества.

#### `t_store_removeProductQuantity(container)`

Удаляет контрол количества.

### Галерея

#### `t_store__initDefaultGallery(recId, options)`

Инициализирует стандартную галерею.

#### `t_store__initCustomGallery(recId, container, options)`

Инициализирует кастомную галерею.

#### `t_store_galleryVideoHandle(container)`

Обрабатывает видео в галерее.

#### `t_store_hoverZoom_init(recId)`

Инициализирует zoom при наведении.

#### `t_store_prodPopup_updateGalleryThumbs(recId, options)`

Обновляет превью галереи в popup.

#### `t_store__addSlideChangeListener(slider, callback)`

Добавляет listener смены слайда.

### URL и роутинг

#### `t_store_generateUrl(product, options)`

Генерирует URL товара.

```javascript
var url = t_store_generateUrl(productData, options);
// "/tproduct/123-product-name"
```

#### `t_store_history_pushState(url, title)`

Добавляет запись в History API.

#### `t_store_checkUrl(options, recId)`

Проверяет URL и открывает popup если нужно.

#### `t_store_changeUrl(recId, productUrl, productTitle)`

Изменяет URL при открытии товара.

#### `t_store_paramsToObj(recId, options)`

Парсит URL параметры в объект.

#### `t_store_paramsToObj_updateUrl(recId, params)`

Обновляет URL из объекта параметров.

#### `t_store_updateOptionsBasedOnUrl(options, params, recId)`

Обновляет опции на основе URL.

### Стили и типографика

#### `t_store_createButtonCss(recId, options)`

Создаёт CSS для кнопок.

#### `t_store_applyButtonStyles(button, options)`

Применяет стили к кнопке.

#### `t_store_applyTypoConfig(element, config)`

Применяет типографику к элементу.

#### `t_store_copyTypographyFromLeadToPopup(recId)`

Копирует стили из лида в popup.

#### `t_store_setProductNameStyles(container, options)`

Устанавливает стили названия товара.

#### `t_store_setProductPriceStyles(container, options)`

Устанавливает стили цены.

#### `t_store_setProductDescriptionStyles(container, options)`

Устанавливает стили описания.

### Утилиты

#### `t_store_dict(key)`

Получает перевод по ключу.

```javascript
var text = t_store_dict("addToCart"); // "В корзину"
```

#### `t_store_getDictObj()`

Загружает словарь переводов.

#### `t_store__getFormattedPrice(price, currency, options)`

Форматирует цену.

```javascript
var formatted = t_store__getFormattedPrice(1500, "RUB", options);
// "1 500 ₽"
```

#### `t_store__getFormattedPriceRange(minPrice, maxPrice, currency)`

Форматирует диапазон цен.

```javascript
var range = t_store__getFormattedPriceRange(1000, 5000, "RUB");
// "1 000 - 5 000 ₽"
```

#### `t_store__cleanPrice(priceString)`

Очищает строку цены.

#### `t_store_hexToRgb(hex)`

Конвертирует HEX в RGB.

#### `t_store_luma_rgb(rgb)`

Определяет светлость цвета.

#### `t_store_escapeQuote(string)`

Экранирует кавычки.

#### `t_store_unescapeHtml(string)`

Декодирует HTML-сущности.

#### `t_store_isEmptyValue(value)`

Проверяет пустое значение.

#### `t_store__fadeIn(element, duration)`

Плавное появление элемента.

#### `t_store__removeElement(element)`

Удаляет элемент из DOM.

#### `t_store__scrollToBlock(recId)`

Прокручивает к блоку каталога.

#### `t_store_onFuncLoad(funcName, callback)`

Выполняет callback когда функция загружена.

#### `t_store_triggerEvent(element, eventName)`

Вызывает событие на элементе.

### API и загрузка

#### `t_store__setupEndpoint()`

Настраивает endpoint API.

#### `t_store_changeEndpoint(newEndpoint)`

Меняет endpoint API.

#### `t_store__loadJSFile(url, callback)`

Загружает JS файл.

#### `t_store__loadCSSFile(url)`

Загружает CSS файл.

#### `t_store__getRootZone()`

Возвращает корневую зону (com/ru).

#### `t_store__handleRootzoneRedirect()`

Обрабатывает редирект по зоне.

## События (Events)

### События документа

| Событие  | Описание                | Данные |
| --------------- | ------------------------------- | ------------ |
| `popupShowed` | Popup товара открыт | -            |
| `popupHidden` | Popup товара закрыт | -            |

### Использование событий

```javascript
// Отслеживание открытия popup товара
document.addEventListener('popupShowed', function() {
    console.log('Popup товара открыт');

    var popup = document.querySelector('.t-store__prod-popup__container');
    var productName = popup.querySelector('.t-store__prod-popup__name');
    console.log('Товар:', productName.textContent);
});

// Отслеживание закрытия popup
document.addEventListener('popupHidden', function() {
    console.log('Popup товара закрыт');
});
```

## Data-атрибуты товара

| Атрибут                         | Описание                     |
| -------------------------------------- | ------------------------------------ |
| `data-product-uid`                   | Уникальный ID товара |
| `data-product-gen-uid`               | Генерируемый UID         |
| `data-product-lid`                   | ID листинга                  |
| `data-product-part-uid`              | UID категории               |
| `data-product-price-def`             | Цена по умолчанию     |
| `data-product-price-def-str`         | Цена строкой              |
| `data-product-inv`                   | Остаток на складе     |
| `data-product-img`                   | URL изображения           |
| `data-product-url`                   | URL товара                     |
| `data-product-unit`                  | Единица измерения    |
| `data-product-portion`               | Порция                         |
| `data-product-single`                | Единичный товар        |
| `data-product-pack-label`            | Метка упаковки          |
| `data-product-pack-m`                | Масса упаковки          |
| `data-product-pack-x/y/z`            | Размеры упаковки      |
| `data-edition-option-id`             | ID опции варианта       |
| `data-product-edition-variant-price` | Цена варианта            |

## CSS классы

### Карточки товаров

- `.t-store__card` - карточка товара
- `.t-store__card__imgwrapper` - обёртка изображения
- `.t-store__card__title` - название
- `.t-store__card__descr` - описание
- `.t-store__card__price` - цена
- `.t-store__card__price_old` - старая цена
- `.t-store__card__btn` - кнопка
- `.t-store__card__btn_second` - вторая кнопка
- `.t-store__card__btns-wrapper` - обёртка кнопок

### Popup

- `.t-store__prod-popup` - popup товара
- `.t-store__prod-popup__container` - контейнер popup
- `.t-store__prod-popup__name` - название в popup
- `.t-store__prod-popup__brand` - бренд
- `.t-store__prod-popup__price` - цена в popup
- `.t-store__prod-popup__gallery` - галерея

### Фильтры

- `.t-store__filter` - блок фильтра
- `.t-store__filter__controls-wrapper` - обёртка контролов
- `.t-store__filter__search-and-sort` - поиск и сортировка

### Сетка

- `.t-store__grid-cont` - контейнер сетки
- `.t-store__grid-cont_itemwrapper` - обёртка элемента

## Примеры использования

### Получение данных товара из карточки

```javascript
function getProductDataFromCard(cardEl) {
    return {
        uid: cardEl.getAttribute('data-product-uid'),
        name: cardEl.querySelector('.t-store__card__title').textContent,
        price: parseFloat(cardEl.getAttribute('data-product-price-def')),
        img: cardEl.getAttribute('data-product-img'),
        url: cardEl.getAttribute('data-product-url'),
        inventory: parseInt(cardEl.getAttribute('data-product-inv')) || 0
    };
}

// Использование
var card = document.querySelector('.t-store__card');
var productData = getProductDataFromCard(card);
console.log(productData);
```

### Программное открытие popup товара

```javascript
function openProductPopupByUid(productUid) {
    var card = document.querySelector('.t-store__card[data-product-uid="' + productUid + '"]');
    if (card) {
        var recId = card.closest('.t-rec').id.replace('rec', '');
        // Нужно получить options из инициализации
        card.click(); // Самый простой способ
    }
}
```

### Отслеживание просмотра товаров

```javascript
document.addEventListener('popupShowed', function() {
    var popup = document.querySelector('.t-store__prod-popup__container');
    if (!popup) return;

    var productData = {
        name: popup.querySelector('.t-store__prod-popup__name')?.textContent,
        price: popup.querySelector('.t-store__prod-popup__price')?.textContent,
        sku: popup.querySelector('.t-store__prod-popup__sku')?.textContent
    };

    // Отправка в аналитику
    if (window.dataLayer) {
        dataLayer.push({
            event: 'view_item',
            ecommerce: {
                items: [{
                    item_name: productData.name,
                    price: t_store__cleanPrice(productData.price)
                }]
            }
        });
    }
});
```

### Кастомная фильтрация

```javascript
// Программная установка фильтра
function setFilter(recId, filterName, filterValue) {
    var filterEl = document.querySelector(
        '#rec' + recId + ' .t-store__filter[data-filter-name="' + filterName + '"]'
    );

    if (filterEl) {
        var option = filterEl.querySelector('[data-filter-value="' + filterValue + '"]');
        if (option) {
            option.click();
        }
    }
}

// Сброс всех фильтров
function resetAllFilters(recId) {
    var resetBtn = document.querySelector('#rec' + recId + ' .t-store__filter__reset');
    if (resetBtn) {
        resetBtn.click();
    }
}
```

### Кастомная сортировка

```javascript
function setSorting(recId, sortValue) {
    var sortSelect = document.querySelector('#rec' + recId + ' .t-store__sort-select');
    if (sortSelect) {
        sortSelect.value = sortValue;
        t_store_triggerEvent(sortSelect, 'change');
    }
}

// Примеры значений сортировки
// "date:desc" - по дате (новые)
// "date:asc" - по дате (старые)
// "price:asc" - по цене (возрастание)
// "price:desc" - по цене (убывание)
// "title:asc" - по названию (А-Я)
```

### Мониторинг загрузки товаров

```javascript
// Перехват загрузки товаров
var originalLoadProducts = t_store_loadProducts;
t_store_loadProducts = function(recId, options, page) {
    console.log('Загрузка товаров:', { recId, page });
    return originalLoadProducts.apply(this, arguments);
};

// Отслеживание успешной загрузки
var originalProcess = t_store_process;
t_store_process = function(recId, products, options, isLoadMore) {
    console.log('Загружено товаров:', products.length);
    return originalProcess.apply(this, arguments);
};
```

### Кастомный рендер карточки

```javascript
// Добавление кастомного элемента в карточку после рендера
document.addEventListener('DOMContentLoaded', function() {
    var observer = new MutationObserver(function(mutations) {
        mutations.forEach(function(mutation) {
            mutation.addedNodes.forEach(function(node) {
                if (node.classList && node.classList.contains('t-store__card')) {
                    // Добавляем кастомный бейдж
                    var inv = parseInt(node.getAttribute('data-product-inv'));
                    if (inv > 0 && inv < 5) {
                        var badge = document.createElement('div');
                        badge.className = 'custom-low-stock-badge';
                        badge.textContent = 'Осталось ' + inv + ' шт.';
                        node.querySelector('.t-store__card__imgwrapper').appendChild(badge);
                    }
                }
            });
        });
    });

    var grid = document.querySelector('.t-store__grid-cont');
    if (grid) {
        observer.observe(grid, { childList: true, subtree: true });
    }
});
```

## Структура объекта товара (API)

```javascript
{
    uid: "product-uid-123",
    title: "Название товара",
    descr: "Описание товара",
    text: "Полное описание",
    price: "1500",
    price_old: "2000",
    sku: "ART-001",
    quantity: "100",
    unit: "шт.",
    portion: "",
    pack_label: "",
    pack_m: "",
    pack_x: "", pack_y: "", pack_z: "",
    brand: "Бренд",
    chars: [
        { title: "Характеристика", value: "Значение" }
    ],
    galleries: [
        { img: "url1.jpg" },
        { img: "url2.jpg", video: "youtube-id" }
    ],
    editions: [
        {
            uid: "edition-1",
            price: "1500",
            quantity: "50",
            option: [
                { name: "Размер", value: "XL" },
                { name: "Цвет", value: "Красный" }
            ]
        }
    ],
    single: "",
    url: "/tproduct/123-product-name",
    externalid: "external-123",
    partuids: ["category-1", "category-2"],
    mark: {
        sale: true,
        new: false,
        bestseller: false
    }
}
```
