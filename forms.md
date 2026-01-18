# tilda-forms-1.0.min.js

Модуль форм Tilda. Управляет валидацией, отправкой данных, отображением ошибок и успешных сообщений. Поддерживает reCAPTCHA, условные поля и переменные.

## Глобальные переменные

| Переменная             | Тип   | Описание                                           |
| -------------------------------- | -------- | ---------------------------------------------------------- |
| `window.tildaForm`             | Object   | Основной объект форм с методами |
| `window.tildaForm.endpoint`    | String   | Endpoint для отправки (forms.tildacdn.com)      |
| `window.tildaForm.versionLib`  | String   | Версия библиотеки                          |
| `window.initForms`             | Object   | Инициализированные формы по recId |
| `window.t_forms__lang`         | String   | Язык форм (RU/EN/и др.)                         |
| `window.t_forms__inputData`    | Object   | Данные placeholder'ов                              |
| `window.t_forms__fieldApi`     | Function | API для работы с полями                    |
| `window.t_forms__clearForm`    | Function | Очистка формы                                  |
| `window.t_forms__errorTimerID` | Number   | ID таймера ошибки                             |
| `window.validateForm`          | Function | Функция валидации                          |

## Основные функции

### Инициализация

#### `t_forms__initForms()`

Автоматическая инициализация всех форм на странице.

#### `t_forms__initFormFields(recEl)`

Инициализирует поля формы в блоке.

#### `t_forms__addRecaptcha()`

Добавляет reCAPTCHA к формам с ключом.

### Объект window.tildaForm

#### `tildaForm.validate(formEl)`

Валидирует форму и возвращает массив ошибок.

```javascript
var form = document.querySelector('.t-form');
var errors = tildaForm.validate(form);

if (errors.length > 0) {
    console.log('Ошибки:', errors);
    // [{obj: inputEl, type: ['req']}, ...]
} else {
    console.log('Форма валидна');
}
```

#### `tildaForm.send(formEl, btnEl, formType, formsKey)`

Отправляет данные формы.

```javascript
var form = document.querySelector('.t-form');
var btn = form.querySelector('[type="submit"]');
var formType = form.getAttribute('data-formactiontype');
var formsKey = document.getElementById('allrecords').getAttribute('data-tilda-formskey');

tildaForm.send(form, btn, formType, formsKey);
```

#### `tildaForm.showErrors(formEl, errors)`

Отображает ошибки валидации.

```javascript
var errors = [{obj: inputEl, type: ['req', 'email']}];
tildaForm.showErrors(form, errors);
```

#### `tildaForm.hideErrors(formEl)`

Скрывает все ошибки.

```javascript
tildaForm.hideErrors(form);
```

#### `tildaForm.captchaCallback()`

Callback после прохождения reCAPTCHA.

### Валидация

#### `t_forms__getMsg(key)`

Получает сообщение об ошибке по ключу.

```javascript
t_forms__getMsg('email');     // "Укажите, пожалуйста, корректный email"
t_forms__getMsg('req');       // "Пожалуйста, заполните все обязательные поля"
t_forms__getMsg('phone');     // "Укажите, пожалуйста, корректный номер телефона"
t_forms__getMsg('success');   // "Спасибо! Данные успешно отправлены."
```

#### Типы валидации

| Тип        | Описание                  | Паттерн                                   |
| ------------- | --------------------------------- | ------------------------------------------------ |
| `req`       | Обязательное поле | -                                                |
| `email`     | Email адрес                  | Regex с поддержкой кириллицы |
| `url`       | URL адрес                    | http(s)/ftp                                      |
| `phone`     | Телефон                    | Цифры, скобки, +/-                    |
| `number`    | Число                        | Только цифры                          |
| `date`      | Дата                          | Через datepicker                            |
| `time`      | Время                        | HH:mm                                            |
| `name`      | Имя                            | Любые буквы                            |
| `namerus`   | Имя (кириллица)       | Только кириллица                  |
| `nameeng`   | Имя (латиница)         | Только латиница                    |
| `string`    | Строка                      | Буквы, цифры, пунктуация     |
| `minlength` | Мин. длина                | data-tilda-rule-minlength                        |
| `maxlength` | Макс. длина              | data-tilda-rule-maxlength                        |

### Работа с полями

#### `t_forms__findFormField(formEl, fieldName)`

Находит поле формы по имени.

```javascript
var field = t_forms__findFormField(form, 'Email');
```

#### `t_forms__getFormData(formEl)`

Получает данные формы как объект.

```javascript
var data = t_forms__getFormData(form);
// {name: "Иван", email: "ivan@example.com", phone: "+79001234567"}
```

#### `t_forms__clearForm(formEl)`

Очищает все поля формы.

```javascript
t_forms__clearForm(form);
// или
window.t_forms__clearForm(form);
```

#### `t_forms__focusInput(inputEl)`

Фокусирует поле ввода.

### Успешная отправка

#### `t_forms__handleSuccess(formEl)`

Обработка успешной отправки.

#### `t_forms__handleRedirect(formEl, url, successBox)`

Редирект после успешной отправки.

#### `t_forms__drawNewSuccessPopup(formEl)`

Показывает popup успеха (новый стиль).

### Расчёт ширины полей

#### `t_forms__calculateInputsWidth(recId)`

Пересчитывает ширину полей формы.

```javascript
t_forms__calculateInputsWidth(123456789);
```

### Условные поля

#### `t_forms__getConditionCheckHandler(formEl)`

Получает обработчик условий для полей.

```javascript
var handler = t_forms__getConditionCheckHandler(form);
var isHidden = handler.isHiddenByCondition(fieldEl);
```

### Переменные формы

#### `t_forms__getVariableRegexp()`

Regex для поиска переменных `{{form.field}}` или `{{tilda.var}}`.

#### `t_forms__bindFormVariablesToPlaceholders(recEl, placeholders)`

Связывает переменные формы с placeholder'ами.

#### `t_forms__updateAllVariables()`

Обновляет все переменные на странице.

```javascript
window.t_forms__updateAllVariables();
```

### Утилиты

#### `t_forms__getCanonicalUrl()`

Получает canonical URL страницы.

#### `t_forms__getRootZone()`

Возвращает корневую зону (com/ru).

#### `t_forms__isNewSuccessBox(formEl)`

Проверяет новый стиль success-блока.

#### `t_forms__isCachedPage()`

Проверяет кэшированную страницу.

## События (Events)

### Кастомные события формы

| Событие             | Элемент | Описание                             |
| -------------------------- | -------------- | -------------------------------------------- |
| `tildaform:beforesend`   | form           | Перед отправкой формы     |
| `tildaform:aftersuccess` | form           | После успешной отправки |
| `tildaform:aftererror`   | form           | После ошибки отправки     |

### Системные события

| Событие        | Описание                                       |
| --------------------- | ------------------------------------------------------ |
| `displayChanged`    | Изменение отображения поля     |
| `recalculate`       | Пересчёт условий                        |
| `reset`             | Сброс формы                                  |
| `sync-conditionals` | Синхронизация условных полей |

### Примеры подписки на события

```javascript
var form = document.querySelector('.t-form');

// Перед отправкой
form.addEventListener('tildaform:beforesend', function(e) {
    console.log('Отправка формы...');

    // Можно модифицировать данные
    var data = t_forms__getFormData(form);
    console.log('Данные:', data);
});

// После успешной отправки
form.addEventListener('tildaform:aftersuccess', function(e) {
    console.log('Форма успешно отправлена!');

    // Отправка в аналитику
    if (window.dataLayer) {
        dataLayer.push({
            event: 'form_submit',
            formId: form.id
        });
    }
});

// После ошибки
form.addEventListener('tildaform:aftererror', function(e) {
    console.log('Ошибка отправки формы');
});
```

## Data-атрибуты

### Атрибуты формы

| Атрибут          | Описание                               |
| ----------------------- | ---------------------------------------------- |
| `data-formactiontype` | Тип отправки (1-email, 2-custom)    |
| `data-success-url`    | URL редиректа после успеха |
| `data-success-popup`  | Показывать popup ("y")               |
| `data-input-lid`      | ID листинга поля                   |

### Атрибуты полей

| Атрибут                | Описание                        |
| ----------------------------- | --------------------------------------- |
| `data-tilda-req`            | Обязательное поле (1/0) |
| `data-tilda-rule`           | Тип валидации               |
| `data-tilda-rule-minlength` | Минимальная длина       |
| `data-tilda-rule-maxlength` | Максимальная длина     |
| `data-tilda-mask`           | Маска ввода                   |
| `data-hidden-by-condition`  | Скрыто условием ("true")  |

### Атрибуты reCAPTCHA

| Атрибут            | Описание   |
| ------------------------- | ------------------ |
| `data-tilda-captchakey` | Ключ reCAPTCHA |

## CSS классы

### Форма

| Класс                | Описание                               |
| ------------------------- | ---------------------------------------------- |
| `.t-form`               | Контейнер формы                  |
| `.t-form__inputsbox`    | Контейнер полей                  |
| `.t-form__successbox`   | Блок успеха                          |
| `.t-form-success-popup` | Popup успеха                             |
| `.js-form-proccess`     | Форма для обработки           |
| `.js-send-form-error`   | Форма с ошибкой                   |
| `.js-send-form-success` | Форма успешно отправлена |

### Поля

| Класс                | Описание                 |
| ------------------------- | -------------------------------- |
| `.t-input`              | Поле ввода              |
| `.t-input-group`        | Группа поля            |
| `.t-input-error`        | Текст ошибки поля |
| `.t-input-block`        | Блок поля                |
| `.js-tilda-rule`        | Поле с валидацией |
| `.js-error-control-box` | Поле с ошибкой       |

### Ошибки

| Класс                 | Описание                         |
| -------------------------- | ---------------------------------------- |
| `.js-errorbox-all`       | Контейнер всех ошибок |
| `.js-rule-error-all`     | Общая ошибка                  |
| `.js-rule-error-{type}`  | Ошибка по типу               |
| `.t-form__errorbox-item` | Элемент ошибки              |

### Кнопка

| Класс         | Описание                                  |
| ------------------ | ------------------------------------------------- |
| `.t-submit`      | Кнопка отправки                     |
| `.t-btn_sending` | Кнопка в процессе отправки |

## Примеры использования

### Кастомная валидация

```javascript
// Добавить свою валидацию перед отправкой
var form = document.querySelector('.t-form');

form.addEventListener('tildaform:beforesend', function(e) {
    var phone = form.querySelector('input[name="Phone"]');

    // Проверка российского номера
    if (phone && phone.value) {
        var cleaned = phone.value.replace(/\D/g, '');
        if (cleaned.length !== 11 || cleaned[0] !== '7') {
            e.preventDefault();
            alert('Введите номер в формате +7XXXXXXXXXX');
            return false;
        }
    }
});
```

### Программная отправка формы

```javascript
function submitFormProgrammatically() {
    var form = document.querySelector('.t-form');
    var btn = form.querySelector('[type="submit"]');

    // Скрыть ошибки
    tildaForm.hideErrors(form);

    // Валидация
    var errors = tildaForm.validate(form);

    if (errors.length > 0) {
        tildaForm.showErrors(form, errors);
        return false;
    }

    // Отправка
    var formType = parseInt(form.getAttribute('data-formactiontype'));
    var formsKey = document.getElementById('allrecords').getAttribute('data-tilda-formskey');

    tildaForm.send(form, btn, formType, formsKey);
    return true;
}
```

### Заполнение формы из URL параметров

```javascript
document.addEventListener('DOMContentLoaded', function() {
    var params = new URLSearchParams(window.location.search);

    params.forEach(function(value, key) {
        var input = document.querySelector('.t-form input[name="' + key + '"]');
        if (input) {
            input.value = value;
        }
    });
});
```

### Отслеживание изменений полей

```javascript
var form = document.querySelector('.t-form');
var inputs = form.querySelectorAll('.t-input');

inputs.forEach(function(input) {
    input.addEventListener('change', function() {
        console.log('Поле изменено:', input.name, '=', input.value);
    });
});
```

### Работа с переменными формы

```html
<!-- В тексте страницы -->
<p>Здравствуйте, {{form.Name=Гость}}!</p>
<p>Ваш email: {{form.Email}}</p>
```

```javascript
// Обновить переменные после изменения формы
form.addEventListener('change', function() {
    window.t_forms__updateAllVariables();
});
```

### Кастомный success callback

```javascript
// Переопределить обработку успеха
var originalOnSuccess = t_forms__onSuccess;
t_forms__onSuccess = function(formEl) {
    // Свой код
    console.log('Форма отправлена!');

    // Отправка в CRM
    var data = t_forms__getFormData(formEl);
    fetch('/api/crm/lead', {
        method: 'POST',
        body: JSON.stringify(data)
    });

    // Вызвать оригинальную функцию
    return originalOnSuccess.apply(this, arguments);
};
```

### Динамическое добавление полей

```javascript
function addCustomField(form, name, value) {
    var input = document.createElement('input');
    input.type = 'hidden';
    input.name = name;
    input.value = value;
    form.appendChild(input);
}

// Использование
var form = document.querySelector('.t-form');
addCustomField(form, 'utm_source', 'google');
addCustomField(form, 'page_url', window.location.href);
```

## Сообщения об ошибках (RU/EN)

```javascript
// Русские сообщения
{
    success: "Спасибо! Данные успешно отправлены.",
    email: "Укажите, пожалуйста, корректный email",
    phone: "Укажите, пожалуйста, корректный номер телефона",
    req: "Пожалуйста, заполните все обязательные поля",
    minlength: "Слишком короткое значение",
    maxlength: "Слишком длинное"
}

// Английские сообщения
{
    success: "Thank you! Your data has been submitted.",
    email: "Please enter a valid email address",
    phone: "Please put a correct phone number",
    req: "Please fill out all required fields",
    minlength: "Value is too short",
    maxlength: "Value too big"
}
```
