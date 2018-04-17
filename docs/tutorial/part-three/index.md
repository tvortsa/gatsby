---
title: Создание вложенных макетов в Gatsby
typora-copy-images-to: ./
---
# Добро пожаловать в третью часть

## В этой части учебника

В этой части вы узнаете, как Gatsby позволяет создавать «компоненты компоновки». Компоненты компоновки - это разделы вашего сайта, которые вы хотите разделить на нескольких страницах. Например, сайты Gatsby обычно имеют компонент компоновки с общим заголовком и нижним колонтитулом. Другими распространенными вещами для добавления в макеты являются боковая панель и меню навигации.

На этой странице боковая панель слева (предполагая, что вы находитесь на более крупном устройстве), а заголовок сверху - часть компонента макета gatsbyjs.org.

Давайте погрузимся и исследуем Gatsby layouts.

## Установите стартер

Как мы уже упоминали в Части второй, на данный момент, вероятно, неплохо закрыть оконные окна и файлы проекта из предыдущих разделов учебника, чтобы сохранить чистоту на рабочем столе. Затем откройте новое окно терминала и запустите следующие команды, чтобы создать новый сайт Gatsby в каталоге под названием `tutorial-part-three` а затем перейдите в этот новый каталог:

```shell
gatsby new tutorial-part-three https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-three
```

После того, как сайт будет завершен, установите `gatsby-plugin-typography`. Для напоминания о том, как это сделать, см. Вторую часть учебников. Для
темы Typography.js, давайте попробуем "Fairy Gates" typography тему на этот раз:

```shell
npm install --save gatsby-plugin-typography typography-theme-fairy-gates
```

Создайте папку `src/utils` , и затем создайте typography config файл в `src/utils/typography.js`:

```javascript
import Typography from "typography";
import fairyGateTheme from "typography-theme-fairy-gates";

const typography = new Typography(fairyGateTheme);

export default typography;
```

Затем создайте наш сайт. `gatsby-config.js` в корне сайта, и добавьте к нему следующий код:

```javascript
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography.js`,
      },
    },
  ],
};
```

Теперь давайте добавим несколько разных страниц: главную страницу - front page, about page, и contact page.

`src/pages/index.js`

```jsx
import React from "react";

export default () => (
  <div>
    <h1>Здравствуй! Я создаю поддельный сайт Гэтсби как часть учебника!</h1>
    <p>
      Что мне нравится делать? Много конечно, но определенно нравится создавать веб-сайты.
    </p>
  </div>
);
```

`src/pages/about.js`

```jsx
import React from "react";

export default () => (
  <div>
    <h1>Обо мне</h1>
    <p>Я достаточно хорош, я достаточно умен, и черт возьми, такие люди, как я!</p>
  </div>
);
```

`src/pages/contact.js`

```jsx
import React from "react";

export default () => (
  <div>
    <h1>Я бы хотел поговорить! Напишите мне по указанному ниже адресу</h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
);
```

![no-layout](no-layout.png)

Теперь у нас начинается хороший персональный сайт!

Но есть несколько проблем. Во-первых, было бы неплохо, если бы содержимое страницы было сосредоточено на экране, как во второй части учебника. И, во-вторых, мы действительно должны иметь какую-то глобальную навигацию, поэтому посетителям легко найти и посетить каждую из подстраниц.

Давайте рассмотрим эти проблемы, создав наш первый компонент компоновки.

## Наш первый layout компонент

Сначала создайте новый каталог в `src/layouts`. Все компоненты компоновки должны находиться в этом каталоге.

Давайте создадим очень простой компонент компоновки в `src/layouts/index.js`:

```jsx
import React from "react";

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children()}
  </div>
);
```

_Обратите внимание, что в отличие от большинства `children` props,  `children` prop переданный компонентам компоновки, является функцией и должен быть выполнен_

Остановите `gatsby develop` и запустите его снова, чтобы новый макет вступил в силу.

![with-layout2](with-layout2.png)

Сладкий, макет работает! Теперь наш текст центрирован и привязан к столбцу шириной 650 пикселей, как мы указали.

Давайте теперь добавим в том же файле название нашего сайта:

```jsx{5}
import React from "react";

export default ({ children }) =>
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MySweetSite</h3>
    {children()}
  </div>
```

Если мы перейдем на любую из трех наших страниц, мы увидим, что добавлен один и тот же заголовок, например,
`/about/` page:

![with-title](with-title.png)

Давайте добавим navigation links на каждую из трех страниц:

```jsx{2-9,12-22}
import React from "react";
import Link from "gatsby-link";

const ListLink = props =>
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>
      {props.children}
    </Link>
  </li>

export default ({ children }) =>
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `1.25rem 1rem` }}>
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MySweetSite</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Home</ListLink>
        <ListLink to="/about/">About</ListLink>
        <ListLink to="/contact/">Contact</ListLink>
      </ul>
    </header>
    {children()}
  </div>
```

![with-navigation](with-navigation.png)

И вот он у нас есть! Трехстраничный сайт с базовой глобальной навигацией.

_Challenge:_ С новым "layout component" powers, попробуем добавить headers, footers,
global navigation, sidebars, etc. к вашему сайту Гэтсби!

## Что будет дальше

Продолжайте
[часть четвертая учебника, где мы начнем изучать уровень данных Gatsby и программные страницы!](/tutorial/part-four/)
