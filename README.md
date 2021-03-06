# Guidelines

## Руководство по написанию SCSS кода 

### Пробелы и отступы

* Используйте мягкие отступы с двумя пробелами — это единственный способ гарантировать, что ваш код будет везде одинаково выглядеть.
* При группировке селекторов помещайте каждый селектор на отдельную строку.
* Оставляйте один пробел перед `{` блока объявлений.
* Помещайте `}` блока объявлений на новой строке.
* Оставляйте один пробел после `:` для каждого объявления.
* Каждое объявление должно находится на отдельной строке для более точного сообщения об ошибках.
* Завершайте все объявления точкой с запятой.


### Форматирование

* Не добавляйте начальный ноль для значений (например, `.5` вместо `0.5`).
* Все 16-ичные значения записывайте строчными буквами (в нижнем регистре), например, `#fff`. 
* Используйте короткие 16-ичные значения везде, где это возможно, например, `#fff` вместо `#ffffff`.
* Всегда берите в кавычки значения атрибутов внутри селектора, например, `input[type="text"]`. 
* Опускайте единицы измерения при нулевом значении, например, `margin: 0;` вместо `margin: 0px;`.
* Используйте `0` вместо `none`, например `border: 0;` вместо `border: none;`.
* Используйте `//` для комментариев вместо `/* */`.


### Прочее

Избегайте излишней вложенности селекторов. Обычно три уровня бывает достаточно. Если трех уровней не достаточно, подумайте, возможно код стоит пересмотреть.


### Пример

```scss
/* Плохой SCSS */
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  background-color:rgba(0,0,0,0.5);
  box-shadow:0 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}
```

```scss
// Хороший SCSS
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin: 0 0 15px;
  background-color: rgba(0, 0, 0, .5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```


## Система именования БЭМ для SCSS

Существует несколько вариаций использования именований. Мы используем следующую:

```scss
.block {}
.block__element {}
.block--modifier {}
.block__element--modifier {}
```

* имена блоков, элементов и модификаторов должны быть недлинными и семантически значимыми;
* используются только маленькие латинские буквы, символ тире и цифры;
* символ подчёркивания используется как специальный разделитель.



## Правила именования

Для именования любых сущностей: файлов scss, изображений всех форматов, шаблонов html, BEM-блоков и пр. используется `kebab-case`. Это означает, что в именах используются только латинские буквы в нижнем регистре и цифры, для разделения слов используется символ тире (-). 

Символ нижнего подчеркивания (\_) может быть использован **только как первый символ** в имени scss-файла или директории блока. В этом случае scss-файл не компилируется в самостоятельный css и может быть использован только через `@import` из других файлов, либо будет полностью проигнорирован. Папка со знаком подчеркивания вначале имени игнорируется при сборке стилей и используется, чтобы исключить какие-то блоки из css, не удаляя их исходные файлы.



## Файловая структура


Все файлы проекта лежат в директории `scss` (придумать новое название и переписать), которая является корневой для всех подпроектов. Каждый проект может иметь произвольное количество подпроектов. Каждый подпроект это набор файлов scss, изображений и скриптов, сгруппированных по слоям и блокам, компилируемых в конечном счете в свой отдельный набор css-файлов, которые могут подключаться независимо друг от друга.

Примером подпроекта могут быть основные стили вебсайта, css-файлы которого подключаются на всех страницах сайта, пример другого подпроекта - промо-раздел вебсайта, css-файлы которого подключаются только на страницах этого промо-раздела.

Для создания подпроекта, в корневой директории проектов создается файл с именем подпроекта и расширением `scss` и директория с таким же именем. Все файлы, относящиеся к данному подпроекту размещаются внутри соответствующей ему директории.

Подпроекты структурируются согласно методологии ITCSS (http://itcss.io/).

Видео и слайды, отлично раскрывающие тему:

* https://speakerdeck.com/dafed/managing-css-projects-with-itcss
* https://www.youtube.com/watch?v=1OKZOV-iLj4

Согласно этой методологии, каждая директория подроекта подразделяется на еще ряд директорий, называемых слоями (layers). Каждый слой подключаются в определенной последовательности, указанной ниже. С каждым последующем слоем специфичность стилей возрастает, при этом они затрагивают все меньшее количество DOM-узлов.

В большинстве подпроектов используются следующие слои, но их количество может меняться от проекта к проекту.

* settings
* tools
* generic
* base
* objects
* components
* trumps


### settings

Файлы с глобальными переменными и настройками, затрагивающими весь подпроект, хранятся в этой директории.


### tools

Файлы с scss `@mixin`s и `@function`s, глобально используемые в подпроекте. 

Обычно все подпроекты используют один и тот же общий набор инструментов, поэтому почти всегда директория `tools` вынесена в корневую папку на один уровень с папками подпроектов и имеет название `_tools`. Нижнее подчеркивание используется, чтобы избежать ошибки и не принять эту папку за название подпроекта. При этом все подпроекты ссылаются на одну и туже корневую папку `_tools`. 


### generic

Нулевые стили проекта, глобальные резеты, normilize.css, box-sizing и т.п.


### base

HTML теги без классов, очень базовые стили для ссылок, списков, заголовоков и т.п. Это последний слой, где могут применяться селекторы по типу (type selectors), во всех последующих слоях в идеальном случае только селекторы по классам.


### objects

Максимально абстрактные, проектонезависимые, переносимые из проекта в проект (в идеале) блоки. Примером таких блоков являются OSCSS или первый базовый слой MCSS (http://operatino.github.io/MCSS/#1).

Основное правило — абстрактность как в названиях, так и в разметке. Для именования используются только классы.

Названия классов этого слоя не должны выглядеть чужеродно в любом месте на сайте.
Блоки модуля должны иметь стиль по умолчанию, но должны оставаться простыми и легко модифицируемыми под задачи различных проектных модулей.


### components

Этот слой включает в себя изолированные, проектные модули, из которых в дальнейшем собирается страница. Аналогично проектному слою MCSS (http://operatino.github.io/MCSS/#2). Для именования продолжают использоваться только классы.

Каждый модуль должен быть максимально изолирован и существовать как отдельная, независимая единица интерфейса. Изолированность модулей позволяет легко работать с их стилями, не боясь задеть отдельные части веб сайта.


### trumps

Слой для косметических классов, хелперов. Аналогичен косметическому слою MCSS (http://operatino.github.io/MCSS/#3). Очень специфичные классы, затрагивающие крайне малое количество элементов за один раз. Возможно и поощряется использование `!important` в свойствах данных классов.

Позволяет использовать простые классы, для редких, непроектных ситуаций, например нестандартный отступ или цвет ссылки.


### Пример файла подпроекта

Для описанных выше слоев, основной файл подпроекта `website` может иметь следующую структуру:

```scss
@import "website/settings/*";
@import "_tools/*";
@import "website/generic/*";
@import "website/base/*";
@import "website/objects/*";
@import "website/components/*";
@import "website/trumps/*";
```

## Добавление новых слоев

Слои подключаются в строго определенной последовательности, при этом порядок подключения файлов внутри слоя не имеет и не должен иметь значения.

В каждом конкретном подпроекте набор слоев может меняться. Новые слои могут быть добавлены при необходимости. Например, можно добавить слой `pages` для блоков, специфичных для отдельных страниц вебсайта. При подключении новых слоев необходимо следовать правилу, что в каждом последующем слое правила имеют более высокую специфичность, но затрагивают меньшее количество DOM-элементов. В случае примера `pages`, такой слой следовало бы подключать после слоя `base`, перед слоем `objects`.


## Расположение стилей

Стили каждого блока должны храниться рядом друг с другом, каждый блок должен быть выделен в отдельный файл. Имя файла совпадает с именем блока:

```scss
// _module.scss
.module { }

.module__list { }

.module-list--modificator { }
```

Селекторы, модифицируемые каскадом, должны так же храниться рядом с родительским классом:

```scss
.module { }

.module__list { }

.module__list .other-module { }
```

Исключением могут быть только классы из слоя контекста и глобальные модификаторы (`g-`):

```scss
.module { }

.g-menu-expanded .module { }

.module__list { }

.touch .module__list { }

.lt-ie9 .module__list { }
```

Модификаторы стоит прописывать после основной группы модифицируемых элементов, чтобы не создавать путаницу, которая обязательно возникнет при появлении нескольких модификаций.

```scss
.block {}

.block__element {}

.block__sub-element {}

.block--modifier {
  .block_element {}

  .block__sub-element {}
}
```

Такая организация стилей упрощает дальнейший рефакторинг кода и позволяет непосвященному разработчику быстро разобраться во всех стилях модуля и быстро приступить к работе с ним.

При чистке кода просто избавляться от старых блоков и от всех его зависимостей.


# Gulp


## Описание задач

Вся подготовка и сборка файлов фронтенда происходит при помощи gulp-задач. При помощи gulp выполняются следующие процедуры: 

* компиляция scss в css
* создание отдельных css-файлов для разных локализаций
* создание rtl-версии стилей
* оптимизация изображений
* сборка png и svg спрайтов



### Компиляция SCSS

Консольная команда `gulp`.

Данная задача выполняет:

* Компиляцию SCSS при помощи node-sass
* Группировку media-запросов
* Добавление вендорных префиксов при помощи autoprefixer.
* Создание отдельных css-файлов для разных локализаций
* Создание rtl-версии стилей


### Оптимизация изображений

Консольная команда `gulp images`.

Данная команда оптимизирует изображения форматов `png`, `jpg`, `gif`, `svg`. Оптимизированные копии исходных изображений сохранятся в отдельной директории. При этом задача обрабатывает изображение только в том случае, если изображение в целевой директории отсутствует или является более старым, чем изображение в исходной директории.

Данная задача игнорирует png и svg файлы, предназначенные для объединения в спрайты.

При удалении изображения в иходной директории, копия в целевой директории сохраняется и требует ручного удаления.


### Создание png-спрайтов

Конольная команда `gulp sprites`.

Данная команда создает спрайты из png-изображений и соответвущие им scss-файлы. Для генерации спрайта необходимо соблюсти ряд правил в именовании директории.

Обработаны будут только директории, имя которых начинается с префикса, указанного в файле конфигурации (по умолчанию `sprite-`). Каждая такая директория должна содержать только файлы в формате png. Для каждой директории будет создан свой спрайт объединенных изображений и scss-файл стилей.

Если название директори заканчивается на `@2x`, то для нее будет создано два спрайта: для обычных экранов и для экранов с двойной плотностью пикселов. В этом случае папка должна содержать изображения вдвое большего разрешения. Спрайт с уменьшенными изображениями будет создан автоматически на основе удвоенных.

Пример правильных имен директорий: `sprite-logos`, `sprite-promotion@2x`.


### Создание svg-спрайтов

Конольная команда `gulp svg`.

Данная команда создает html-файл со спрайтом из svg-файлов. Подробнее о технике https://css-tricks.com/svg-sprites-use-better-icon-fonts/ 

При этом каждый подпроект имеет свой отдельный svg-спрайт. Для того, чтобы данное svg-изображение было объединено в спрайт, имя файла должно начинаться с префикса, указанного в файле конфигурации (по умолчанию `sprite-`). При этом префикс в имени в итоговом файле будет удален и ссылаться на изображения в шаблонах нужно по их исходному имени.

Пример правильного имени файла: `sprite-facebook.svg`. Ссылка на это изображение будет `#facebook`.


## Как начать использовать svg-изображения в проекте.

Svg-изображения являются векторным форматом, поэтому их использование весьма предпочтительно, т.к. это упрощает поддержку для устройств с экранами повышенной плотности. Хорошим сферой применения svg являются иконки.

Существует ряд методов и инструментов, которые позволяют значительно упростить применение svg в проекте.

Подробнее о технике https://css-tricks.com/svg-sprites-use-better-icon-fonts/ 


### 1. Создать template tag в проекте django

[Документация Django по теме](https://docs.djangoproject.com/en/1.8/howto/custom-template-tags/#simple-tags).

В папке `templatetags` создать файл `svg_icon.py`:

```python
from django.template import Library

register = Library()


@register.simple_tag
def svg_icon( id ):
    return '<svg class="ico _%s" role="img"><use xlink:href="#%s"></use></svg>' % (id, id)

```


### 2. Разместить svg-файлы в проекте

Разместить svg-файлы в одной директории с блоком, который их использует. Если блок использует те же самые svg-файлы, которые использует уже существующий в проекте блок, эти файлы должны быть скопированы в директорию нового блока и существовать независимо. 

Такая система позволяет изолировать блоки  друг от друга и в случае, когда блок перестает использоваться, удалить все используемые им файлы вместе с блоком, не опасаясь удалить файлы, которые могут быть использованы другими блоками.

Добавить префикс `sprite-` (может быть изменен в конфигурации gulp) в имени каждого из svg-файлов. Имя файла без префикса и расширения svg является его идентификатором, который используется при вставке изображения в код шаблона.


### 3. Сгенерировать файл спрайта

Выполнить консольную команду `gulp svg`.
 
Для каждого подпроекта генерируется отдельный файл спрайтов. Файл спрайта сохраняется в папке шаблонов, указанной в конфигурации gulp. Файл имеет расширение html и содержит в имени название подпроекта.
 
Пример имени: `svg-sprite-website.html`.


### 4. Подключить svg-спрайт в шаблон

Подключить сгенерированный спрайт в шаблоне

```html
<div style="display:none">{% include "svg-sprite-website.html" %}</div>
```


### 5. Вставить svg-изображение в шаблон

Загрузить templatetag в шаблоне `{% load svg_icon %}`. Затем использовать подключенный templatetag:

```html
<div class="article-views">
{% svg_icon "views" %} 125 просмотров
</div>
```

где `views` это идентификаторор изображения.



### Спасибо

[MCSS](http://operatino.github.io/MCSS/) от [Robert Haritonov](https://twitter.com/operatino)

[ITCSS](http://itcss.io/) от [Harry Roberts](https://twitter.com/csswizardry)

[CSS Codeguide](http://codeguide.co/#css) от [Mark Otto](https://twitter.com/mdo)
