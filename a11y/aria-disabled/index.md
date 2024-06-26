---
title: "`aria-disabled`"
description: "Атрибут для неизменяемых интерактивных элементов, с которыми всё равно можно взаимодействовать."
authors:
  - teplostanski
keywords:
  - доступность
  - ARIA
  - ARIA-атрибут
  - readonly
related:
  - a11y/role-button
  - a11y/role-tab
  - html/disabled
tags:
  - doka
---

## Кратко

`aria-disabled` — это [ARIA-атрибут](/a11y/aria-attrs/#atributy-vidzhetov), который указывает, что элемент интерфейса в данный момент недоступен для взаимодействия. Он позволяет разработчикам явно указать, что элемент неактивен, без изменения его DOM-структуры или удаления привязанных событий.

Так же работает HTML-атрибут [`disabled`](/html/disabled/).

## Пример

Пример с кнопкой, которую хотим временно сделать неактивной, не удаляя при этом обработчики событий:

```html
<button aria-disabled="true">Отправить</button>
```

Кнопка будет отображаться, но пользователь не сможет по ней кликнуть или сфокусироваться.

## Как пишется

Добавьте к тегу атрибут `aria-disabled` с одним из значений:

- `true` — элемент неактивен;
- `false` (по умолчанию) — элемент активен, с ним можно взаимодействовать.

`aria-disabled` можно задавать только некоторым тегам и [ролям](/a11y/aria-roles/):

- [`<button>`](/html/button/), [`<summary>`](/html/details/), [`<input>` с типами](/html/input/#type) `button`, `image`, `reset`, `submit` или для роли [`button`](/a11y/role-button/);
- [`<a>`](/html/a/) или [`link`](/a11y/role-link/);
- [`<details>`](/html/details/), [`<fieldset>`](/html/fieldset/), [`<optgroup>`](/html/optgroup/) или [`group`](/a11y/role-group/);
- [`<hr>`](/html/hr/) или [`separator`](/a11y/role-separator/);
- [`<div>`](/html/div/), [`<span>`](/html/span/) или [`generic`](/a11y/role-generic/);
- [`tab`](/a11y/role-tab/);
- [`scrollbar`](/a11y/role-scrollbar/);
- [`application`](/a11y/role-application/);
- [`gridcell`](/a11y/role-gridcell/);
- [`menuitem`](/a11y/role-menuitem/).

Для HTML-тегов лучше использовать атрибут `disabled` вместо `aria-disabled` там, где он поддерживается.

### Подводные камни и особенности

- **неактивные родительские элементы**: если родительский элемент получает `aria-disabled="true"`, его дочерние элементы также считаются неактивными;
- **CSS и JavaScript**: чтобы полностью реализовать поведение неактивности, нужно использовать CSS для стилизации и JavaScript для управления состоянием, так как `aria-disabled` сам по себе не останавливает взаимодействие с элементом.

### Особенности для ссылок

Для элементов [`<a>`](/html/a/), которые не содержат атрибут `href`, `aria-disabled="true"` может быть использован для указания, что ссылка в текущем состоянии нефункциональна. Такой трюк используют для хлебных крошек. Так пользователи клавиатуры знают, где находятся сейчас, при этом не могут перейти по ссылке на текущую страницу. Например:

```html
<a href="#dogs">Товары для собак</a>

<a
  role="link"
  aria-disabled="true"
  aria-current="page"
>
  Товары для котов
</a>
```

В случае неактивной ссылки лучше не использовать `href`, и чтобы ссылка не потеряла свою роль, явно её задать с помощью атрибута `role="link"`.

## Демонстрация работы

В этом примере кнопка становится неактивной при нажатии, меняя свой визуальный стиль и убирая возможность взаимодействия. Это может быть полезно в формах, когда действительно важно не дать пользователю сразу кликнуть по кнопке отправки формы, если не все поля заполнены.

<iframe title="Пример с кнопкой" src="demos/" height="300"></iframe>

### Задействуем CSS и JavaScript

Использование `aria-disabled` отличается от атрибута `disabled`. ARIA-атрибут не останавливает все виды пользовательского взаимодействия с элементом на уровне браузера. Поэтому важно использовать CSS и JavaScript для управления состояниями элементов. Вот простой пример, как можно стилизовать и управлять элементом с `aria-disabled`:

```css
[aria-disabled="true"] {
  opacity: 0.7;
  cursor: not-allowed;
}

[aria-disabled="true"]:focus {
  outline: none;
}
```

```javascript
document.querySelectorAll('[aria-disabled="true"]').forEach((element) => {
  element.addEventListener('click', (e) => {
    // Предотвращаем клики
    e.preventDefault()
  })
})
```

### Интеграция с фреймворками

В фреймворках, таких как React или Angular, вы можете интегрировать `aria-disabled` напрямую в ваш компонентный подход. Например, в React компоненте это может выглядеть так:

```jsx
function SaveButton({ isSaving }) {
  return (
    <button aria-disabled={isSaving ? 'true' : 'false'}>
      {isSaving ? 'Saving...' : 'Save Changes'}
    </button>
  )
}
```

## Как понять

На элементе с `aria-disabled` пользователи не могут сделать фокус, узнать о его роли и состоянии, а также скопировать из него данные. Такое поведение может быть у временно неактивных элементов. К примеру, когда в форме заполнены не все поля или какое-то действие в процессе выполнения.

С помощью `aria-disabled` можете создать гибкие и доступные интерфейсы. Помните, что для полной функциональности нужны CSS и JavaScript. Это не только делает сайт более доступным, но и улучшает пользовательский опыт в целом. Не забудьте также протестировать сайт со [скринридерами](/a11y/screenreaders/). ARIA-разметка должна облегчать взаимодействие с интерфейсом всех пользователей, а не усложнять.
