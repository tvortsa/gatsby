---
title: Сборка с помощью компонентов
---

Для мспользования Gatsby, вам потребуется базовое понимание React компонентов.

Вот [официальный учебник](https://reactjs.org/tutorial/tutorial.html)
это хорошее место для начала.

## Зачем React компоненты?

React - компонентная архитектура упрощает создание крупных веб-сайтов путем поощрения модульности, повторного использования и четких абстракций. React имеет большую экосистему компонентов с открытым исходным кодом, учебных пособий и инструментов, которые могут быть легко использованы для создания сайтов с помощью Gatsby. Gatsby построена так, чтобы вести себя почти как обычное React приложение.

[Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html)
является хорошим ресурсом для изучения того, как структурировать приложения с помощью React.

## Как использует Гэтсби React Компоненты?

Все в Gatsby строится с использованием компонентов.

Основная структура каталогов проекта может выглядеть так::

```sh
.
├── gatsby-config.js
├── package.json
└── src
    ├── html.jsx
    ├── pages
    │   ├── index.jsx
    │   └── posts
    │       ├── 01-01-2017
    │       │   └── index.md
    │       ├── 01-02-2017
    │       │   └── index.md
    │       └── 01-03-2017
    │           └── index.md
    ├── templates
    │   └── post.jsx
    │
    └── layouts
        └── index.jsx
```

### Компоненты страницы

Компоненты в `src/pages` автоматически становятся страницами с путями на основе имени файла. Например `src/pages/index.jsx` отображается на `yoursite.com`
и `src/pages/about.jsx` становится `yoursite.com/about/`. каждый `.js` или `.jsx`
файл в папке pages должен разрешаться либо строкой либо компонентом react,
в противном случае ваша сборка не удастся.

пример:

`src/pages/about.jsx`

```jsx
import React, { Component } from "react";

class AboutPage extends Component {
  render() {
    return (
      <div className="about-container">
        <p>About me.</p>
      </div>
    );
  }
}

export default AboutPage;
```

### Page template компоненты

Вы можете программно создавать страницы, используя "page template components". Все страницы это React компоненты но очень часто эти компоненты представляют собой только обертки вокруг данных из файлов или других источников.

`src/templates/post.jsx` является примером page component. Он запрашивает GraphQL
для markdown данных и затем отображает страницу, используя эти данные.

Смотрите [part four](/tutorial/part-four/) учебник для подробного ознакомления с программным созданием страниц.

пример:

```jsx
import React from "react";

class BlogPostTemplate extends React.Component {
  render() {
    const post = this.props.data.markdownRemark;

    return (
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
    );
  }
}

export default BlogPostTemplate;

export const pageQuery = graphql`
  query BlogPostBySlug($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`;
```

### Layout компоненты

`src/layouts/index.jsx` (необязательный) оборачивает компоненты страницы. Вы можете использовать его для частей страниц, которые разделяются между страницами, такими как верхние и нижние колонтитулы.

Вы можете использовать `location` prop для визуализации на основе URL-адреса страницы.

пример:

```jsx
import React from "react";
import Navigation from "../components/Navigation/Navigation.jsx";

export default class Template extends React.Component {
  render() {
    if (this.props.location.pathname !== "/") {
      return <Navigation>{this.props.children()}</Navigation>;
    } else {
      return this.props.children();
    }
  }
}
```

### HTML компонент

`src/html.jsx` несет ответственность за все, кроме того Gatsby который живет в `<body />`.

В этом файле вы можете изменить `<head>` metadata, общая структура документа и добавление внешних ссылок.

Как правило, вы должны omit это для вашего сайта так как по файла умолчанию html.js будет достаточно. Если вам требуется больше контроля над рендерингом сервера, тогда важно иметь html.js.

пример:

```jsx
import React from "react";
import favicon from "./favicon.png";

let inlinedStyles = "";
if (process.env.NODE_ENV === "production") {
  try {
    inlinedStyles = require("!raw-loader!../public/styles.css");
  } catch (e) {
    console.log(e);
  }
}

export default class HTML extends React.Component {
  render() {
    let css;
    if (process.env.NODE_ENV === "production") {
      css = (
        <style
          id="gatsby-inlined-css"
          dangerouslySetInnerHTML={{ __html: inlinedStyles }}
        />
      );
    }
    return (
      <html lang="en">
        <head>
          <meta charSet="utf-8" />
          <meta
            name="viewport"
            content="width=device-width, initial-scale=1.0"
          />
          {this.props.headComponents}
          <link rel="shortcut icon" href={favicon} />
          {css}
        </head>
        <body>
          <div
            id="___gatsby"
            dangerouslySetInnerHTML={{ __html: this.props.body }}
          />
          {this.props.postBodyComponents}
        </body>
      </html>
    );
  }
}
```

Это примеры различных способов использования React компонентов в Gatsby
сайтах. Чтобы увидеть полные рабочие примеры, ознакомьтесь с
[examples directory](https://github.com/gatsbyjs/gatsby/tree/master/examples) в реппозитории Gatsby.
