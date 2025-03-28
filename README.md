
# BlogExtension Plugin

- [Требования](#require)
- [Возможности и компоненты](#features)
- [Модели](#models)


## Требования <a name="require"/>
- плагин RainLab.Blog
- плагин GinoPane.BlogTimeToRead
- плагин Vdomah.BlogViews

## Возможности и компоненты <a name="features"/>
Плагин является дополнением к плагину RainLab.Blog. Основные дополнения:
- [возможность пользователю бэкэнда быть автором поста, добавляет профиль автора](#post-author)
- [навигация по постам](#post-navigation)
- [цветовой код HEX для каждой категории](#hex-code)
- [время чтения поста](#post-time)
- [количество просмотров поста](#count-show-post) 
- [Компоненты](#components)
    - [вывод всех авторов постов](#component-autor-list)
    - [вывод полной информации об авторе](#component-autor-profile)
    - [Форма создания/редактирования компании](#company-crud)
    - [Список всех каталогов](#directory-list)
    - [Каталог, список разделов каталога](#section-list)
    - [Раздел каталога (список компаний по slug раздела)](#company-list-slug)
    - [Раздел каталога (список компаний по id раздела)](#company-list-id)
    - [Профиль компании](#item-profile)
    - [Похожие компании](#similar-companies)


### Возможность пользователю backend быть автором поста, добавляет профиль автора <a name="post-author"/>
 
Автор - это новая роль в BackendUser. 
Ее нужно создать в настройках OctoberCMS: Администраторы -> Управление ролями -> Новая роль с такими параметрами:
- **name: Author**
- **code: author**
- **permissions: blog/manage the blog posts(allow)**

После этого при изменении или добавлении нового пользователя для backend будет доступен выбор этой роли. 

Чтобы назначить автора блога, вам нужно перейти на вкладку Blog(Блог) в OctoberCMS и после выбора поста для редактирования, нажать вкладку  Manage( Управление) -> Published by(Опубликовано) и выбрать имя нужного автора.

### Навигация по записям в блоге <a name="post-navigation">
Это расширение получает весь текст записи, проверяет его и находит заголовки h1, h2. Если они были найдены, генерирует массив JSON с этими заголовками и их URL.
Для отображения навигации необходим файл, в котором содержимое поста отображается после этого кода:

```
<ul>
    {% for post in post.navigation %}
        {% if post.level == 2 %}
            <li>
                <a href="#{{ post.id }}" data-target="#{{ post.id }}">
                    {{ post.innerHtml }}
                </a>
            </li>
            {% else %}
                <li>
                    <a href="#{{ post.id }}" data-target="#{{ post.id }}" >
                        {{ post.innerHtml }}
                    </a>
                </li>
        {% endif %}
    {% endfor %}
</ul>
```
где:
- **post.level** - тип заголовков (h1 = 1, h2 = 2).
- **post.innerHtml** - текст заголовков.
- **post.id** - URL-адрес в тексте для этого заголовка.

### Цветовой код HEX для каждой категории <a name="hex-code">

Это свойство расширяет модель категории и может быть добавлено как свойство по умолчанию.
```
{{ category.color }}
// Вывод будет таким: #000000
```
Для выбора цвета для категории, нужно перейти на страницу Blog(Блог) -> Categories(Категории) и выбрать для редактирования категорию. 

### Время чтения поста <a name="post-time">

Это расширение показывает время чтения постов. Для его работы необходима установка плагина GinoPane.BlogTimeToRead
Пример использования:
```
[timeToReadExtended]
==
... 
Время чтения {{ timeToReadExtended.MINTS}} минут
... 
```

### Количество просмотров поста <a name="count-show-post">

Необходим плагин Vdomah.BlogViews
Пример использования:
```
... 
{{ post.viewsBt }}
... 
```
Свойство viewsBt кешируется на 2 часа.


## Компоненты <a name="components"/>

### Вывод всех авторов постов <a name="component-autor-list">

Вывод список всех авторов постов.

> Возвращает масив с данными о всех авторов постов.

**Название компонента:** [``authorsList``](https://github.com/VadimIzmalkov/oc-blogextension-plugin/blob/main/components/AuthorsList.php)

**Методы:**

- `getAuthorsList()` - возвращает **данные** в виде масива, о всех авторов

#### Структура возвращаемых данных из ``getAuthorsList()``:

<pre><code>array:[
  0 => <a href="#backendmodelsuser" title="Backend\Models\User">Backend\Models\User</a>
  ...
]</code></pre>

#### Пример использования

```
[authorsList]

==
{% set authors = authorsList.getAuthorsList() %}
{% for person in authors %}
    <img src="{{person.getAvatarThumb}}" alt="avatar"><br>
    Nickname <a href="{{ 'testauthor'|page({ slug:person.author.nickname }) }}">{{person.author.nickname}}</a>
    About author {{person.author.about|raw}} 
    Full name (if not displays check is set in admin){{person.author.user.getFullNameAttribute}}<br>
{% endfor %}
```

### Вывод полной информации об авторе <a name="component-autor-profile">

Вывод информации о конкретном авторе, включая все его посты и новости

**Название компонента:** [``authorProfile``](https://github.com/VadimIzmalkov/oc-blogextension-plugin/blob/main/components/AuthorProfile.php)

**Параметры:**

- `nickname = "{{ :slug }}"` - nickname автора
- `postsPerPage` - количество постов на странице
- `newsPerPage` - количество новостей на странице

**Методы:**

- `getPostsList()` - возвращает **данные** в виде масива, постов пользователя 
- `getNewsList()` - возвращает **данные** в виде масива, новостей пользователя 

#### Структура возвращаемых данных из ``getPostsList()``:

<pre><code>array:[
  0 => <a href="#rainlabblogmodelspost" title="RainLab\Blog\Models\Post">RainLab\Blog\Models\Post</a>     
  ...
]</code></pre>


#### Пример использования

```
[authorsList]

==
{% set authors = authorsList.getAuthorsList() %}
{% for person in authors %}
    <img src="{{person.getAvatarThumb}}" alt="avatar"><br>
    Nickname <a href="{{ 'testauthor'|page({ slug:person.author.nickname }) }}">{{person.author.nickname}}</a>
    About author {{person.author.about|raw}} 
    Full name (if not displays check is set in admin){{person.author.user.getFullNameAttribute}}<br>
{% endfor %}
```






## Модели данных <a name="models">
### Backend\Models\User<a name="Backend_Models_User">

<pre><code>{
  "id": 40
  "first_name": "Тест"
  "last_name": "Тестович"
  "login": "Тест Тестович"
  "email" : "mail@test.com"
  "permissions" : ""
  "is_activated":false,
  "role_id":5,
  "activated_at":null,
  "last_login":null,
  "created_at":"2022-04-26T15:43:50.000000Z",
  "updated_at":"2022-04-26T15:43:50.000000Z",
  "deleted_at":null,
  "is_superuser":0,
  "is_password_expired":0,
  "password_changed_at":null,
  "author": BTDev\BlogExtension\Models\Author {
	  "id":1,
	  "nickname":"test-testovich",
	  "about":"",
	  "backend_user_id":20,
	  "deleted_at":null,
	  "created_at":"2022-04-26T15:48:40.000000Z",
	  "updated_at":"2022-04-27T17:10:22.000000Z"		
  }  
}</code></pre>

### RainLab\Blog\Models\Post<a name="RainLab_Blog_Models_Post">

<pre><code>{
  "id": 2,
  "user_id": 29,
  "title": "",
  "slug": "kak-moshenniki-pytayutsya-sobirat-nedoimki-po-nalogam",
  "excerpt": "",
  "content": " ",
  "content_html": "", 
  "published_at": "2021-10-28T15:00:00.000000Z",
  "published": 1,
  "created_at": "2021-10-28T12:53:34.000000Z",
  "updated_at": "2022-05-13T18:10:53.000000Z",
  "metadata":null,
  "navigation":[{
    "id":0,
    "level": "",
    "innerText": "",
    "innerHtml": "",
    "outerHtml": ""
  }],
  "summary":"",
  "has_summary":true,
  "user":<a href="#backendmodelsuser" title="Backend\Models\User">Backend\Models\User</a>,
 }</code></pre>
