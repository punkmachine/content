Метод `Function.prototype.bind()` выполняет связывание (binding) функции с указанными параметрами: значением `this` и набором аргументов.

Пример выполнения:

```js
function targetFunc(x) {
  const id = this.id ?? 'неизвестно'
  console.log('id:', id, 'данные:', x)
}
const thisObj = {id: 42}
const boundFunc = targetFunc.bind(thisObj, 7)
```

Для чего это может потребоваться?

Во-первых, чтобы указать и окончательно закрепить значение `this` ещё до вызова самой функции. Например, это упрощает открепление метода от его родительского объекта.

Во-вторых, чтобы задать последовательность аргументов, которые при вызове функции будут предшествовать всем другим аргументам. Указанные аргументы будут связаны с функцией без необходимости передавать их при вызове. Такой подход называют [частичным применением функции](/tools/fp/#chastichnoe-primenenie).

Как это работает?

При вызове `.bind()` создаётся новая функция-обёртка (bound function). Эта функция, хранит требуемый `this` и приоритетные аргументы и защищена от возможности изменить эти параметры при вызове. В дальнейшем вызов функции-обёртки приводит к вызову целевой (target) функции.

Посмотрим как это работает на примере:

```js
// Целевая функция
function show(a, b) {
  const name = this.name ?? 'неизвестно'
  console.log('имя:', name, 'данные:', a, b)
}

// Объект для указания this
const character1 = {
  name: 'Pумпельштильцхен'
}

// Прямой вызов целевой функции
show()
// имя: неизвестно, данные: undefined undefined

// Создаём функцию-обёртку, привязываем аргументы
// `character1` как this и `true` как первый аргумент
const boundShow = show.bind(character1, true)

// Целевая функция использует привязанные параметры
boundShow()
// имя: Pумпельштильцхен, данные: true undefined

boundShow(false)
// имя: Pумпельштильцхен, данные: true false

boundShow.call({name: 'Риннебист'}, 2)
// имя: Pумпельштильцхен, данные: true 2
```

Единственное исключение, когда `this` не будет равен аргументу при связывании внутри целевой функции, — вызов функции-обёртки как конструктора:

```js
function Pixel(color) {
  this.color = color
  console.log('color:', this.color, 'hidden:', this.hidden)
}

// Создаём функцию-обёртку
const MyPixel = Pixel.bind({hidden: true}, 'white')

// Прямой вызов целевой функции
Pixel('red')
// color: red hidden: undefined

// Вызов функции-обёртки
MyPixel('black')
// color: white hidden: true

// Вызов функции-обёртки как конструктора
new MyPixel('blue')
// color: white hidden: undefined
```
