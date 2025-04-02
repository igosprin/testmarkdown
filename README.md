
# BlogExtension Plugin


- [Краткое описание](#brief_description)
- [Возможности](#features)
- [Компоненты](#components)
- [Требования](#require)
- [Модели](#models)


## Краткое описание <a name="brief_description"/>
Плагин является расширением базовых плагинов RainLab.Blog,GinoPane.BlogTimeToRead,Vdomah.BlogViews,Indikator.News и Rahman.Blogtags. 
 
## Возможности <a name="features"/> 
- [Авторы постов блога и новостей](#post-author)
- [Оглавление (table of contents) к посту](#post-navigation)
- [Пользовательские списков постов](#create-post-list) 
- [Персональный цвета для разделов блога](#hex-code)
- [Время чтения поста](#post-time)
- [Кеширование количества просмотров поста](#count-show-post) 
- Прикрепленные/избранные посты
- Кеширование списка постов

### Авторы постов блога и новостей <a name="post-author"/>
 
Автор - это новая роль в BackendUser. 
Ее нужно создать в настройках OctoberCMS: Администраторы -> Управление ролями -> Новая роль со следующими параметрами:
- **name: Author**
- **code: author**
- **permissions: blog/manage the blog posts(allow)**

После этого при изменении или добавлении нового пользователя для backend будет доступен выбор этой роли. 

Чтобы назначить автора блога, вам нужно перейти на вкладку Blog(Блог) в OctoberCMS и после выбора поста для редактирования, нажать вкладку  Manage( Управление) -> Published by(Опубликовано) и выбрать имя нужного автора.

### Оглавление (table of contents) к посту <a name="post-navigation">
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

### Персональный цвета для разделов блога <a name="hex-code">

Это свойство расширяет модель категории и может быть добавлено как свойство по умолчанию.
```
{{ category.color }}
// Вывод будет таким: #000000
```
Для выбора цвета для категории, нужно перейти на страницу Blog(Блог) -> Categories(Категории) и выбрать для редактирования категорию. 

### Время чтения поста <a name="post-time">

Это расширение показывает время чтения постов. Для его работы необходима установка плагина GinoPane.BlogTimeToRead.

Пример использования:

```HTML
[timeToReadExtended]
==
... 
Время чтения {{ timeToReadExtended.MINTS}} минут
... 
```

### Количество просмотров поста <a name="count-show-post">

Необходим плагин Vdomah.BlogViews

Пример использования:

```HTML
... 
{{ post.viewsBt }}
... 
```
Свойство viewsBt кешируется на 2 часа.


### Пользовательские списков постов <a name="create-post-list">

Добавлена возможность создавать списки постов. Для создания списка необходимо перейти на страницу Blog(Блог) -> PostLists.

Использование: см. компонент [postList](#post-list)

## Компоненты <a name="components"/> 
  - [Список авторов](#component-autor-list)
  - [Информация об авторе](#component-autor-profile)
  - [Пользовательский список](#post-list)
  - [Список постов со всех категорий](#blog-posts-categories)
  - [Список постов в случайно порядке](#blog-posts-similar-random)
  - [Список постов с параметрами](#blog-posts)

### Список авторов <a name="component-autor-list">

Компонент выводит список всех авторов постов.

**Название компонента:** [``authorsList``](https://github.com/VadimIzmalkov/oc-blogextension-plugin/blob/main/components/AuthorsList.php)

**Методы:**

- `getAuthorsList()` - возвращает массив с **данными** о всех авторов постов

#### Структура возвращаемых данных из ``getAuthorsList()``:

<pre><code>array:[
  0 => <a href="#backendmodelsuser" title="Backend\Models\User">Backend\Models\User</a>
  ...
]</code></pre>

#### Пример использования

```HTML
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

### Информация об авторе <a name="component-autor-profile">

Компонент выводит информацию о конкретном авторе, включая все его посты и новости

**Название компонента:** [``authorProfile``](https://github.com/VadimIzmalkov/oc-blogextension-plugin/blob/main/components/AuthorProfile.php)

**Параметры:**

- `nickname = "{{ :slug }}"` - nickname автора
- `postsPerPage` - количество постов на странице
- `newsPerPage` - количество новостей на странице

**Свойства:**

- `author` - возвращает **данные** об авторе
- `authorsPostsCount` - возвращает **данные** в виде числа, количество постов автора
- `authorPostCategories` - возвращает **данные** в виде массива, категорий постов автора
- `authorsNewsCount` - возвращает **данные** в виде числа, количество новостей автора
- `authorNewsCategories` - возвращает **данные** в виде массива, категорий новостей автора
- `authorPostTags` - возвращает **данные** в виде массива, тэги постов автора

#### Структура возвращаемых данных из ``author``:

<pre><code>BTDev\BlogExtension\Models\Author {
	  "id":1,
	  "nickname":"test-testovich",
	  "about":"",
	  "backend_user_id":20,
	  "deleted_at":null,
	  "created_at":"2022-04-26T15:48:40.000000Z",
	  "updated_at":"2022-04-27T17:10:22.000000Z",
          "user": <a href="#backendmodelsuser" title="Backend\Models\User">Backend\Models\User</a>
 }</code></pre>
 
 #### Структура возвращаемых данных из ``authorPostCategories``:

```
array:[
    "category-1-slug"=>"Category 1",
    "category-2-slug"=>"Category 2",
    ...
]
```
#### Структура возвращаемых данных из ``authorNewsCategories``:


```
array:[
    "category-1-slug"=>"Category 1",
    "category-2-slug"=>"Category 2",
    ...
]
```

#### Структура возвращаемых данных из ``authorPostTags``:


```
array:[
    "tag-1-slug"=>"tag 1",
    "tag-2-slug"=>"tag 2",
    ...
]
```

**Методы:**

- `getPostsList()` - возвращает **данные** в виде массива, постов пользователя 
- `getNewsList()` - возвращает **данные** в виде массива, новостей пользователя 

#### Структура возвращаемых данных из ``getPostsList()``:

<pre><code>array:[
  0 => <a href="#rainlabblogmodelspost" title="RainLab\Blog\Models\Post">RainLab\Blog\Models\Post</a>     
  ...
]</code></pre>

#### Структура возвращаемых данных из ``getNewsList()``:

<pre><code>array:[
  0 => <a href="#indikatornewsmodelsposts" title="Indikator\News\Models\Posts">Indikator\News\Models\Posts</a>     
  ...
]</code></pre>



#### Пример использования

```HTML
[authorProfile]
nickname = "{{ :slug }}"
postsPerPage = 20
newsPerPage = 6

==
{% set posts = authorProfile.getPostsList() %}
{% set news_list = authorProfile.getNewsList() %}

<img class="g-width-100 g-height-100 g-mr-20 g-rounded-5" src="{{ author.user.getAvatarThumb(80)}}" alt="Author"/>
{{ author.nickname }} <br>
{{ author.user.getFullNameAttribute }} <br>
{{ author.user.email }} <br>
{{ author.about|raw }} <br>

List of posts {{ authorsPostsCount }}<br>
{% for post in posts %}
  {{post.title}} <br>
{% endfor %}    
----
List news {{authorsNewsCount}}<br>
{% for news in news_list %}
  {{news.title}} <br>
{% endfor %}
Tags:<br>
{% for tag in authorPostTags %}
  {{ tag }}
{% endfor %}

```


### Пользовательский список <a name="post-list">

Компонент выводит посты из пользовательского списка.

**Название компонента:** [``postList``](https://github.com/VadimIzmalkov/oc-blogextension-plugin/blob/main/components/PostList.php)

**Параметры:**

- `listId` - id списка
- `limit` - количество постов 
 
**Методы:**

- `getPostListItems()` - возвращает **данные** в виде массива, постов в списке

#### Структура возвращаемых данных из ``getPostListItems``:

<pre><code>array:[
  0 => BTDev\BlogExtension\Models\PostListItem{
    "id":1,
    "list_id":"blog-page",
    "post_id":2,
    "order":1,
    "created_at":"2022-02-21T15:48:54.000000Z",
    "updated_at":"2022-02-21T15:48:54.000000Z",
    "deleted_at":null,
    "post": <a href="#rainlabblogmodelspost" title="RainLab\Blog\Models\Post">RainLab\Blog\Models\Post</a>
  }     
  ...
]</code></pre>

#### Пример использования

```HTML
[postList]
listId = "blog-page"
limit = 2

==
{% set postListItems = postList.getPostListItems %}
{% for postListItem in postListItems %}
  <a href="{{ 'blog/post'|page({slug:postListItem.post.slug}) }}">{{postListItem.post.title}}</a>
  {{postListItem.post.title}}
{% endfor %}
```

### Список постов со всех категорий <a name="blog-posts-categories">

Компонент выводит определенное количества постов из всех категорий. Данные кешируются на 1 день.

**Название компонента:** [``blogPostsByCategories``](https://github.com/VadimIzmalkov/oc-blogextension-plugin/blob/main/components/PostsByCategories.php)

**Параметры:**

- `limit` - количество постов по каждой категории. Значение по умолчанию: 6
 
**Свойства:**

- `categories` - возвращает **данные** в виде массива, список постов

#### Структура возвращаемых данных из ``categories``:

<pre><code>array:[
  0 => RainLab\Blog\Models\Category{
    "id":2,
    "name":"name Category",
    "slug":"fraud",
    "code":null,
    "description":"",
    "parent_id":null,
    "nest_left":1,
    "nest_right":2,
    "nest_depth":0,
    "created_at":"2020-12-24T15:47:12.000000Z",
    "updated_at":"2020-12-24T15:47:14.000000Z",
    "color_category":null,
    "page_title":"",
    "meta_title":"",
    "meta_description":"",
    "url":"",
    "posts": array:[
        0 => <a href="#rainlabblogmodelspost" title="RainLab\Blog\Models\Post">RainLab\Blog\Models\Post</a>,
        ...
    ]
    },       
  ...
]</code></pre>

#### Пример использования

```HTML
[blogPostsByCategories]
limit = 2

==
{% for category in categories %}
  category:{{category.name}}</br>
  list posts:  
  {% for post in category.posts %}     
    <a  href="{{ 'blog/post'|page({ slug: post.slug }) }}">                                
        {{ post.title }}                                
    </a>
  {% endfor %}
{% endfor %}
```

### Список постов в случайно порядке <a name="blog-posts-similar-random">

Компонент выводит посты, упорядоченные случайным образом(Random). Данные кешируются на 20 минут.

**Название компонента:** [``blogPostsSimilar``](https://github.com/VadimIzmalkov/oc-blogextension-plugin/blob/main/components/PostsSimilar.php)

**Параметры:**

- `limit` - количество постов по каждой категории. Значение по умолчанию: 10
- `noPostsMessage` - сообщение выводимое в случаи отсутствия постов. Значение по умолчанию: rainlab.blog::lang.settings.posts_no_posts_default
- `categoryPage` - slug для категорий постов. Значение по умолчанию: blog/category
- `postPage` - slug для постов. Значение по умолчанию: blog/post
- `curentPost` - slug или id текущего поста. Опционально.
- `category` - id категории из которых необходимо вывести посты. Опционально.
 
**Свойства:**

- `posts` - возвращает **данные** в виде массива, список постов

#### Структура возвращаемых данных из ``posts``:

<pre><code>array:[
  0 => <a href="#rainlabblogmodelspost" title="RainLab\Blog\Models\Post">RainLab\Blog\Models\Post</a>     
  ...
]</code></pre>

#### Пример использования

```HTML
[blogPostsSimilar]
limit = 6
noPostsMessage = "No posts found"
categoryPage = "blog/category"
postPage = "blog/post"
curentPost="{{ :slug }}"

==
{% for post in blogPostsSimilar.posts|slice(0, 8)%}    
    <a href="{{ post.url }}">
        {{ post.title }}
    </a>
    
{% endfor %}
```

### Спосок постов с параметрами <a name="blog-posts">

Расширение компонента blogPosts от Rainlab - добавлено кеширование данных. Время кеширования можно задавать через параметры. По умолчанию время составляет 1 час (3600) сек

**Название компонента:** [``blogPostsBTDev``](https://github.com/VadimIzmalkov/oc-blogextension-plugin/blob/main/components/Posts.php)

**Параметры:**

- `pageNumber` - номер страницы для пагинации. Значение по умолчанию: {{ :page }}
- `categoryFilter` - slug категории из которой необходимо выводить посты. Опционально
- `postsPerPage` - количество постов выводимых на странице. Значение по умолчанию: 10
- `noPostsMessage` - сообщение выводимое в случаи отсутствия постов. Значение по умолчанию: rainlab.blog::lang.settings.posts_no_posts_default
- `sortOrder = "title asc/title desc/created_at asc/created_at desc/updated_at asc/updated_at desc/published_at asc/published_at desc/random"` - slug для категорий постов. Значение по умолчанию: published_at desc. Опционально
- `categoryPage` - slug для категорий постов. Значение по умолчанию: blog/category
- `postPage` - slug для постов. Значение по умолчанию: blog/post
- `exceptPost` - ID/slug категорий, которую необходимо исключить. Опционально.
- `exceptCategories` - разделенный запятыми список slug категорий, которые необходимо исключить. Опционально.
- `cachingTime` - время кеширования в секундах. Значение по умолчанию: 3600 сек. Опционально.

 
**Свойства:**

- `posts` - возвращает **данные** в виде массива, список постов

#### Структура возвращаемых данных из ``posts``:

<pre><code>array:[
  0 => <a href="#rainlabblogmodelspost" title="RainLab\Blog\Models\Post">RainLab\Blog\Models\Post</a>     
  ...
]</code></pre>

#### Пример использования

```HTML
[blogPostsBTDev]
pageNumber = "{{ :page }}"
categoryFilter = "finance"
postsPerPage = 4
noPostsMessage = "No posts found"
sortOrder = "published_at desc"
categoryPage = "blog/category"
postPage = "blog/post"
cachingTime = 300

==
{% for post in blogPostsBTDev.posts %}    
    <a href="{{ post.url }}">
        {{ post.title }}<br/>
        {{ post.content_html|raw }}
        {% for image in post.featured_images %}
            <p>
                <img
                    data-src="{{ image.filename }}"
                    src="{{ image.path }}"
                    alt="{{ image.description }}"
                    style="max-width: 100%" />
            </p>
        {% endfor %}
    </a>
    
{% endfor %}
```


## Модели данных <a name="models">
### Backend\Models\User

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

### RainLab\Blog\Models\Post

<pre><code>{
  "id":2,
  "user_id":29,
  "title":"",
  "slug":"slug",
  "excerpt":"",
  "content":" ",
  "content_html":"", 
  "published_at":"2021-10-28T15:00:00.000000Z",
  "published":1,
  "created_at":"2021-10-28T12:53:34.000000Z",
  "updated_at":"2022-05-13T18:10:53.000000Z",
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
  "user": <a href="#backendmodelsuser" title="Backend\Models\User">Backend\Models\User</a>,
  "viewsBt":10,
  "categories":array:[
    0 => <a href="#rainlabblogmodelscategory" title="RainLab\Blog\Models\Category">RainLab\Blog\Models\Category</a>,
    ...
  ]
 }</code></pre>
 
 ### Indikator\News\Models\Posts

<pre><code>{
  "id":774,
  "title":"",
  "slug":"",
  "introductory":"",
  "content":"",
  "published_at":"2022-12-26T11:35:26.000000Z",
  "status":"1",
  "statistics":148,
  "created_at":"2022-12-26T11:36:45.000000Z",
  "updated_at":"2025-03-28T19:32:43.000000Z",
  "image":"\/news\/20221226\/ge_bank.jpeg",
  "featured":2,
  "last_send_at":null,
  "enable_newsletter_content":0,
  "newsletter_content":"",
  "user_id":29,
  "tags":null,
  "seo_title":"",
  "seo_keywords":"",
  "seo_desc":"",
  "seo_image":"",
  "content_label":0
  "user":<a href="#backendmodelsuser" title="Backend\Models\User">Backend\Models\User</a>,
 }</code></pre>
 
 ### RainLab\Blog\Models\Category

<pre><code>{
  "id":2,
  "name":"name Category",
  "slug":"fraud",
  "code":null,
  "description":"",
  "parent_id":null,
  "nest_left":1,
  "nest_right":2,
  "nest_depth":0,
  "created_at":"2020-12-24T15:47:12.000000Z",
  "updated_at":"2020-12-24T15:47:14.000000Z",
  "color_category":null,
  "page_title":"",
  "meta_title":"",
  "meta_description":"",
  "url":"",
  "posts": array:[
    0 => <a href="#rainlabblogmodelspost" title="RainLab\Blog\Models\Post">RainLab\Blog\Models\Post</a>,
    ...
  ]
 }</code></pre>

## Требования <a name="require"/>
- плагин RainLab.Blog
- плагин GinoPane.BlogTimeToRead
- плагин Vdomah.BlogViews
- плагин Indikator.News
- плагин Rahman.Blogtags
