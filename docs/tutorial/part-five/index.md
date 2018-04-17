---
title: Source plugins
typora-copy-images-to: ./
---

## В этой части учебника

В этом уроке вы узнаете, как вытащить данные на ваш сайт Gatsby с помощью GraphQL и source плагинов. Однако, прежде чем вы узнаете об этих плагинах, вы захотите узнать, как использовать что-то называемое Graph_i_QL, инструмент, который поможет вам правильно структурировать ваши запросы.

## Введение Graph_i_QL

Graph_i_QL это GraphQL интегрированная среда развития (IDE). Это мощный (и удивительный) инструмент который вы будете часто использовать при построении Gatsby сайтов.

Вы можете получить к нему доступ, когда сервер разработки сайта работает - обычно на
<http://localhost:8000/___graphql>.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

Здесь мы ткнули вокруг встроенного `Site` "type" и посмотреть, какие поля доступны на нем, включая `siteMetadata` объект, который мы запросили ранее. Попробуйте открыть
Graph_i_QL и играть с вашими данными! Нажмите <kbd>Ctrl + Space</kbd> вызвать окно автозаполнения и <kbd>Ctrl + Enter</kbd> чтобы выполнить запрос. Мы будем использовать Graph_i_QL намного больше в оставшейся части учебника.

## Source плагины

Данные на сайтах Gatsby могут поступать из любого места: API, базы данных, CMS, локальные файлы и т. Д.

Source плагины извлекают данные из своего источника. Так filesystem source plugin
знает, как извлекать данные из файловой системы. WordPress plugin знает, как извлекать данные из WordPress API.

Давайте добавим [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) и изучим, как это работает.

Сначала установите плагин в корне проекта:

```sh
npm install --save gatsby-source-filesystem
```

Затем добавьте его в свой `gatsby-config.js`:

```javascript{6-12}
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-plugin-glamor`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
};
```

Сохраните это и перезапустите сервер разработки gatsby. Затем откройте Graph_i_QL
еще раз.

Если вы откроете окно автозаполнения, вы увидите:

![graphiql-filesystem](graphiql-filesystem.png)

Hit <kbd>Enter</kbd> on `allFile` then type <kbd>Ctrl + Enter</kbd> to run a
query.

![filesystem-query](filesystem-query.png)

Удалите `id` из запроса и снова откройте автозаполнение (<kbd>Ctrl +
Space</kbd>).

![filesystem-autocomplete](filesystem-autocomplete.png)

Попробуйте добавить в запрос несколько полей, нажмите <kbd>Ctrl + Enter</kbd>
каждый раз для повторного запуска запроса. Вы увидите что-то вроде этого:

![allfile-query](allfile-query.png)

Результатом является массив File "nodes" (node является причудливым именем для объекта в
"graph"). каждый File объект имеет поля, которые мы запрашивали для.

## Создайте страницу с помощью запроса GraphQL

Создание новых страниц с помощью Gatsby часто начинается с Graph_i_QL. Сначала вы набросаете запрос данных, играя в Graph_i_QL, затем скопируйте это в React page компонента, чтобы начать строить UI.

Попробуем это.

Создайте новый файл в `src/pages/my-files.js` с `allFile` запросом, который мы только что создали:

```jsx{4}
import React from "react"

export default ({ data }) => {
  console.log(data)
  return <div>Hello world</div>
}

export const query = graphql`
  query MyFilesQuery {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

Строка `console.log(data)` выделено выше. Часто бывает полезно создать новый компонент для консолидации данных, которые вы получаете из запроса, чтобы вы могли исследовать данные в консоли своего браузера, создавая UI.

Если вы посетите новую страницу в `/my-files/` и откройте свою консоль браузера, вы увидите:

![data-in-console](data-in-console.png)

Форма данных соответствует форме запроса.

Давайте добавим код к нашему компоненту, чтобы распечатать File data.

```jsx{5-37}
import React from "react"

export default ({ data }) => {
  console.log(data)
  return (
    <div>
      <h1>My Site's Files</h1>
      <table>
        <thead>
          <tr>
            <th>relativePath</th>
            <th>prettySize</th>
            <th>extension</th>
            <th>birthTime</th>
          </tr>
        </thead>
        <tbody>
          {data.allFile.edges.map(({ node }, index) =>
            <tr key={index}>
              <td>
                {node.relativePath}
              </td>
              <td>
                {node.prettySize}
              </td>
              <td>
                {node.extension}
              </td>
              <td>
                {node.birthTime}
              </td>
            </tr>
          )}
        </tbody>
      </table>
    </div>
  )
}

export const query = graphql`
  query MyFilesQuery {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

И... 😲

![my-files-page](my-files-page.png)

## Что будет дальше?

Теперь вы узнали, как source плагины приносят данные _внутрь_ Систему данных Гэтсби. В следующем уроке вы узнаете, как transformer plugins _трансформируют_ raw содержание, предоставленный source плагинами. Сочетание source плагинов и transformer плагинов может обрабатывать все источники данных и преобразование данных, которые вам могут понадобиться при создании сайта Гэтсби. Нажмите здесь для [чтобы узнать о transformer плагинах](/tutorial/part-six/).
