---
title: Добавление страниц Markdown
---

Gatsby может использовать файлы разметки для создания страниц на вашем сайте.
Добавьте плагины для чтения и понимания папок с markdown файлами и автоматического создания страниц из них .

Вот шаги которые Gatsby делает для этого.

1. Чтение файлов в Gatsby из файловой системы
2. преобразование markdown в HTML и frontmatter в данные
3. Создать page component для markdown файлов
4. Программно создавать страницы, используя API Gatsby node.js `createPage`

### Чтение файлов в Gatsby из файловой системы - `gatsby-source-filesystem`

Использовать плагин [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/#gatsby-source-filesystem) для чтения файлов.

#### установка

`npm i --save gatsby-source-filesystem`

Затем откройте `gatsby-config.js` чтобы добавить этот плагин в массив плагинов.

Чтобы добавить плагин, добавьте либо строку (имя плагина), либо параметры передачи, объект.
Для `gatsby-source-filesystem` мы передаем объект, чтобы мы могли установить путь к файловой системе:

```javascript
plugins: [
  {
    resolve: `gatsby-source-filesystem`,
    options: {
      path: `${__dirname}/path/to/markdown/files`,
      name: "markdown-pages",
    },
  },
];
```

Теперь, когда мы "sourced" файлы markdown из файловой системы, мы можем теперь "transform" markdown в HTML и YAML frontmatter в JSON.

### Преобразование markdown — `gatsby-transformer-remark`

Мы будем использовать плагин [`gatsby-transformer-remark`](/packages/gatsby-transformer-remark/) чтобы распознавать файлы, которые markdown и читать их содержимое. Он преобразует frontmatter метаданные вашего markdown файла как `frontmatter` а содержимое как HTML.

`npm i --save gatsby-transformer-remark`

Добавьте это в `gatsby-config.js` после ранее добавленного `gatsby-source-filesystem`.

```javascript
plugins: [
  {
    resolve: `gatsby-source-filesystem`,
    options: {
      path: `${__dirname}/path/to/markdown/files`,
      name: "markdown-pages",
    },
  },
  `gatsby-transformer-remark`,
];
```

#### Примечание по созданию markdown файлов

Когда вы создаете Markdown файл, в верхней части файла, добавьте нижеприведенный блок. У вас могут быть разные пары ключ-значене, которые имеют отношение к вашему веб-сайту. Этот блок будет распознан `gatsby-transformer-remark` как `frontmatter`. GraphQL API предоставит эти данные в наших компонентах React.

```
---
path: "/blog/my-first-post"
date: "2017-11-07"
title: "My first blog post"
---
```

### Создайте шаблон страницы для markdown данных

Создайте папку `templates` в папке `/src` вашего Gatsby приложения.
Теперь создайте в этой папке файл `blogTemplate.js` со следующим содержимым.

```jsx
import React from "react";

export default function Template({
  data, // это prop будут инжектированы запросом GraphQL query ниже.
}) {
  const { markdownRemark } = data; // data.markdownRemark содержит наши данные для поста
  const { frontmatter, html } = markdownRemark;
  return (
    <div className="blog-post-container">
      <div className="blog-post">
        <h1>{frontmatter.title}</h1>
        <h2>{frontmatter.date}</h2>
        <div
          className="blog-post-content"
          dangerouslySetInnerHTML={{ __html: html }}
        />
      </div>
    </div>
  );
}

export const pageQuery = graphql`
  query BlogPostByPath($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`;
```

В приведенном выше файле важны две вещи.

1. Во второй половине файла выполняется запрос GraphQL для получения Markdown данных. Gatsby автоматически предоставил вам все метаданные Markdown и HTML в результате этого запроса.
   **Note: Узнать больше о GraphQL, можно здесь [excellent resource](https://www.howtographql.com/)**
2. Результат запроса инжектируется Гэтсби в компонент `Template` как `data`. `markdownRemark` это свойство, которое мы находим, имеет все детали файла Markdown. Мы можем использовать это для создания шаблона для нашего поста в блоге. Поскольку это React компонент, вы можете стилизовать его с помощью любой из рекомендованных систем стилизации в Gatsby.

### Создание статических страниц с использованием Gatsby Node API

Гэтсби имеет мощную Node.js API, которая позволяет использовать такие функции, как создание динамических страниц. Эта API находится в файле `gatsby-node.js` в корневой папке вашего проекта, на том же уровне что и `gatsby-config.js`. Каждый экспорт, найденный в этом файле, будет выполняться Gatsby, как указано в его [спецификация Node API](/docs/node-apis/). Однако в этом случае нам нужен только один конкретный API, `createPages`.

Gatsby вызывает API `createPages` (если имеется) во время сборки с инжектированными параметрами, `boundActionCreators` и `graphql`. Используйте `graphql` для запроса данных файла Markdown, как показано ниже. Затем используйте создатель действия createPage для создания страницы для каждого файла Markdown с помощью `blogTemplate.js`, который мы создали на предыдущем шаге.

```javascript
const path = require("path");

exports.createPages = ({ boundActionCreators, graphql }) => {
  const { createPage } = boundActionCreators;

  const blogPostTemplate = path.resolve(`src/templates/blogTemplate.js`);

  return graphql(`
    {
      allMarkdownRemark(
        sort: { order: DESC, fields: [frontmatter___date] }
        limit: 1000
      ) {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `).then(result => {
    if (result.errors) {
      return Promise.reject(result.errors);
    }

    result.data.allMarkdownRemark.edges.forEach(({ node }) => {
      createPage({
        path: node.frontmatter.path,
        component: blogPostTemplate,
        context: {}, // additional data can be passed via context
      });
    });
  });
};
```

Это должно помочь вам начать с некоторой базовой мощью markdown на вашем Gatsby сайте. Вы можете дополнительно настроить `frontmatter` и файл шаблона для получения желаемых эффектов!

## Другие учебники

Ознакомьтесь с учебными пособиями, перечисленными на[Awesome Gatsby](/docs/awesome-gatsby/#gatsby-tutorials) страница для более подробной информации о создании сайтов Гэтсби с markdown.

## Gatsby markdown starters

Существует ряд [Gatsby starters](/docs/gatsby-starters/) которые были предварительно сконфигурированы для работы с markdown.
