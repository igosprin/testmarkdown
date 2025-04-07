# IntlTelInput Plugin

## Содержание
- [Краткое описание](#brief_description)
- [Возможности](#features)
- [Компоненты](#components)


## Краткое описание <a name="brief_description"/>
Плагин JavaScript для ввода и проверки международных телефонных номеров. 
Он преобразует обычное поле ввода, добавляет раскрывающийся список стран, автоматически определяет страну пользователя, отображает соответствующий номер-заполнитель, форматирует номер по мере ввода и предоставляет комплексные методы проверки.
<img src="https://drive.usercontent.google.com/download?id=12coDyiqCVxRjxA4KXLi3aqLq0EJlXZot&export=view&authuser=0"/>

## Возможности <a name="features"/> 
- [Автоматически выбирает текущую страну пользователя с помощью поиска по IP](#auto-search-ip)
- Преобразует обычное поле ввода в поле для ввода телефона
- Форматирует номер по мере ввода пользователем

### Автоматически выбирает текущую страну пользователя с помощью поиска по IP <a name="auto-search-ip"/>

При инициализации плагин автоматически определяет страну пользователя по IP. Поиск осуществляется с помощью сервиса ipinfo.io с передачей токена. Для того чтобы установить токен необходимо перейдите на вкладку Settings (Настройки) в OctoberCMS, выбрать сервис IntlTelInput.

![image](https://drive.usercontent.google.com/download?id=12coDyiqCVxRjxA4KXLi3aqLq0EJlXZot&export=view&authuser=0)


## Компоненты <a name="components"/> 
  - [Преобразования поля и автоматическое определение страны пользователя](#rerender-input)

### Преобразования поля и автоматическое определение страны пользователя <a name="rerender-input"/>

Компонент ищет на странице обычные поля ввода, которые содержат CSS класс iti_phone или iti_phone_popup для поля в попап-окне (всплывающее окно). Также кеширует кеширует токен на 5 часов, используемый для автоматического определения страны пользователя. 

**Название компонента:** [``intlTelInput``](https://github.com/VadimIzmalkov/oc-intltelinput-plugin/blob/master/components/IntlTelInput.php)

#### Пример использования

```HTML
[intlTelInput]
==

<input name="phone" type="tel" placeholder="Введите номер телефона"
              class="iti_phone g-brd-gray-dark-viti_phone g-height-64 g-color-gray-dark-v1 form-control form-control-md g-py-14 rounded-0 g-font-size-14  g-py-14 g-brd-transparent g-bg-secondary g-brd-primary--focus g-brd-primary--hover "
              required value="">
<small class="iti_error-msg hide form-control-feedback"></small>
<small class="iti_valid-msg hide form-control-feedback">✓ Правильный номер</small>

```
