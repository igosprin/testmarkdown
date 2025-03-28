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
