#### Что такое стратегия?

Стратегия _(Strategy)_ — это ООП поведенческий шаблон проектирования, который позволяет расширять базовый класс или метод новым функционалом. Для этого нужно передать в него, так называемый, **конкретный класс**. Например:

```js
const Clock = new Clock // Создаем базовую версию часов
const GoldenClock = new Clock(new GoldenParts) // Часы с золотым оформлением
const WoodenClock = new Clock(new WoodenParts) // Часы с деревянным оформлением

Clock.draw() // Рисуем стандартную версию часов
GoldenClock.draw() // Метод тот же, что и строкой выше, но теперь мы используем золотое оформление
WoodenClock.draw() // Используем деревянное оформление
```

#### Выполнение задачи

Чтобы написать такой класс нам понадобится специальный [**well-known** символ](https://262.ecma-international.org/6.0/#sec-well-known-symbols) `[Symbol.split]`. Метод `split()` вызывает функцию `[Symbol.split](str)`, а результат вызова возвращает как результат `split()`. У строк это уже реализовано:

```js
"123,4,56".split(",") // ["123", "4", "56]
```

Мы можем добавить такое же поведение к своему классу:

```js
class MySplit {
  constructor(value) {
    this.value = value; // Принимаем строку по которой будем сплитить
  }
  [Symbol.split](string) { // принимаем строку-аргумент (которую будем сплитить)
    // В конце необходимо вернуть полученный результат, иначе split() вернет undefined
  }
}
```

Для нашей задачи разобьём строку при помощи регулярного выражения:

```js
[Symbol.split](string) {
  // Заменяем все вхождения this.value на /${this.value}/
  let index = string.replace(new RegExp(this.value, "g"), `/${this.value}/`);

  // убираем первый слэш, /url/Path ->  url/Path
  if (index[0] === "/") index = index.substr(1)
  // Строка должна начинаться с url/, даже если его не было в начале
  if (!index.startsWith(this.value)) index = `${this.value}/` + index;

  return index;
}
```

#### Пример работы

```js
"foobarfoobaz".split(new MySplit("foo")) // "foo/bar/foo/baz"
"foobarfoobaz".split(new MySplit("bar")) // "bar/foo/bar/foobaz"
```

#### Что хотят проверить?

Этот вопрос проверяет умеете ли вы использовать символы. Хотя такие вопросы задают редко, из-за специфичности темы, умение пользоваться символами – полезный инструмент, который вам обязательно пригодится.
