# SCSS guidelines

## БЭМ

### Основы

#### Блоки

Блок — это независимая сущность **со своим собственным смыслом**, он представляет на странице отдельный кирпичик интерфейса.

Например, блоками могут быть:

* заголовок
* кнопка
* навигационное меню

Чтобы задать блок, нужно придумать ему имя и определить его назначение. В интерфейсе может одновременно присутствовать несколько экземпляров одного и того же блока (например, разные кнопки или несколько меню).

#### Элементы

Элемент — это **часть блока**, связанная с ним и по смыслу, и функционально. Элемент не существует и не используется без блока, к которому относится. Но не у всех блоков должны быть элементы.

Примеры элементов:

* навигационное меню (блок), содержащее пункты меню (элементы);
* таблица (блок), внутри которой строки, ячейки и заголовки (элементы).

У элементов тоже есть названия. Если внутри блока несколько одинаковых элементов, то они идут под одним именем (например, ячейки таблицы или пункты списка). Элементы определяются **по смыслу**, а не исходя из HTML-верстки блока; так, отдельный элемент может быть представлен сложной HTML-структурой.

#### Модификаторы

Модификаторы — флаги у блоков или элементов, они определяют вариации или состояния. У сущности может быть несколько модификаторов, если эти модификаторы описывают разные вещи.

* меню с модификатором, чтобы выглядеть различно в шапке и подвале
* поле поиска с кнопкой в виде иконки


### Система именования БЭМ для CSS

Существует несколько вариаций использования именований. Мы используем следующую:

```
.block {}
.block__element {}
.block--modifier {}
.block__element--modifier {}
```

* имена блоков, элементов и модификаторов должны быть недлинными и семантически значимыми;
* используются только маленькие латинские буквы, символ тире и цифры;
* символ подчёркивания используется как специальный разделитель.




## Файловая структура


Все файлы проекта лежат в папке `scss` (придумать новое название и переписать), которая является корневой для своих подпроектов. Каждый проект может иметь произвольное количество подпроектов. Каждый подпроект это набор файлов scss, изображений и скриптов, сгруппированных по слоям и блокам, компилируемых в конечном счете  в свои отдельные наборы css-файлов, которые могут подключаться независимо друг от друга.

Например, подпроект с основными стилями вебсайта, css-файлы которого подключаются на всех страницах сайта и подпроект промо-раздела, css-файлы которого подключаются только на страницах этого промо-раздела.

Для создания подпроекта, в корневой папке проектов создается файл с именем подпроекта и расширением `scss` и директория с таким же именем. Все файлы, относящиеся к данному подпроекту размещаются внутри соответствующей ему директории.

Подпроекты структурируются согласно методологии ITCSS (http://itcss.io/).

Видео и слайды, отлично раскрывающие тему:

* https://speakerdeck.com/dafed/managing-css-projects-with-itcss
* https://www.youtube.com/watch?v=1OKZOV-iLj4

Согласно этой методологии, каждая директория подроекта подразделяется на еще ряд директорий, называемых слоями (layers). Каждая их таких директорий имеет свое назначение и подключается в определенной последовательности. Стили в каждой последующем слое имеют более высокую специфичность, чем в предыдущем и при этом затрагивают все меньшее количество DOM-узлов.

В большинстве подпроектов используются следующие слои:

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

```
@import "website/settings/*";
@import "_tools/*";
@import "website/generic/*";
@import "website/base/*";
@import "website/objects/*";
@import "website/components/*";
@import "website/trumps/*";
```

### Правила именования

Для именования любых сущностей: файлов scss, изображений всех форматов, шаблонов html, BEM-блоков и пр. используется `kebab-case`. Это означает, что в именах используются только латинские буквы в нижнем регистре, для разделения слов используется символ минус (-). 

Символ нижнего подчеркивания (\_) может быть использован **только как первый символ** в имени scss-файла или директории блока. В этом случае scss-файл не компилируется в самостоятельный css и может быть использован только через `@include` из других файлов, либо будет полностью проигнорирован. Папка со знаком подчеркивания вначале имени игнорируется при сборке стилей и применяется, чтобы исключить какие-то блоки из css, не удаляя их исходные файлы.




Каждый блок в отдельном файле.



## Расположение стилей

Стили каждого модуля должны храниться рядом друг с другом, можно их выделять как в закомментированные секции, так и в отдельные файлы:

```
.module { }
.module__list { }
.module-list--modificator { }
```

Селекторы, модифицируемые каскадом, должны так же храниться рядом с родительским классом:

```
.module { }
.module__list { }
.module__list .other-module { }
```

Исключением могут быть только классы из слоя контекста:

```
.module { }
.module__list { }
.touch .module__list { }
.lt-ie9 .module__list { }
```

Модификаторы стоит прописывать после основной группы модифицируемых элементов, чтобы не создавать путаницу, которая обязательно возникнет при появлении нескольких модификаций.

```
.block {}
.block__element {}
.block__sub-element {}

.block--modifier {
	.block_element {}
    .block__sub-element {}
}
```

Такая организация стилей упрощает дальнейший рефакторинг кода и позволяет непосвященному разработчику быстро разобраться во всех стилях модуля и быстро приступить к работе с ним.

При чистке кода просто избавляться от старых модулей и от всех его зависимостей.
 

### Использованные источники

[MCSS](http://operatino.github.io/MCSS/) by [Robert Haritonov](https://twitter.com/operatino)

[ITCSS](http://itcss.io/) by [Harry Roberts](https://twitter.com/csswizardry)

[Масштабирование наоборот: БЭМ-методология Яндекса на небольших проектах](https://ru.bem.info/articles/bem-for-small-projects/) by Максим Ширшин

https://www.youtube.com/watch?v=1OKZOV-iLj4
https://speakerdeck.com/dafed/managing-css-projects-with-itcss
