# Directory. OctoberCMS plugin

- [Возможности и описание](#features)
    - [Каталоги и уровни вложенности](#catalogues)
    - [Разделы каталогов](#sections)
    - [Профили компаний](#profiles)
        - [Описание полей](#profile-fileds)
        - [URL-адреса](#profile-urls)
        - [Диалоги и комментарии](#profile-dialogs)
    - [Телефонный справочник](#phonebook)
- [Компоненты](#components)
    - [Metainfo](#metainfo)
    - [Форма создания/редактирования компании](#company-crud)
    - [Список всех каталогов](#directory-list)
    - [Каталог, список разделов каталога](#section-list)
    - [Раздел каталога (список компаний по slug раздела)](#company-list-slug)
    - [Раздел каталога (список компаний по id раздела)](#company-list-id)
    - [Профиль компании](#item-profile)
    - [Похожие компании](#similar-companies)
    - [Directory Items](#directory-items)
    - [Последние отзывы на компании](#last-reviews)
    - [Отзывы пользователя](#user-reviews")
    - [Кнопка "Полезный отзыв"](#useful-review")
    - [Пользовательский список каталогов](#user-directory-list)
    - [Пользовательский список разделов](#user-section-list)
    - [Пользовательский список компаний](#user-company-list)
    - [Телефонный справочник](#phonebook-component)
    - [Отзывы раздела](#last-section-reviews")
    - [Отзывы каталога](#last-directory-reviews")
    - [Раздел "вопросы и ответы" определенной секции](#directory-section-faq")
- [База данных](#database)
    - [Схема таблиц и связей](#db-scheme)
    - [Модели](#models)
- [Настройки](#settings)
    - [Каталог](#settings-directory)
    - [Телефонный справочник](#phonebook-setting)
    - [Sitemap](#settings-sitemap)
- [Рекомендации для FrontEnd](#frontend-recommendations)
    - [Вывод хлебных крошек](#frontend-recommendations-breadcrumbs)
- [Управление каталогом](#directory-managment)
    - [Создание каталога](#frontend-recommendations-breadcrumbs)
    - [Создание раздела](#frontend-recommendations-breadcrumbs)
    - [Создание компании](#directory-banners)
    - [Размещение баннеров раздела на профиле компании](#directory-banners)
- [Решение других проблем с каталогом](#solving-other-problems)
  - [Востановление метаданных компаний](#recover-meta)

## Возможности и описание <a name="features"/>

Плагин позволяет создавать сайты-агрегаторы, каталоги компаний (товаров, телеграмм каналов, фильмов и тд) и так называемые "отзовики".

- Создание каталогов компаний, товаров, услуг и тд
- Создание разделов и подразделов каталога
- Телефонный справочник
- Несколько каталогов с одном уровнем вложенности, или один с двумя
- Добавление компаний для зарегистрированных и незарегистрированных пользователей
- Написание отзыв на компанию для зарегистрированных и незарегистрированных пользователей
- Индивидуальные HTML страницы раздела каталога
- Индивидуальные HTML страницы профилей компаний, для выбранного раздела
- Индивидуальные HTML страницы  профиля отдельно выбранной компаний
- URL-адреса профилей компаний и разделов добавляются в sitemap
- Возможность связывания компаний - создание партнеров, спонсоров и тд.
- Хлебные крошки каталога

### Каталоги и уровни вложенности <a name="catalogues"/>

Сайт может содержать как **один** каталог и иметь **два** уровня вложенности, например каталог компаний:
- ``example.com/companies`` - каталог компаний, список разделов
- ``example.com/companies/finance`` - раздел финансы, список подразделов
- ``example.com/companies/finance/bank`` - подраздел банки , список компаний

Так и **несколько** каталогов с **одним** уровнем вложенности:
- ``example.com/companies`` - каталог компании, список разделов
- ``example.com/companies/banks`` - раздел банки, список компаний
- ``example.com/goods`` - каталог товаров, список разделов
- ``example.com/goods/tvs`` - раздел телевизоры, список телевизоров

HTML код для стандартной страницы каталога хранится в файле темы ``directory/directory.htm`` и имеет параметризованный URL-адрес внутри CMS страницы
```
url = '/:directory_slug/'
```
Что бы изменить дизайн стандартной страницы каталога, создается файлы вида ``directory/directory-goods.htm`` с константным URL-адресом и своей HTML разметкой:
```
url = '/goods/'
```

### Разделы каталогов <a name="sections"/>

HTML код для стандартной страницы раздела каталога храниться в файле темы ``directory/section.htm`` и имеет параметризованный URL-адрес внутри CMS страницы
```
url = '/:directory_slug/:section_slug/'
```

Для страниц разделов, где расположение блоков, баннеры, оформление должны отличаться от стандартного, создается файлы вида ``directory/section-banks.htm`` с константным URL-адресом:
```
url = '/companies/banks/'
```

### Профили компаний и их URL-адреса <a name="profiles"/>

Ссылка на профиль компании может иметь одну из двух форм

- короткую   ``example.com/companies/alfabank``
- длинную    ``example.com/companies/banks/alfabank``


HTML код для стандартной страницы профиля храниться в файле темы ``directory/profile.htm``.
Тип URL-адреса определяется параметром ``url``.

Для короткой формы :
```
url = '/companies/:item_slug'

[itemDirectoryProfile]
directorySlug = ""
sectionSlug = ""
itemDirectorySlug = "{{ :item_slug }}"
```

Для длинной формы:
```
url = '/:directory_slug/:section_slug/:item_slug'

[itemDirectoryProfile]
directorySlug = "{{ :directory_slug }}"
sectionSlug = "{{ :section_slug }}"
itemDirectorySlug = "{{ :item_slug }}"
```

Если выбрана длинная форма URL-адреса, каждый раздел может иметь свой уникальный дизайн профиля. Для этого создается соответствующая CMS страница ``directory/profile-forex.htm`` (только для длинной формы!):
```
url = '/companies/forex/:item_slug'

[itemDirectoryProfile]
directorySlug = "companies"
sectionSlug = "forex"
itemDirectorySlug = "{{ :item_slug }}"
```

Что бы создать уникальный профиль для определенной компании, нужно создать файл профиля ``directory/profile-alfabank.htm``:
```
url = /companies/banks/alfabank

[itemDirectoryProfile]
directorySlug = "companies"
sectionSlug = "banks"
itemDirectorySlug = "alfabank"
```

#### URL адреса <a name="profile-urls"/>

Для получения канонического URL-адреса на профиль используется метод модели Элемент (itemDirectory) `getCanonicalUrl()`:

```PHP
$profileUrl = $itemDirectory->getCanonicalUrl();
```

Если необходимо получить URL-адрес на профиль компании, но в другом разделе (не конанический), вызывается `getProfileUrl()` на моделе Елемент-Раздел (itemSection):

```PHP
$profileUrl = $itemSection->getProfileUrl();
```

В случае короткой формы адреса для формированя ссылки на профиль допускается использовать стандатный способ построения ссылок в OctoberCMS:

```HTML
<a href="{{ 'directory/profile-brokers'|page({item_slug: itemDirectory.slug }) }}">
```

### Телефонный справочник <a name="phonebook"/>

Телефонный справочник - это каталог с одним разделом, в котором хранятся телефоны и отзывы на них. Такой каталог может существовать в единичном числе и после создания не может быть удален.

Телефонный справочник создается как обычный раздел с включенной опцией "Телефонный справочник":

![изображение](https://user-images.githubusercontent.com/6684336/177720619-47c89e0f-d3e7-4c08-ae8b-a03e9d8e0a5d.png)

После создания телефонного справочника в списке появится каталог с название PhoneBook, и разделом PhoneBook. Дополнительных разделов этот каталог содержать не может.

В настройках плагина необходимо выбрать CMS страницу которая будет страницей телефона (Phone Profile) с его отзывами

Телефоны добавляются через FrontEnd (см компонент ...)

## Компоненты <a name="components"/>

### Компоненты перенесенные с плагина metainfo <a name="metainfo"/>
### directoryMetainfo
Компонент для добавления метаинформации на страницу директории (страница со списком секций конкретной директории).

> В параметрах можно указать либо ID конкретной директории (параметр directory) либо выбрать директорию по :directory_slug (параметр directorySlug).

Добавить на страницу в разделе конфигурации:

    [directoryMetainfo]
    directory = 0
    directorySlug = "{{ :directory_slug }}"

> Если вы добавляете этот компонент через раздел CMS в административной части сайта, то в дропдауне выбора директории вы можете выбрать 'Select by slyg', или оставить пустым. Тогда директория будет выбрана по слагу.

Также на страницу в раздел php необходимо добавить следующий код со значениями по умолчанию:

    <?php
        function onEnd()
        {
            $this->setMeta();
        }

        function setMeta()
        {
            $defaultPageTitle       = 'Список подразделов раздела';
            $defaultMetaTitle       = 'Список подразделов раздела';
            $defaultMetaDescription = 'Список подразделов раздела';

            $directoryPageTitle       = $this->directoryMetainfo->getPageTitle();
            $directoryMetaTitle       = $this->directoryMetainfo->getMetaTitle();
            $directoryMetaDescription = $this->directoryMetainfo->getMetaDescription();

            $this->page->title            = $directoryPageTitle ? $directoryPageTitle : $defaultPageTitle;
            $this->page->meta_title       = $directoryMetaTitle ? $directoryMetaTitle : $defaultMetaTitle;
            $this->page->meta_description = $directoryMetaDescription ? $directoryMetaDescription : $defaultMetaDescription;
        }
    ?>

### sectionMetainfo
Компонент для добавления метаинформации на страницу секции (страница со списком компаний конкретной секции).

> В параметрах можно указать либо ID конкретной секции (параметр section) либо выбрать секцию по :directory_slug и :section_slug (параметры directorySlug и sectionSlug).

Добавить на страницу в разделе конфигурации:

    [sectionMetainfo]
    section = 0
    directorySlug = "{{ :directory_slug }}"
    sectionSlug = "{{ :section_slug }}"

> Если вы добавляете этот компонент через раздел CMS в административной части сайта, то в дропдауне выбора секции вы можете выбрать 'Select by slyg', или оставить пустым. Тогда секция будет выбрана по слагу.

Также на страницу в раздел php необходимо добавить следующий код со значениями по умолчанию:

    <?php
        function onEnd()
        {
            $this->setMeta();
        }

        function setMeta()
        {
            $defaultPageTitle       = 'Каталог компаний раздела';
            $defaultMetaTitle       = 'Каталог компаний раздела';
            $defaultMetaDescription = 'Список лучших по надежности компаний';

            $sectionPageTitle       = $this->sectionMetainfo->getPageTitle();
            $sectionMetaTitle       = $this->sectionMetainfo->getMetaTitle();
            $sectionMetaDescription = $this->sectionMetainfo->getMetaDescription();

            $this->page->title            = $sectionPageTitle ? $sectionPageTitle : $defaultPageTitle;
            $this->page->meta_title       = $sectionMetaTitle ? $sectionMetaTitle : $defaultMetaTitle;
            $this->page->meta_description = $sectionMetaDescription ? $sectionMetaDescription : $defaultMetaDescription;
        }
    ?>

### itemDirectoryMetainfo
Компонент для добавления метаинформации на страницу профиля компании.

Добавить на страницу в разделе конфигурации:

    [itemDirectoryMetainfo]
    itemDirectorySlug = "{{ :slug }}"

Также на страницу в раздел php необходимо добавить следующий код со значениями по умолчанию:

    <?php
        function onEnd()
        {
            $this->setMeta();
        }

        function setMeta()
        {
            $defaultPageTitle       = 'Анкета компании: ' . $this->itemDirectory->name;
            $defaultMetaTitle       = 'Анкета компании: ' . $this->itemDirectory->name;
            $defaultMetaDescription = 'Детально про компанию: ' . $this->itemDirectory->name;

            $itemDirectoryPageTitle       = $this->itemDirectoryMetainfo->getPageTitle();
            $itemDirectoryMetaTitle       = $this->itemDirectoryMetainfo->getMetaTitle();
            $itemDirectoryMetaDescription = $this->itemDirectoryMetainfo->getMetaDescription();

            $this->page->title            = $itemDirectoryPageTitle ? $itemDirectoryPageTitle : $defaultPageTitle;
            $this->page->meta_title       = $itemDirectoryMetaTitle ? $itemDirectoryMetaTitle : $defaultMetaTitle;
            $this->page->meta_description = $itemDirectoryMetaDescription ? $itemDirectoryMetaDescription : $defaultMetaDescription;

            $this->page->meta_robots      = $this->itemDirectoryMetainfo->getMetaRobots();
        }
    ?>


### Форма создания/редактирования компании <a name="company-crud"/>

**Название компонента:** ``directoryCRUDForm`` (https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/DirectoryCRUDForm.php)

Позволяет пользователям создавать/редактировать компанию. Компания может быть создана как зарегистрированным пользователем так и нет.

Для _зарегистрированного_ пользователя добавление компании происходит в 2 шага:
- Добавление информации о компании
- Внесение компании в раздел каталога

Для _незарегистрированного_ пользователя добавляется дополнительный шаг:
- Добавление информации о компании
- Внесение компании в раздел каталога
- Регистрация пользователя и привязка добавленной компании к его созданному аккаунту

**Параметры компонента:**

- `context = create/update` - влияет на работу компонента, если контекст update то компонент будет искать единицу каталога по слагу и проверять имеет ли пользователь право её редактировать
- `itemDirectorySlug = {{ :item_slug }}` - слаг единицы каталога, которую необходимо отредактировать. (в контексте create не нужен)
- `redirect = "account/companies"` - страница на которую будет переадресован пользователь после успешного апдейта единицы каталога

**Методы компонента:**

- `getDirectories()` - возвращает коллекцию всех каталогов из базы для отображения в дропдауне для добавлении единицы каталога в каталог ([пример](#example2))
- `getItemDirectory()` - возвращает единицу каталога, открытую для редактирования
- `onCreate()` - метод для ajax-формы создания единицы каталога ([пример](#example1))
- `onUpdate()` - метод для ajax-формы апдейта единицы каталога и custom_fields в принадлежащих ей itemsSection ([пример](#example3))
- `onChooseDirectory()` - ajax метод перерендеривает паршиал directory-dependent-select и обновляет в нём опции дропдауна, вставляя в качестве опций разделы, принадлежащие выбранному выше каталогу ([пример](#example2))
- `onChooseSection()` - ajax метод перерендеривает паршиал section-dependent-fields и отображает в нём поля для заполнения custom_fields из выбранного выше раздела ([пример](#example2))
- `onItemSectionCreate()` - метод для ajax-формы добавления единицы каталога в раздел (создания itemSection с заполнением custom_fields) ([пример](#example2))
- `onItemSectionUpdate()` - метод для ajax-формы редактирования custom_fields в itemSection ([пример](#example3))

#### Пример формы создания компании: <a name="example1" />

```html
<!-- компонент -->
[directoryCRUDForm]
context = "create"
redirect = "account/company-add-catalog"

<!-- форма -->
{{ form_ajax('onCreate', {files: 'true', 'data-request-validate': true }) }}
  <input name="name" type="text">
  <input name="web_site" type="text">
  <!-- здесь необходимые поля -->
  <button type="submit" data-attach-loading>Сохранить</button>
{{ form_close() }}
```

#### Пример формы добавления компании в раздел каталога: <a name="example2" />

В примере использован плагин [Select2](https://select2.org/)

Выбор раздела реализован двумя взаимозависимыми выпадающими списками - _выбор каталога_ и _выбор раздела_.

Если выбран каталог разделы подгружаются в выпадющий список который находится внутри тега ``<div id="directoryDependentSelect">``.

После выбора раздела внутри тега ``<div id="sectionDependentFields">`` появятся поля с дополнительной информацией о компании (см custom_fields)

```html
<!-- компонент -->
[directoryCRUDForm]
context = "update"
itemDirectorySlug = {{ :item_slug }}
redirect = "account/companies"

{% set directories = __SELF__.getDirectories() %}

<!-- форма -->
{{ form_ajax('onItemSectionCreate', {'data-request-validate': true }) }}

  <!-- Выбор каталога -->
  <select name="directory"
    data-request="onChooseDirectory"
    data-request-success="$.HSCore.components.HSSelect.init('.js-custom-select')">
    <option></option> <!-- необходимое условние. см док. по select2 https://select2.org/placeholders -->
    {% for directory in directories %}
      <option value="{{ directory.id }}">
        {{ directory.name }}
      </option>
    {% endfor %}
  </select>

  <!-- Выбор раздела -->
  <div id="directoryDependentSelect">
    <select name="section" data-request="onChooseSection" disabled>
      <option></option>
    </select>
  </div>

  <!-- Поля с данными о компании -->
  <div id="sectionDependentFields"></div>

  <button type="submit">Добавить в каталог</button>
{{ form_close() }}
```

#### Пример формы редактирования компании: <a name="example3" />

```html
[directoryCRUDForm]
context = "update"
itemDirectorySlug = {{ :item_slug }}
redirect = "account/companies"

{% set itemsDirectory = componentDirectoryCRUDForm.getItemDirectory %}
{{ form_ajax('componentDirectoryCRUDForm::onUpdate', {files: 'true', 'data-request-validate': true }) }}
  <input name = "name">

  <!-- пример поля телефон с выбором кода страны (см. https://github.com/jackocnr/intl-tel-input) -->
  <input name = "phone" id = "iti_phone" type = "tel" value="{{ form_value('phone')|default(itemDirectory.phone) }}">
  <small class="iti_error-msg hide form-control-feedback"></small>
  <small class="iti_valid-msg hide form-control-feedback">✓ Правильный номер</small>
  <div data-validate-for="phone" class="alert alert-danger"></div>

  <!-- здесь дополнительные поля -->

  <!-- вывод пользовательских полей для каждого раздела -->
  {% for itemSection in itemDirectory.itemsSection %}

    <!-- элемент формы -->
    {% for fieldMetadata in itemSection.section.custom_fields_metadata %}
      {% set preparedFieldName = itemSection.section.slug ~ '_' ~ fieldMetadata.field_name %}

      <!-- название, метка поля -->
      <label for="{{ preparedFieldName }}">
        {{ fieldMetadata.frontend_field_label }}
        <!-- звездочка, если поле обязательное -->
        {% if fieldMetadata.frontend_required %}<span class="g-color-lightred">*</span>{% endif %}
      </label>

      <!-- для textarea и richeditor используется своя разметка -->
      {% if fieldMetadata._group == 'textarea' or fieldMetadata._group == 'richeditor' %}
      <textarea name="custom_fields[{{ itemSection.section.slug }}][{{ preparedFieldName }}]"
        placeholder="{{ fieldMetadata.frontend_field_placeholder }}"
        id="{{ preparedFieldName }}"
        {% if fieldMetadata.frontend_required %} required {% endif %}
        {% if fieldMetadata.frontend_textarea_rows %}
          rows="{{ fieldMetadata.frontend_textarea_rows }}"
        {% endif %}>
        {{ form_value(preparedFieldName)|default(itemSection.custom_fields[preparedFieldName]) }}
      </textarea>
      <div data-validate-for="{{ preparedFieldName }}" class="alert alert-danger"></div>

      <!-- для остальных полей -->
      {% else %}
        <input  name="custom_fields[{{ itemSection.section.slug }}][{{ preparedFieldName }}]"
          type="{{ fieldMetadata._group }}"
          placeholder="{{ fieldMetadata.frontend_field_placeholder }}"
          id="{{ preparedFieldName }}" {% if fieldMetadata.frontend_required %} required {% endif %}
          {% if fieldMetadata.checked_by_default %} checked {% endif %}
          value="{{ form_value(preparedFieldName)|default(itemSection.custom_fields[preparedFieldName]) }}">
        <div data-validate-for="{{ preparedFieldName }}" class="alert alert-danger"></div>
      {% endif %}

{{ form_close() }}
```

### Список всех каталогов <a name="directory-list"/>

Выводит список всех каталогов на странице.

> Возвращает масив с данными о главных разделах с кеша, если в кеше данных нет, выполняеться запрос и кеширует его. Данные в кеше храняться 2 часа(7200 секунд).

**Название компонента:** [``directories``](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/Directories.php)

**Методы:**

- `getDirectories()` - возвращает **данные** в виде масива, о всех каталогах из базы
- `getBreadcrumbs(array $titles = [])` - возвращает массив где ключи - названия страниц, а значения - ссылки на эти страницы (названия можно передать кастомные)

#### Структура возвращаемых данных из ``getDirectories()``:

```PHP
array:7 [▼
  0 => {#6223 ▼ // instance of stdClass
    +"id": 40
    +"name": "Информационные технологии"
    +"slug": "information-technologies"
    +"created_at": "2021-12-20T15:29:25.000000Z"
    +"updated_at": "2023-02-20T16:11:25.000000Z"
    +"deleted_at": null
    +"directory_page_title": "Информационные технологии – отзывы о организациях в сфере информационных технологий"
    +"directory_meta_title": "Информационные технологии"
    +"directory_meta_description": "Список компаний предоставлающих услуги в области информационных технологий. Читайте и оставляйте отзывы о сервисах и организация."
    +"about": ""
    +"sort_order": 10
    +"number_of_sections": 4
    +"number_of_items_directory": 87
    +"phone_book": 0
    +"picture": {#6224 ▼
      +"id": 4882
      +"disk_name": "61efc980f394b714982298.svg"
      +"file_name": "Vector.svg"
      +"file_size": 1583
      +"content_type": "image/svg"
      +"title": null
      +"description": null
      +"field": "picture"
      +"sort_order": 4882
      +"created_at": "2022-01-25T09:57:20.000000Z"
      +"updated_at": "2022-01-25T09:57:22.000000Z"
      +"path": "http://127.0.0.1:8007/storage/app/uploads/public/61e/fc9/80f/61efc980f394b714982298.svg"
      +"extension": "svg"
    }
  }
  1 => {#6225 ▶}
  2 => {#6227 ▶}
]
```


#### Пример использования: <a name="directory-list-example" />

```HTML
<!-- компонент -->
[directories componentDirectories]

{% set directories = componentDirectories.getDirectories() %}
{% for directory in directories %}
  <a href="{{ 'directory/directory'|page({ directory_slug:directory.slug }) }}">
    {{ directory.picture.path | resize(60, 30, { mode: 'fit' }) }}{{ directory.name }}
  </a>
  <p>{{directory.directory_meta_title}} или <p>{{directory.directory_page_title}}</p></p>
  <p>{{directory.directory_meta_description}}</p>
  <!-- количество компаний в каталоге -->
  {{ directory.number_of_items_directory }} компаний
  <!-- количество разделов в каталоге -->
  {{ directory.number_of_sections }} разделов
{% endfor %}
```

### Список разделов каталога <a name="section-list"/>

Выводит список разделов разделов по слагу каталога

**Название компонента:** [`directorySections`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/DirectorySections.php)

**Праметры компонента:**

- `directorySlug = {{ :directory_slug}}` - слаг каталога
- `itemsSectionPerPage` - количество выводимых на странице единиц каталога

**Методы компонента:**

- `getDirectory()` - возвращает найденный по слагу каталог (модель)
- `getSections()` - возвращает коллекцию разделов каталога
- `getItemsSection()` - возвращает пагинированную коллекцию itemsSection (моделей-связей между единицей каталога и разделом, принадлежащих разделам текущего каталога)
- `listSections()` - возвращает разделы текущего каталога
- `getBreadcrumbs(array $titles = [])` - возвращает массив где ключи - названия страниц, а значения - ссылки на эти страницы (названия можно передать кастомные)

#### Пример использования: <a name="section-list-example" />

```HTML
<!-- компонент -->
[directorySections]
directorySlug = {{ :directory_slug }}
itemsSectionPerPage = 10

{% set sections = directorySections.getSections %}
{% for section in sections %}
  <a href="{{ 'directory/section' | page({ directory_slug:directory.slug,section_slug:section.slug }) }}">
    {{ directory.name }}
  </a>
  <!-- количество компаний в разделе -->
  {{ section.number_of_items_directory }} компаний
{% endfor %}
```

### Список компаний раздела (slug) <a name="company-list-slug"/>

Выводит список компаний по slug раздела

**Название компонента:** [`sectionItemsDirectory`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/SectionItemsDirectory.php)

**Параметры:**

- `directorySlug = "{{ :directory_slug }}"` - слаг каталога
- `sectionSlug = "{{ :section_slug }}"` - слаг раздела
- `itemsPerPage` - количество единиц каталога выводимых на странице
- `orderBy = display_rating/fixed_rating/rating/created_at` - поле по которому надо отсортировать выводимые единицы каталога
- `orderDirection = asc/desc` - направление сортировки - по возрастанию или по убыванию

**Методы:**

- `getItemsSectionPaginated(int $itemsPerPage = null)` - возвращает пагинированную коллекцию itemsSection. Если не передавать параметр $itemsPerPage, то будет использован параметр указанный при подключении компонента.

#### Пример использования: <a name="company-list-slug-example" />

```HTML
<!-- компонент -->
[sectionItemsDirectory]
directorySlug = "{{ :directory_slug }}"
sectionSlug = "{{ :section_slug }}"
itemsPerPage = 30
orderBy = "display_rating"
orderDirection = "desc"

{% set items = sectionItemsDirectory.getItemsSectionPaginated(30) %}
{% for item in items %}
  <a href="{{ item.getProfileUrl() }}">{{ item.itemDirectory.name }}</a>
  <img src="{{ item.itemDirectory.logo|resize(80, auto, { mode: 'portrait' }) }}">
  <!-- рейтинг -->
  <div class="js-rating" data-rating="{{ item.itemDirectory.display_rating }}"></div>
  <!-- количество отзывов -->
  <span>{{ item.itemDirectory.getApprovedReviews.count }}</span>
  <!-- количество просмотров -->
  <span>{{ item.itemDirectory.views }}</span>
{% endfor %}
```

### Список компаний раздела (id) <a name="company-list-id"/>

Выводит список компаний по id раздела (Старый компонента, поддерживается для обратной совместимости со старыми темами)

**Название компонента:** [`sectionItemsDirectoryStatic => 'recordList'`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/SectionItemsDirectoryStatic.php)

**Параметры:**

- `directory` - id каталога
- `section` - id раздела
- `recordsPerPage` - количество единиц каталога выводимых на странице
- `orderByRandom` - сортировать компании случайным образом

**Методы:**

- `listItems()` - возвращает коллекцию itemsSection (моделей-связей между единицей каталога и разделом, принадлежащих текущему разделу)

#### Пример использования: <a name="company-list-id-example" />

```HTML
<!-- компонент -->
[recordList]
directory = 1
section = 1
recordsPerPage = 15
orderByRandom = 1

{% set items = recordList.listItems() %}
{% for item in items %}
  <a href="{{ item.getProfileUrl() }}">{{ item.itemDirectory.name }}</a>
  <img src="{{ item.itemDirectory.logo|resize(80, auto, { mode: 'portrait' }) }}">
  <!-- рейтинг -->
  <div class="js-rating data-rating="{{ item.itemDirectory.display_rating }}"></div>
  <!-- количество отзывов -->
  <span>{{ item.itemDirectory.getApprovedReviews.count }}</span>
  <!-- количество просмотров -->
  <span>{{ item.itemDirectory.views }}</span>
{% endfor %}
```

### Профиль компании <a name="item-profile"/>

Находит в базе единицу каталога ItemDirectory (компанию) и выводит данные о ней. Позволяет оставлять отзывы о компании.

**Название компонента:** [`itemDirectoryProfile`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/ItemDirectoryProfile.php)

**Параметры:**

- `directorySlug = "{{ :directory_slug }}"` - слаг каталога
- `sectionSlug = "{{ :section_slug }}"` - слаг раздела
- `itemDirectorySlug = "{{ :item_slug }}"` - слаг компании
- `reviewsPerPage` - количество отзывов выводимых на странице
- `ratingRequired` - требовать поставить оценку при добавлении отзыва и не сохранять отзывы без оценки

**Методы:**

- `getItemDirectory()` - возвращает модель компании (единицы каталога)
- `getItemSection()` - возвращает модель itemSection (модель-связь между компанией и разделом)
- `getCanonicalUrl()` - возвращает ссылку на "главный" профиль, если текущий профиль открыт не с primary itemSection
- `getApprovedReviewsPaginated()` - возвращает пагинированную коллекцию одобренных отзывов о компании
- `getApprovedAndMyReviewsPaginated()` - возвращает пагинированную коллекцию одобренных и отзывов и отзывов оставленных текущим пользователем
- `getBreadcrumbs($titles = [])` - возвращает ассоциативный массив, где ключами являются названия страниц, а значениям ссылки на них. Этот массив можно передать в паршиал 'common/breadcrumbs'. Также в метод можно передать кастомные названия страниц
- `getSectionBanner($bannerName)` - возвращает путь к паршиалу баннера для текущего раздела, выбранный по названию баннера (для каждого раздела может быть несколько банеров с разными названиями)
- `onReview()` - обработчик формы добавления отзыва. Пример использования можно увидеть в паршиале review-form разметки компонента
- `onAnswer()` - обработчик формы добавления ответа на другой отзыв. Пример использования можно увидеть в паршиале modal-answer-form разметки компонента
- `isAnonymousCommentsAllowed()` - возвращает true/false в зависимости от того разнешены ли анонимные отзывы на этом сайте в настройках плагина в админке
- `isUserItemDirectoryOwner()` - возвращает true/false в зависимости от того является ли пользователь владельцем текущей компании
- `getApprovedAndMyReviewsWithRating()` - возвращает процентное соотношение отзывов (например, "Ужасно": 10%, "Отлично": 90%);

#### Пример использования: <a name="item-profile-example" />

```HTML
<!-- компонент -->
[itemDirectoryProfile componentItemDirectoryProfile]
directorySlug = "{{ :directory_slug }}"
sectionSlug = "{{ :section_slug }}"
itemDirectorySlug = "{{ :item_slug }}"
reviewsPerPage = 15
ratingRequired = 1

==
<?php
function onEnd()
{
    $this['itemDirectory'] = $this->componentItemDirectoryProfile->getItemDirectory();
    if ($this->itemDirectory->logo) {
        $this->page->og_image = $this->itemDirectory->logo->getThumb('auto', 250);
    }
}

function setCanonicalUrl()
{
    $this->page->canonical_url = $this->itemDirectory->getCanonicalUrl();
}
==

{% set itemDirectory = componentItemDirectoryProfile.getItemDirectory %}
{% set itemSection = componentItemDirectoryProfile.getItemSection %}

{% partial 'common/breadcrumbs' breadcrumbs = componentItemDirectoryProfile.getBreadcrumbs() %}

Название компании: {{ itemDirectory.name }}
Количество просмотров: {{ itemDirectory.views }}

Отзывы:
{% partial 'componentItemDirectoryProfile::reviews'
reviews = componentItemDirectoryProfile.getApprovedAndMyReviewsPaginated %}
Оставить отзыв:
{% component 'componentItemDirectoryProfile::review-form' %}
```

### Похожие компании <a name="similar-companies"/>

Выбирает из базы компании, которые находятся в одинаковых разделах с данной компанией

> Возвращает масив с данными о компаниях с кеша, если в кеше данных нет, выполняеться запрос и кеширует его. Данные в кеше храняться 2 часа(7200 секунд).

**Название компонента:** [`similarItems`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/SimilarItems.php)

**Параметры:**

- `itemDirectorySlug = "{{ :item_slug }}"` - слаг компании
- `perPage` - количество похожих компаний выводимых на странице

**Методы:**

- `getBreadcrumbs(array $titles = [])` - генерирует клебные крошки страницы
- `getItems()` - возвращает данные в виде масива, о каждой выбраной компании с установленым лимитом (для этого метода perPage - это лимит)
- `getItemsSectionPaginated()` - возвращает пагинированую коллекцию.

#### Структура возвращаемых данных из ``getItems()``:

```PHP
array:10 [▼ // instance of stdClass
  0 => {#6191 ▼
    +"companyName": "Crypto Donut"
    +"companySlug": "crypto-donut"
    +"numViews": 6
    +"companyDisplayRating": "1.00"
    +"sectionSlug": "telegram-channels"
    +"sectionName": "Telegram-каналы"
    +"directorySlug": "information-technologies"
    +"directoryName": "Информационные технологии"
    +"logoPath": "http://127.0.0.1:8007/storage/app/uploads/public/633/ff2/2c5/633ff22c56503694548910.jpg"
    +"sectionCustonFields": array:3 [▼ // instance of stdClass
      0 => {#6192 ▼
        +"field": "subscribers"
        +"value": "12945"
      }
      1 => {#6197 ▶}
      2 => {#6269 ▶}
    ]
    +"numReviews": 3
    +"canonicalUrl": "http://127.0.0.1:8007/directories/information-technologies/telegram-channels/crypto-donut"
  }
  1 => {#6272 ▶}
  2 => {#6266 ▶}
  3 => {#6248 ▶}
]
```

#### Пример использования: <a name="similar-companies-example" />

```HTML
[similarItems]
itemDirectorySlug = "{{ :item_slug }}"
recordsPerPage = 4

==
{% set items = similarItems.getItems() %}

{% for item in items %}
    <img data-src="{{ item.logoPath |resize(100, auto) }}" alt="{{ item.companyName}} logotype">
    <a href="{{'directory/profile' | page({ item_slug: item.companySlug })  }}">{{ item.companyName}}</a>
    Количество отзывов: {{ item.numReviews }}
    Количество просмотров: {{ item.numViews }}

    {% for field in item.sectionCustomFields %}
      <span>{{field.name}} - {{field.value}}</span><br>
    {% endfor %}
{% endfor %}
```

### Directory Items <a name="directory-items"/>

This component allows you to display directory companies.

**Component Name:**[`directoryItems`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/DirectoryItems.php)

**Parameters:**

- `directorySlug = "{{ :directory_slug }}"` - for to define directory slug.
- `itemsPerPage` - for to define number of companies.

**Methods:**

- `getDirectoryItems()` - You need to use this to get items for the directory.

#### Usage Example for Front-End: <a name="directory-items-example" />

```html
[directoryItems]
directorySlug = "{{ :directory_slug }}"
itemsPerPage = 10

{% set items = directoryItems.getDirectoryItems %}
{{ dump(items|raw) }}

    {% for item in items %}
    <b>{{ item.name }}</b>
    <b>{{ item.id }}</b>
{% endfor %}
```

### Последние отзывы на компании <a name="last-reviews"/>

Выбирает из базы компании, которые находятся в одинаковых разделах с данной компанией

**Название компонента:** [`lastReviews`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/LastReviews.php)

**Параметры:**

- `reviewsPerPage` - Количество отзывов
- `orderBy = "name/rating/created_at/created_at_display/useful"` - поле по которому надо отсортировать отзывы
- `orderDirection = "asc/desc"` - направление сортировки (по возростанию / по убыванию)

**Методы:**

- `getReviewsList()` - возвращает коллекцию отзывов о компаниях, отсортированную и выбранную по полю, указанному в параметре компонента

#### Пример использования: <a name="last-reviews-example" />

```HTML
<!-- компонент -->
[lastReviews]
reviewsPerPage = 9
orderBy = "created_at"
orderDirection = "desc"

==

{% set reviews = lastReviews.getReviewsList %}

{% for review in reviews %}
    Отзыв {{ review.name }} о {{ review.itemDirectory.name }}
    Оценка: {{ review.rating }}
    Создан: {{ review.created_at_display}}
    {{ review.content }}
{% endfor %}
```

### Отзывы пользователя <a name="user-reviews"/>

Расширяет компонент UserProfile плагина BTDev.UserProfile. Добавляет возможность получить отзывы пользователя, выбранного по его md5_id

**Название компонента:** [`userReviews`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/UserReviews.php)

**Параметры:**

- `md5_id` - идентификатор пользователя, которого надо найти
- `reviewsPerPage` - количество отзывов пользователя на страницу

**Методы:**

- `getApprovedReviews()` - возвращает коллекцию отзывов пользователя
- `getApprovedReviewsPaginated()` - возвращает пагинированную коллекцию отзывов пользователя

#### Пример использования: <a name="user-reviews-example" />

```HTML
<!-- компонент -->
[userReviews componentUserReviews]
md5_id = "{{ :md5_id }}"
reviewsPerPage = 15

==
<!-- Здесь переменную не стоит называть просто user, так как скорее всего на странице уже используется компоненты плагина RainLab.User, которые проверяют залогинен ли пользователь и уже создают переменную user на странице. -->
<!-- И чтобы не путались данные залогиненного пользователя и пользователя которого мы выбрали по md5_id, переменную лучше назвать по другому. В примере это profileUser так как страница является публичным профилем пользователя -->
{% set profileUser = componentUserReviews.getUser %}

Имя: {{ profileUser.name }}
Всего отзывов: {{ profileUser.getApprovedReviews.count }}

{% set reviews = componentUserReviews.getApprovedReviewsPaginated %}
{% for review in reviews %}
    Отзыв {{ review.name }} о компании {{ review.itemDirectory.name }}:
    {{ review.content }}
{% endfor %}
```

### Кнопка "Полезный отзыв" <a name="useful-review"/>

Добавляет на страницу функционал кнопки отметить отзыв полезным

**Название компонента:** [`reviewsUseful`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/ReviewsUseful.php)

**Методы:**

- `onUsefulChange()` - обработчик нажатия на кнопку "Полезно". - Добавляет "лайк" пользователя или отменяет его.

#### Пример использования: <a name="useful-review-example" />

```HTML
<!-- компонент -->
[reviewsUseful componentReviewsUseful]

==

{% for review in reviews %}
    Отзыв {{ review.name }} о компании {{ review.itemDirectory.name }}:
    {{ review.content }}

    {% partial componentReviewsUseful~'::default' review = review isUseful = review.isUsefulChecked() %}

{% endfor %}
```

### Пользовательский список каталогов <a name="user-directory-list"/>

Название зарезервировано, но функционал не реализован. Будет выводить пользовательский список каталогов по названию списка

**Название компонента:** [`directoryList`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/DirectoryList.php)

### Пользовательский список разделов<a name="user-section-list"/>

Позволяет выводить пользовательский список разделов, выбранный по названию списка

> Возвращает масив с данными о разделах с кеша, если в кеше данных нет, выполняеться запрос и кеширует его. Данные в кеше храняться 2 часа(7200 секунд).

**Название компонента:** [`sectionList`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/SectionList.php)

**Параметры:**

- `listId` - идентификатор списка (название списка в базе)
- `limit` - количество выводимых разделов из списка

**Методы:**

- `getSections()` - возвращает данные в виде масива, о каждом выбраном разделов

#### Структура возвращаемых данных из ``getSections()``:

```PHP
array:2 [▼
  0 => {#6090 ▼ // instance of stdClass
    +"sectionName": "Биржевые Брокеры"
    +"sectionSlug": "brokers"
    +"directoryName": "Финансы"
    +"directorySlug": "finance"
    +"numberOfItemsDirectory": 1925
    +"sectionPicturePath": "http://127.0.0.1:8007/storage/app/uploads/public/619/61a/9e4/61961a9e48f6f574265308.png"
  }
  1 => {#6096 ▶}
]
```

#### Пример использования: <a name="user-section-list-example" />

```HTML
<!-- компонент -->
[sectionList]
listId = 'priority-home-search'
limit = 6

==
{% set items = sectionList.getSections() %}

{% for item in items %}

  <div>
      <a href="{{ 'directories/section'|page({ directory: item.directorySlug, section: item.sectionSlug }) }}">
        <img alt="category-icon" src="{{ item.sectionPicturePath | default('assets/build/img/main/no-logo-company.png'| theme) }}">
      </a>
  </div>
  <div>
      <a href="{{ 'directories/section'|page({ directory: item.directorySlug, section: item.sectionSlug }) }}">{{ item.sectionName }}</a>
      <a href="{{ 'directories/directory'|page({ directory: item.directorySlug }) }}">{{ item.directoryName }}</a>
      <p>{{ item.numberOfItemsDirectory }} компаний</p>
  </div>

{% endfor %}
```

### Список выбранных компаний <a name="user-company-list"/>

Вывод пользовательских списков компаний. Списки состаляются вручную

> Возвращает масив с данными о компаниях с кеша, если в кеше данных нет, выполняеться запрос и кеширует его. Данные в кеше храняться 2 часа(7200 секунд).

**Название компонента:** [`itemDirectoryList`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/ItemDirectoryList.php)

**Параметры:**

- `listId` - идентификатор списка (название списка в базе)
- `limit` - количество выводимых компаний из списка
- `reviewsLimit` - количество отзывов о компаниях из списка

**Методы:**

- `getDirectoryItems()` - возвращает данные в виде масива, о каждой выбраной компании
- `getItemDirectoryListReviews()` - возвращает коллекцию отзывов о компаниях из списка

#### Структура возвращаемых данных из ``getDirectoryItems()``:

```PHP
array:5 [▼
  0 => {#6084 ▼ // instance of stdClass
    +"companyName": "Alpari"
    +"companySlug": "alpari"
    +"companyDisplayRating": "0.00"
    +"sectionSlug": "forex"
    +"sectionName": "Форекс брокеры"
    +"directorySlug": "finance"
    +"directoryName": "Финансы"
    +"logoPath": "http://127.0.0.1:8007/storage/app/uploads/public/624/ea9/e40/624ea9e40f76b642520784.png"
    +"numReviews": 1
    +"canonicalUrl": "http://127.0.0.1:8007/directories/finance/forex/alpari"
  }
  1 => {#6091 ▶}
  2 => {#6092 ▶}
]
```

#### Пример использования: <a name="user-company-list-example" />

```HTML
<!-- компонент -->
[itemDirectoryList priorityCompanies]
listId = "priority-companies"
limit = 6

==
{% set items = priorityCompanies.getDirectoryItems() %}

{% for item in items %}

    <a href="{{ item.canonicalUrl }}">{{ item.companyName }}</a>
    <img src="{{ item.logoPath | resize(60, 30, { mode: 'fit' }) }}" alt="logo {{ item.companySlug }}"/>
    <p>Rating: {{ item.companyDisplayRating }} / Number approved reviews: {{ item.numReviews }}</p>

    <p>Primary Section: {{ item.sectionName }}{{ item.sectionSlug }}</p>
    <p>Directory: {{ item.directoryName }}{{ item.directorySlug }}</p>


{% endfor %}
```

### Автозаполнение поля адрес в формах <a name="user-company-list"/>

Позволяет в формах использовать поле для ввода адреса, которое будет автодополняться из гугл-сервисов и при сохранении парсить адрес и отдельно схоранять страну, город

**Название компонента:** [`addressFinder`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/AddressFinder.php)

> Для работы компонента необходимо чтобы был установлен плагин Rainlab.Location. Также в админке необходимо перейти в Settings -> Location Settings на вкладку Credentials и в поле Google Maps API Key ввести ключ

#### Пример использования: <a name="user-company-list-example" />

```HTML
<!-- компонент -->
[addressFinder]

==
<form>
    <input
        name="address"
        type="text"
        data-control="location-autocomplete"
        data-country-restriction="ru, ua, us, kz, ch, pl"
        data-input-city="#locationCity"
        data-input-state="#locationState"
        data-input-country="#locationCountry"
    />
    <input type="hidden" name="city" value="" id="locationCity" class="form-control"/>
    <input type="hidden" name="state" value="" id="locationState" class="form-control"/>
    <input type="hidden" name="country" value="" id="locationCountry" class="form-control"/>
</form>
```

### Телефонный справочник <a name="phonebook-component"/>

Название компонента: ``Phonebook``

Фома для поиска телефонов на Front End. Если в базе номера нет, он создается и пользователя перенапрялет на только что созданную страницу с этим телефоном.

#### Пример использование

```HTML
[checkPhone]
==
<form data-request="onCheckPhone">
  <input  type="hidden" value="1" name="ItemPhone">
  <input type="tel" name="phone>
  <button type="submit" role="button">Проверить</button>
</form>
```
### Отзывы раздела <a name="last-section-reviews"/>

Выводит отзывы определенного раздела. Данные в кеше храняться 2 часа(7200 секунд)

**Название компонента:** [`lastSectionReviews`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/LastSectionReviews.php)

**Параметры:**

- `sectionSlug = "{{ :section_slug }}"` - слаг раздела
- `reviewsPerPage` - количество отзывов на странице
- `orderBy = "rating/created_at/created_at_display"` - поле по которому надо отсортировать выводимые отзывы
- `orderDirection = "asc/desc"` - направление сортировки (по возростанию / по убыванию)

**Методы:**

- `getReviewsList()` - возвращает коллекцию отзывов о компаниях, отсортированную и выбранную по полю, указанному в параметре компонента

#### Пример использования: <a name="last-section-reviews-example" />

```HTML
<!-- компонент -->
[lastSectionReviews]
sectionSlug={{ :section_slug }}
reviewsPerPage = 10
orderBy = "created_at_display"
orderDirection = "desc"

{% set reviews = lastSectionReviews.getReviewsList %}

{% for review in reviews %}
    Отзыв {{ review.name }} о {{ review.itemDirectory.name }}
    Оценка: {{ review.rating }}
    Создан: {{ review.created_at_display}}
    {{ review.content }}
{% endfor %}
```

### Отзывы каталога <a name="last-directory-reviews"/>

Выводит отзывы определенного каталога. Данные в кеше храняться 2 часа(7200 секунд)

**Название компонента:** [`lastDirectoryReviews`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/lastDirectoryReviews.php)

**Параметры:**

- `directorySlug = "{{ :directory_slug }}"` - слаг каталога
- `reviewsPerPage` - количество отзывов на странице
- `orderBy = "rating/created_at/created_at_display"` - поле по которому надо отсортировать выводимые отзывы
- `orderDirection = "asc/desc"` - направление сортировки (по возростанию / по убыванию)

**Методы:**

- `getReviewsList()` - возвращает коллекцию отзывов о компаниях, отсортированную и выбранную по полю, указанному в параметре компонента

#### Пример использования: <a name="last-directory-reviews-example" />

```HTML
<!-- компонент -->
[lastDirectoryReviews]
directorySlug={{ :directory_slug }}
reviewsPerPage = 10
orderBy = "created_at_display"
orderDirection = "desc"

{% set reviews = lastDirectoryReviews.getReviewsList %}

{% for review in reviews %}
    Отзыв {{ review.name }} о {{ review.itemDirectory.name }}
    Оценка: {{ review.rating }}
    Создан: {{ review.created_at_display}}
    {{ review.content }}
{% endfor %}
```

### Раздел "вопросы и ответы" определенной секции <a name="directory-section-faq"/>

Выводит вопросы и ответы определенной секции.

**Название компонента:** [`faqsSection`](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/FaqSection.php)

**Параметры:**

- `itemSlug = "{{ :item_slug }}"` - слаг компании 
- `limit` - количество элементов на странице. 

**Методы:**

- `getFaqsList()` - возвращает коллекцию вопросов и ответов

#### Пример использования: <a name="directory-section-faq-example" />

```HTML
<!-- компонент -->
[faqsSection]
itemSlug = "{{ :item_slug }}"
limit = "6"


{% for faqsSection.getFaqsList() in item %}
    Вопрос: {{ item.question }}
    Ответ: {{ item.answer }}
{% endfor %}
```


## База данных <a name="database"/>

### Схема таблиц и связей <a name="db-scheme"/>
![изображение](https://user-images.githubusercontent.com/6684336/171170607-7e51fc0d-3598-4c95-9d77-6160c10f6b05.png)

### Модели <a name="models"/>
1. Каталог
   ```Directory```

2. Раздел каталога
   ```Section```

3. Элемент каталога:
   ```ItemDirectory``` - элемент каталога, имеет базовый набор параметров. Элементом может быть _компания_, _продукт питания_, _услуга_, _фильм_ и тд
   ```ItemSection``` - связывающая таблица (pivot) элемента с разделом каталога. Создается при добавлении компании в раздел. Хранит в себе расширенные (определенные для этого раздела) параметры элемента каталога (```ItemDirectory```).

4. Пользовательские списки:
   ```LIDirectory``` - пользовательский список каталогов
   ```LISection``` - пользовательский список разделов
   ```LIItemDirectory``` - пользовательский список элементов каталога, используется для формирования списков элементов для FronEnd блоков - "10 лучших компаний", "Лучшие фильмы раздела" и тд

#### Примеры записей в БД при создании элемента и внесение его в раздел каталога:
Элемент создан, но не занесен ни в один из разделов каталога:
1. ```ItemDirectory```

Элемент создан и занесен в один разделе каталога:
1. ```ItemDirectory```
2. ```ItemSection```

Элемент создан и занесен в два раздела каталога:
1. ```ItemDirectory```
2. ```ItemSection```
3. ```ItemSection```

## Управление каталогом <a name="directory-managment"/>

### Баннеры раздела для профиля компании <a name="directory-banners"/>

Название компонента: [ItemDirectoryProfile](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/ItemDirectoryProfile.php#L18)

Позволяет получить баннер(ы) для раздела

> Файлы баннеров обязательно должны храниться в следующей папке: [partials/banners](https://github.com/VadimIzmalkov/oc-vsyapravda-theme/tree/master/partials/banners)

#### Методы:

* [getSectionBanner(string $bannerName)](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/ItemDirectoryProfile.php#L303) - метод для получения баннера раздела, по указанному имени

#### Пример использования:

```HTML
[itemDirectoryProfile]
directorySlug = "{{ :directory_slug }}"
sectionSlug = "{{ :section_slug }}"
itemDirectorySlug = "{{ :item_directory_slug }}"
==
{% set section_banner_path = itemDirectoryProfile.getSectionBanner('Name') %}

{% partial section_banner_path %}
```

* [getApprovedAndMyReviewsWithRating()](https://github.com/VadimIzmalkov/oc-directory-plugin/blob/master/components/Item.php#L189) - возвращает процентное соотношение отзывов

```HTML
{% for reviews in componentItemDirectoryProfile.getApprovedAndMyReviewsWithRating %}
    <div>{{ reviews.humanReadableLabel }}</div>
    <div role="progressbar" aria-valuenow="{{ reviews.percent }}" aria-valuemin="0" aria-valuemax="100"></div>
    <div>{{ reviews.percent|round(0) }}%</div>
{% endfor %}
```

## Настройки <a name="settings"/>

### Каталог <a name="settings-directory"/>
URL-параметры используемые для страниц каталога:

- `:directory_slug` - параметр каталога
- `:section_slug` - параметр раздела каталога
- `:item_directory_slug` - параметр профиля

#### Google Maps API key
Для работы автоподсказок адресов (напр, при создании/редактировании компании) необходимо указать [Google Maps API key](https://developers.google.com/maps/documentation/javascript/get-api-key) в настройках плагина [RainLab.Location](https://github.com/rainlab/location-plugin) ``Settings > Location settings``

![изображение](https://user-images.githubusercontent.com/6684336/196155209-292d8b42-464f-45b8-93b8-bc0494bd27d5.png)

### Телефонный справочник <a name="settings-phonebook"/>
В выпадающем списке **CMS page for phones** выбиратеся CMS страница телефоного спраочника.
Старница создается и хранится в папке [directory](https://github.com/VadimIzmalkov/oc-vsyapravda-theme/tree/master/pages/directory).

## Рекомендации для FrontEnd <a name="frontend-recommendations"/>

## Решение других проблем с каталогом <a name="solving-other-problems"/>
### Востановление мета данных компаний <a name="recover-meta"/>
В случае потери мета данных всех компаний на сайте, можно запустить следующую комманду в консоли
```BASH
php artisan directory:use-item-metainfo-from-section
```
Эта команда в цикле перебирает все компании и если у них установлена главная секция то включает настройку "Получить метаданные из секции" (Use meta info from section)
Дополнительно главная секция компании определяется как источник шаблона для формирования мета данных.
