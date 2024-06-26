🛠 При написании функции указываются параметры — те переменные, с которыми работает функция. Но возможны случаи, когда не все параметры заданы. Это может быть сделано как специально, например, для использования варианта по умолчанию, так и произойти случайно — ошибка при использовании или неожиданные входные данные.

<iframe title="Параметры функции" src="../demos/params/" height="710"></iframe>

🛠 Давайте функциям имена, чтобы проводить отладку было проще.

Анонимную функцию будет сложнее отлаживать, потому что в стеке вызовов не будет её имени.

```js
someElement.addEventListener('click', function () {
  throw new Error('Error when clicked!')
})
```

В отличие от именованной:

```js
someElement.addEventListener('click', function someElementClickHandler() {
  throw new Error('Error when clicked!')
})
```

🛠 У стрелочных функций можно использовать неявный (implicit) `return`:

```js
const arrowFunc1 = () => {
  return 42
}

const arrowFunc2 = () => 42

arrowFunc1() === arrowFunc2()
// true
// Обе функции возвращают 42
```

Также можно возвращать любые структуры и типы данных:

```js
const arrowFunc3 = () => 'строка'
const arrowFunc4 = () => ['массив', 'из', 'строк']
```

Чтобы вернуть объект, его необходимо обернуть в скобки. Только так JavaScript поймёт, что мы не открываем тело функции, а возвращаем результат:

```js
const arrowFunc5 = () => ({ cat: 'Барс' })

console.log(arrowFunc5())
// { cat: 'Барс' }
```
