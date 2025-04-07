# IntlTelInput Plugin

## Содержание
- [Краткое описание](#brief_description)
- [Возможности](#features)
- [Компоненты](#components)


## Краткое описание <a name="brief_description"/>
Плагин JavaScript для ввода и проверки международных телефонных номеров. 
Он преобразует обычное поле ввода, добавляет раскрывающийся список стран, автоматически определяет страну пользователя, отображает соответствующий номер-заполнитель, форматирует номер по мере ввода и предоставляет комплексные методы проверки.
<img src="https://prnt.sc/oi-yw8qWhFG6"/>
![]([https://prnt.sc/oi-yw8qWhFG6](https://prnt.sc/oi-yw8qWhFG6) "Phone number")

## Возможности <a name="features"/> 
- [Автоматически выбирает текущую страну пользователя с помощью поиска по IP](#auto-search-ip)
- [Преобразует обычное поле ввода в поле для ввода телефона](#rerender-input)
- Форматирует номер по мере ввода пользователем

### Автоматически выбирает текущую страну пользователя с помощью поиска по IP <a name="auto-search-ip"/>

При инициализации плагин автоматически определяет страну пользователя по IP. Поиск осуществляется с помощью сервиса ipinfo.io с передачей токена. Для того чтобы установить токен необходимо перейдите на вкладку Settings (Настройки) в OctoberCMS, выбрать сервис IntlTelInput.

### Автоматически выбирает текущую страну пользователя с помощью поиска по IP <a name="auto-search-ip"/>



Input for PopUp
```
<div class="g-mb-30">
    <input 
    	type="tel" 
    	name="iti_phone"
    	id="iti_phone_popup"
    	class="form-control g-color-black g-bg-white g-bg-white--focus g-brd-gray-light-v3 g-brd-primary--hover rounded-3 g-py-13">
    <small id="iti_error-msg_popup" class="hide form-control-feedback"></small>
    <small id="iti_valid-msg_popup" class="hide form-control-feedback">✓ Правильный номер</small>
</div>
```
