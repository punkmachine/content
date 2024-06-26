---
title: "`<datalist>`"
description: "Автодополнение полей ввода без JavaScript."
authors:
  - blueingreen68
contributors:
  - skorobaeus
  - tatianafokina
keywords:
  - список значений
  - комбинированный список
related:
  - a11y/role-listbox
  - html/select
  - js/deal-with-forms
tags:
  - article
---

## Кратко

Элемент `<datalist>` — это _источник предложений_, то есть такой элемент, который содержит предопределённые варианты выбора для пользователя. В основном за варианты выбора опций выступают элементы [`<option>`](/html/option/).

У тега по умолчанию есть [роль `listbox`](/a11y/role-listbox/).

## Пример

```html
<form>
  <label for="my-browser">Выберите браузер из списка:</label>
  <input type="text" list="browsers" id="my-browser" name="my-browser">
  <datalist id="browsers">
    <option value="Chrome">
    <option value="Firefox">
    <option value="Yandex Browser">
    <option value="Opera">
    <option value="Safari">
    <option value="Microsoft Edge">
  </datalist>
<form>
```

<iframe title="Пример работы элемента <datalist>" src="demos/default/" height="320"></iframe>

## Как пишется

Перед использованием элемент `<datalist>` нужно связать с элементом [`<input>`](/html/input/) через атрибут `list`:

```html
<input list="fruits">
<datalist id="fruits">
  <option value="Банан">
  <option value="Арбуз">
  <option value="Киви">
</datalist>
```

Элемент `<datalist>` не сработает, если между элементами `<input>` и `<datalist>` будет любой другой элемент с `id`. Даже если это сам `<input>` со значением, которое равно значению атрибута `list` элемента `<input>`:

```html
<form>
  <label for="fruits">Выберите фрукт из неработающего списка:</label>
  <input list="fruitsList" id="fruits" name="fruits">
  <div id="fruitsList">
    <datalist id="fruitsList">
      <option value="Банан">
      <option value="Арбуз">
      <option value="Киви">
    </datalist>
  </div>
</form>
```

<iframe title="Неработающий элемент <datalist>" src="demos/datalist-not-work/" height="320"></iframe>

## Как понять

Элемент `<datalist>` не отображается браузерами, его значение [`display`](/css/display/) по умолчанию `none`. `<datalist>` просто содержит набор каких-то предопределённых вами вариантов выбора — опций. Их можно будет выбрать из выпадающего списка после того, как свяжите их с `<input>`. Эти значения будут по-разному отображаться в зависимости от значения атрибута `type` у `<input>`.

Элемент `<datalist>` можно связать с элементом `<input>` только через атрибут `list`, но этот атрибут поддерживается не всеми интерактивными элементами.

Элемент `<input>` не будут работать с элементом `<datalist>`, если у `<input>` одно из следующих значений атрибута `type`:

- `hidden`;
- `password`;
- `checkbox`;
- `radio`;
- `file`;
- `submit`;
- `image`;
- `reset`;
- `button`.

## Интерактивные элементы, поддерживающие `<datalist>`

### `text`, `search`

Элемент `<input>` со значением `text` или `search` у атрибута `type` — это однострочный интерактивный элемент для редактирования обычного текста.

Значения `text`, `search` атрибута `type` элемента `<input>` имеют функцию автодополнения.

По мере набора текста в поле для ввода браузер выдаёт пользователю варианты, которые, в случае совпадения, он находит в элементе `<datalist>`.

При поиске _предложения_ в элементе `<datalist>` браузер ориентируется не только на значение элемента `<option>`, но и на атрибут `label`, если он указан. Также атрибут `label` будет отображаться в выпадающем списке, если он не совпадает со значением элемента `<option>`.

Рассмотрим пример:

```html
<form>
  <label for="my-browser">Текстовое поле:</label>
  <input type="text" list="browsers" id="my-browser" name="my-browser">
  <datalist id="browsers">
    <option value="Chrome" label="Google">
    <option value="Firefox" label="Mozilla">
    <option value="Opera" label="Opera">
  </datalist>
</form>
```

Здесь у каждого элемента `<option>` есть атрибут `label`. У последнего элемента значения атрибутов `value` и `label` совпадают, поэтому в выпадающем списке метка не будет отображаться.

<iframe title="Пример работы <datalist> с интерактивными элементами text, search" src="demos/datalist-text-search/" height="320"></iframe>

<aside>

⚠️ Работа атрибута `label` отличается от браузера к браузеру. Например, в Firefox значение атрибута `label` — опция в выпадающем списке, а значение в атрибуте `value` подставляется в поле ввода только тогда, когда пользователь выберет опцию из списка. В Safari метки не отображаются вовсе.

</aside>

### `tel`

Элемент `<input>` со значением `tel` атрибута `type` — это интерактивный элемент для редактирования телефонного номера. _Предложения_ элемента `<datalist>` отображаются с этим типом также, как и с типом `text`.

### `url`

Элемент `<input>` со значением `url` атрибута `type` — это интерактивный элемент для редактирования одного абсолютного URL. _Предложения_ элемента `<datalist>` отображаются с этим типом также, как и с типом `text`.

### `email`

Элемент `<input>` со значением `email` атрибута `type` — это интерактивный элемент для редактирования адреса электронной почты. _Предложения_ элемента `<datalist>` отображаются с этим типом также, как и с типом `text`.

Если у элемента `<input>` указан атрибут [`multiple`](/html/multiple/), появляется одна особенность. Поле для ввода позволяет указать несколько электронных почт через запятую. После знака запятой автодополнение будет всё так же работать.

<iframe title="Пример работы <datalist> с интерактивным элементом email" src="demos/datalist-email/" height="320"></iframe>

### `date`, `month`, `week`, `time`

Элемент `<input>` со значением `date`, `month`, `week` или `time` атрибута `type` — это интерактивный элемент для выбора определённой даты, месяца, недели или времени.

Чтобы _предложения_ элемента `<datalist>` правильно отображались в интерактивных элементах, необходимо соблюдать определённые правила написания значения элемента `<option>`. Основные правила для `date`, `month`, `week`:

1. Все значения разделяются через дефис.
1. _Год_ должен быть больше 0 и состоять из 4 цифр.
1. _Месяц_ должен состоять из двух цифр и быть не меньше 1, но и не больше 12.
1. _Число_ должно состоять из двух цифр и быть не меньше 1, но и не больше 31.
1. _Неделя_ должна состоять из двух цифр и быть не меньше 1, но и не больше 53 либо 52, в зависимости от года (високосный или невисокосный). Номер недели указывается после символа `W`.

Порядок написания для `date`, `month`, `week` следующий:
- `date` — год-месяц-число;
- `month` — год-месяц;
- `week` — год-неделя;

Для типа `time` действуют следующие правила:

1. Все значения разделяются двоеточием.
1. _Час_ должен состоять из двух цифр и быть не меньше 0, но и не больше 23.
1. _Минута_ должна состоять из двух цифр и быть не меньше 0, но и не больше 59.
1. _Секунда_ должна состоять из двух цифр и быть не меньше 0, но и не больше 59.

Секунда также может быть не целой и иметь дробную часть. Для отделения целой части от дробной используется знак точки. Дробная часть должна состоять максимум из 3 цифр.

Порядок написания для `time` следующий:

- с использованием целой секунды — час:минута:секунда;
- если у секунды есть дробная часть — час:минута:секунда.дробь.

Итоговый код с примерами всех описанных интерактивных элементов для работы со временем:

```html
<!-- Date -->
<form>
  <label for="dateInput">Укажите дату:</label>
  <input type="date" list="dateList" id="dateInput">
  <datalist id="dateList">
    <option value="2022-10-10" label="Сегодня">
    <option value="2022-10-03" label="Неделю назад">
    <option value="2022-10-17" label="Неделю спустя">
  </datalist>
</form>

<!-- Month -->
<form>
  <label for="monthInput">Укажите месяц:</label>
  <input type="month" list="monthList" id="monthInput">
  <datalist id="monthList"></datalist>
    <option value="2022-10" label="Осень">
    <option value="2022-12" label="Зима">
    <option value="2022-03" label="Весна">
  </datalist>
</form>

<!-- Week -->
<form>
  <label for="weekInput">Укажите неделю:</label>
  <input type="week" list="weekList" id="weekInput">
  <datalist id="weekList">
    <option value="2022-W12" label="Двенадцатая неделя">
    <option value="2022-W52" label="Последняя неделя">
    <option value="2022-W1" label="Первая неделя">
  </datalist>
</form>

<!-- Time -->
<form>
  <label for="timeInput">Укажите время:</label>
  <input type="time" list="timeList" id="timeInput" step="any">
  <datalist id="timeList">
    <option value="08:00:20" label="Утром">
    <option value="12:00" label="Днём">
    <option value="18:00" label="Вечером">
  </datalist>
</form>
```

<iframe title="Пример работы <datalist> с интерактивными элементами date, month, week, time" src="demos/datalist-date-month-week-time/" height="620"></iframe>

<aside>

⚠️ В некоторых браузерах время ограничено с точностью до минуты, поэтому в `<datalist>` элементы `<option>` с секундами в значении не будут отображаться в выпадающем списке `<input>` с атрибутом `type="time"`. Если в выпадающем списке нужно время с точностью до секунды, задайте `<input>` атрибут `step="any"`.

</aside>

### `datetime-local`

Элемент `<input>` со значением `datetime-local` атрибута `type` — это интерактивный элемент для установки местной даты и времени, без информации о смещении часового пояса.

Порядок написания для `datetime-local` разделён на две части:

- первая часть — год-месяц-число;
- вторая часть — час:минута:секунда.

Части разделяются пробелом или символом `T`.

```html
<form>
  <label for="datetimeInput">Укажите дату:</label>
  <input type="datetime-local" list="dateList" id="datetimeInput">
  <datalist id="dateList">
    <option value="2022-10-10 12:00" label="Сегодня днём">
    <option value="2022-10-03T08:00" label="Неделю назад утром">
    <option value="2022-10-17 17:00" label="Через неделю вечером">
  </datalist>
</form>
```

<iframe title="Пример работы <datalist> с интерактивным элементом datatime-local" src="demos/datalist-datetime-local/" height="320"></iframe>

### `number`

Элемент `<input>` со значением `number` атрибута `type` — это интерактивный элемент для установки числа. _Предложения_ элемента `<datalist>` отображаются с этим типом также, как и с типом `text`.

### `range`

Элемент `<input>` со значением `range` атрибута `type` — это интерактивный элемент для установки числа, как и `number`, но с более простым отображением в виде ползунка.

Чтобы _предложения_ элемента `<datalist>` правильно отображались в интерактивном элементе `range`, значения опций, элементов `<option>`, должны быть числом с плавающей точкой.

Если значения указаны верно, то браузер при рендеринге будет отображать засечки, которые обозначают элемент `<option>`.

```html
<form>
  <label for="rangeInput">Укажите число:</label>
  <input type="range" list="rangeList" id="rangeInput">
  <datalist id="rangeList">
    <option value="0">
    <option value="25">
    <option value="50">
    <option value="75">
    <option value="100">
  </datalist>
</form>
```

<iframe title="Пример работы <datalist> с интерактивным элементом range" src="demos/datalist-range/" height="320"></iframe>

За отображение числового значения под ползунком в демке отвечает элемент [`<output>`](/html/output/).

### `color`

Элемент `<input>` со значением `color` атрибута `type` — это интерактивный элемент для выбора цвета.

Чтобы _предложения_ элемента `<datalist>` правильно отображались в интерактивном элементе `color`, значения опций, элементов `<option>`, должны быть в виде шестнадцатеричного значения цвета.

Правильное значение должно состоять из 7 символов `#000000`, включая решётку `#`. Сокращённые значения `#000` работать не будут.

```html
<form>
  <label for="colorInput">Укажите цвет:</label>
  <input type="color" list="colorList" id="colorInput">
  <datalist id="colorList">
    <option value="#FF6347" label="Tomato">
    <option value="#F5DEB3" label="Wheat">
    <option value="#DDA0DD" label="Plum">
  </datalist>
</form>
```

<iframe title="Пример работы <datalist> с интерактивным элементом color" src="demos/datalist-color/" height="320"></iframe>

<aside>

⚠️ На данный момент Firefox поддерживает `<datalist>` частично. Нормально работают только интерактивные элементы с типами `text`, `search`, `url`, `tel`, `email` и `number`. Помимо этого, функция автодополнения браузера работает немного по-другому, а сам выпадающий список отличается от большинства браузеров. Так что, проверяйте работу `<datalist>` в Firefox.

</aside>

## Различия между `<select>` и `<datalist>`

Кроме функции автодополнения элемента `<datalist>`, основное отличие между [`<select>`](/html/select/) и этим тегом в том, что в элементе `<select>` нельзя выбрать или указать значение не из списка предлагаемых.

В интерактивных элементах с вводом значений можете указать своё значение или выбрать что-то из предложенного в списке.

С другой стороны, если добавить атрибут `multiple` к `<select>`, появится возможность выбрать несколько вариантов из выпадающего списка. Множественный выбор у интерактивных элементов, связанных с элементом `<datalist>`, работает только у типа `email`.

### Обратная совместимость

Для обратной совместимости с устаревшими браузерами элементы `<datalist>` и `<select>` используются вместе.

Есть два способа:

Первый способ. Если предопределённые элементы выбора не важны в устаревших браузерах, достаточно использовать элемент `<datalist>` с дочерними элементами `<option>`.

Выглядит это так:

```html
<p>
  <label>
    Введите породу вашей собаки:
    <input type="text" name="breed" list="breeds">
  </label>
  <datalist id="breeds">
    <option value="Австралийская овчарка">
    <option value="Акита-ину">
    <option value="Алабай">
    <!-- Гав-гав! -->
  </datalist>
</p>
```

Устаревшие браузеры не поддерживают элемент `<datalist>` и атрибут `value` у элемента `<option>`. Чтобы функция автодополнения подсказывала предопределённые варианты выбора в современных браузерах, значения элементов `<option>` указываются в атрибутах `value`. Если сделать значения содержимым `<option>`, тогда они будут отображаться в устаревших браузерах.

В итоге в устаревших браузерах будет отображаться только элемент `<input>`, в который пользователь введёт нужное значение. В современных браузерах будет элемент `<input>` с выпадающим списком и автодополнением.

Второй способ. Если предопределённые значения нужно показать в устаревших браузерах, то вложите элементы `<option>` в элемент `<select>` и сделайте значения элементов содержимым `<option>`, а не указывайте их в атрибуте `value`. Если указать значения в `value`, они не будут отображаться в выпадающем списке `<select>`. Наконец, вложите `<select>` с дочерними элементами `<option>` в `<datalist>`.

Выглядит это так:

```html
<p>
  <label>
    Введите породу вашей собаки:
    <input type="text" name="breed" list="breeds">
  </label>
  <datalist id="breeds">
    <label>
    или выберите одну из списка:
      <select name="breed">
        <option>Австралийская овчарка
        <option>Акита-ину
        <option>Алабай
        <!-- Гав-гав! -->
      </select>
    </label>
  </datalist>
</p>
```

Современный браузер не отобразит элементы `<select>` и [`<label>`](/html/label/), которые вложены в `<datalist>`. Однако браузер не будет игнорировать элементы с `<option>` в `<select>`, они станут элементами `<datalist>`.

В итоге в современных браузерах отобразиться элемент `<input>` с выпадающим списком и автодополнением. Устаревший браузер будет игнорировать элемент `<datalist>` и предложит ввести своё значение при помощи элемента `<input>` или выбрать значение из выпадающего списка `<select>`.

## Доступность

Хотя у `<datalist>` есть встроенная семантика, тег нельзя назвать полностью доступным. Например, не все скринридеры зачитывают содержимое выпадающего списка с вариантами для автозаполнения.

Следующая проблема связана с CSS — пока у разработчиков нет возможности стилизовать выпадающий список и пункты в нём, а стили по умолчанию далеки от совершенства. Также к списку не применяются системные цвета в режиме высокой контрастности. В нём он выглядит точно так же, как в обычно режиме отображения цветов.

![Выпадающий список в режиме высокой контрастности в Windows 11. Курсор наведён на пункт «Chrome» в выпадающем списке. Фон элемента стал светло-фиолетовым, текст при этом белый и почти сливается с фоновым.](images/datalist-hcm.png)

Наконец, пункты выпадающего списка не увеличиваются при масштабировании.

![Интерфейс увеличен на 210%. Поля и подписи к ним стали больше и перестроились друг под друга. Выпадающий список остался небольшим на фоне других элементов.](images/datalist-zoom.png)
