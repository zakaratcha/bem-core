## Общие сведения

### БЭМ-методология и JavaScript

В БЭМ-методологии веб-интерфейс строится из независимых **блоков** у которых могут быть **элементы**. И блоки, и элементы могут иметь состояния или особенности, описываемые **модификаторами**.

Работа веб-интерфейса обеспечивается несколькими **технологиями** (HTML, CSS, JS и т.д.). Его реализация разбита на компоненты по блокам. Блок содержит набор **файлов технологий**, составляющих аспекты его реализации:

* `my-block.css` — внешний вид блока;
* `my-block.bemhtml` — шаблоны для генерации HTML-представления блока;
* `my-block.js` — **динамическое поведение** блока в браузере.

Фреймворк `i-bem.js` позволяет разложить клиентский JavaScript на компоненты в терминах БЭМ:

* **Блок** — JS-компонент, описывающий логику работы однотипных элементов интерфейса. Например, все кнопки могут быть реализованы в виде блока `button`. В этом случае, `button.css` определяет внешний вид всех кнопок, а `button.js` — логику их работы. На каждой странице может размещаться более одного **экземпляра блока** (например, кнопки). Каждому экземпляру блока соответствует JS-объект, в памяти браузера, хранящий его состояние. JS-объект содержит ссылку на DOM-узел, к которому привязан данный экземпляр блока.
* **Элементы** — DOM-узлы, вложенные в DOM-узел блока, с атрибутом `class`, указывающим на их роль в БЭМ-предметной области (имя блока и элемента). Элементы блока доступны через [JS-API](./i-bem-js-states.ru.md#Управление-модификаторами) экземпляра блока.
* **Модификаторы** — предоставляют информацию о состоянии блока и его элементов. Состояние модификаторов записывается в атрибуте `class` на DOM-узлах блока и элементов. Управление модификаторами производится через [JS-API](./i-bem-js-states.ru.md#Управление-модификаторами) экземпляра блока.

### Сборка

Разработка в рамках БЭМ-методологии ведется модульно — каждый блок программируется отдельно. Финальный исходный код веб-страниц формируется из кода отдельных блоков с помощью процедур **сборки**.

В файловой системе блок удобно представлять в виде каталога, а реализацию блока в каждой из технологий — в виде отдельного файла:

```html
    desktop.blocks/
        my-block/
            my-block.css
            my-block.js
            my-block.bemhtml
            ...

    desktop.blocks/
        other-block/
            other-block.css
            other-block.js
            other-block.bemhtml
            ...
```

Для каждой веб-страницы код использованных на ней блоков может быть собран в единые файлы:

```html
    desktop.bundles/
        index/
            index.html
            index.css
            index.js
            ...
```

Существует два инструмента, поддерживающих БЭМ-предметную область, для сборки кода результирующих веб-страниц из отдельных описаний блоков:

* [bem-tools](https://ru.bem.info/toolbox/bem-tools/)
* [ENB](https://ru.bem.info/toolbox/enb/)

Оба инструмента позволяют автоматизировать создание HTML-разметки для [привязки JS-блоков](./i-bem-js-html-binding.ru.md#Привязка-js-блоков-к-html) и [передачи параметров экземпляру блока](./i-bem-js-params.ru.md#Передача-параметров-экземпляру-блока-и-элемента).

### Почему i-bem.js так называется

В соответствии с БЭМ-методологией, базовая JS-библиотека БЭМ-платформы изначально разрабатывалась как особый служебный блок. Такой подход позволяет работать с базовыми библиотеками так же, как и с обычными блоками. В частности, структурировать код в терминах элементов и модификаторов и гибко настраивать поведение библиотеки на разных уровнях переопределения.

Служебным блокам в БЭМ было принято давать имена с префиксом `i-`. Таким образом, имя `i-bem.js` читается как *реализация блока `i-bem` в технологии `JS`*.

### Как использовать i-bem.js

Фреймворк `i-bem.js` входит в состав библиотеки [bem-core](https://ru.bem.info/platform/libs/bem-core/).

Реализация `i-bem.js` состоит из двух модулей:

* **Модуль [i-bem](https://ru.bem.info/platform/libs/bem-core/)**.

  Предоставляет базовую реализацию JS-блока `i-bem`, от которой наследуются все блоки и элементы в `i-bem.js`. Блок `i-bem` написан с расчетом на использование в любом JS-окружении: как на клиенте, так и на сервере (например, в Node.js).

* **Модуль [i-bem-dom](https://ru.bem.info/platform/libs/bem-core/)**.

  Предоставляет базовую реализацию блока и элемента, привязанных к DOM-узлу. Рассчитан на использование на клиенте, опирается на работу браузеров с DOM. Зависит от `jQuery`.

Зависимости:

* jQuery (только для модуля `i-bem-dom`). При использовании `bem-core` отдельная установка jQuery не требуется.
* Модульная система [ym/modules](https://github.com/ymaps/modules). При использовании [ENB](https://ru.bem.info/toolbox/enb/) с технологией `.browser.js` (и производных от нее) эта зависимость удовлетворяется автоматически.

Можно использовать `i-bem.js` как часть полного стека БЭМ-инструментов. В этом случае свой проект удобно создавать на основе шаблонного репозитория [project-stub](https://ru.bem.info/platform/project-stub/), в котором настроена автоматическая установка зависимых библиотек и сборка.

Если не планируется использование других технологий БЭМ-платформы, достаточно поместить код библиотеки `bem-core` в существующий проект.
