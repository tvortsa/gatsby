---
title: Transformer plugins
typora-copy-images-to: ./
---

## Что в этом учебнике?

В предыдущем учебнике показано, как source плагины доставляют данные _внутрь_ системы данных Гэтсби. В этом уроке вы узнаете, как transformer плагины _преобразуют_ raw содержимое, предоставленное source плагинами. Сочетание source плагинов и transformer плагинов может обрабатывать все источники данных и преобразование данных, которые вам могут понадобиться при создании сайта Gatsby.

## Transformer плагины

Часто формат данных, которые мы получаем из исходных плагинов, не является тем, что вы хотите использовать для создания своего сайта. Плагин источника файловой системы позволяет запрашивать данные _о типе_ файлов, но что, если вы хотите запросить данные _содержащиеся_ в файлах?

Чтобы сделать это возможным, Gatsby поддерживает transformer плагины, которые берут исходное содержимое из source плагинов и _transform_ это во что-то более полезное.

Например, Markdown файлы. Markdown приятно писать, но когда вы строите страницу с ней, вам понадобится чтобы Markdown стали HTML.

Давайте добавим файл Markdown на наш сайт по адресу
`src/pages/sweet-pandas-eating-sweets.md` (Это станет нашей первой записью блога Markdown) и узнаем, как _преобразовать_ его в HTML используя transformer плагины и
GraphQL.

```markdown
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Панды действительно сладкие.

Вот видео с пандами, которые едят.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

После сохранения файла посмотрите `/my-files/` снова - новый Markdown файл находится в таблице. Это очень мощная функция Gatsby. Как и раньше в
`siteMetadata` примере, source плагины могут перегружать данные.
`gatsby-source-filesystem` всегда сканирует новые файлы, которые нужно добавить, и когда они есть, повторно запускает ваши запросы.

Давайте добавим transformer плагин, который может трансформировать Markdown файлы:

```shell
npm install --save gatsby-transformer-remark
```

Затем добавьте его в `gatsby-config.js` как обычно:

```javascript{13}
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
    `gatsby-transformer-remark`,
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

Перезапустите сервер разработки, затем обновите (или снова откройте) Graph_i_QL и посмотрите на автозаполнение:

![markdown-autocomplete](markdown-autocomplete.png)

Еще раз выберите `allMarkdownRemark` и запустите его, как мы сделали для` allFile`. Вы увидите там файл Markdown, который мы недавно добавили. Изучите поля, доступные на `MarkdownRemark` node.

![markdown-query](markdown-query.png)

Ok! Надеюсь, некоторые основы начинают становится на место. Source плагины приносят данные _внутрь_ системы данных Gatsby и _transformer_ плагины преобразовывают исходный контент, предоставленный source плагинами. Этот шаблон может обрабатывать все источники данных и преобразование данных, которые могут потребоваться при создании сайта Gatsby.

## Создайте список файлов Markdown нашего сайта в `src/pages/index.js`

Давайте теперь создадим список наших файлов Markdown на первой странице. Как и многие блоги, мы хотим, чтобы список ссылок на первой странице указывал на каждую запись в блоге. С GraphQL мы можем _сделать запрос_ текущего списка постов Markdown поэтому нам не нужно будет поддерживать список вручную.

Как и со страницей `src/pages/my-files.js` , замените `src/pages/index.js` на
для добавления запроса с некоторым начальным HTML и стилем.

```jsx
import React from "react";
import g from "glamorous";

import { rhythm } from "../utils/typography";

export default ({ data }) => {
  console.log(data);
  return (
    <div>
      <g.H1 display={"inline-block"} borderBottom={"1px solid"}>
        Удивительные кормящиеся панды
      </g.H1>
      <h4>{data.allMarkdownRemark.totalCount} Сообщений</h4>
      {data.allMarkdownRemark.edges.map(({ node }) => (
        <div key={node.id}>
          <g.H3 marginBottom={rhythm(1 / 4)}>
            {node.frontmatter.title}{" "}
            <g.Span color="#BBB">— {node.frontmatter.date}</g.Span>
          </g.H3>
          <p>{node.excerpt}</p>
        </div>
      ))}
    </div>
  );
};

export const query = graphql`
  query IndexQuery {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          excerpt
        }
      }
    }
  }
`;
```

Теперь передняя часть должна выглядеть так::

![frontpage](frontpage.png)

Но наше одно сообщение в блоге выглядит немного одиноким. Итак, давайте добавим еще один
`src/pages/pandas-and-bananas.md`

```markdown
---
title: Панды и Бананы
date: "2017-08-21"
---

У Панды есть бананы? Проверьте это короткое видео, которое показывает, что да! Кажется, панды действительно наслаждаются бананами!

<iframe width="560" height="315" src="https://www.youtube.com/embed/4SZl1r2O_bY" frameborder="0" allowfullscreen></iframe>
```

![two-posts](two-posts.png)

Это выглядит великолепно! Кроме того что порядок сообщений неправильный.

Но это легко исправить. При запросе соединения какого-либо типа вы можете передать множество аргументов в запрос. Ты можешь `sort` и `filter` узлы, установить количество узлов в `skip`, и выберите `limit` количества узлов для извлечения. С помощью этого мощного набора операторов мы можем выбрать любые данные, которые мы хотим - в нужном нам формате.

В запросе нашей индексной страницы измените
`allMarkdownRemark` на
`allMarkdownRemark(sort: {fields: [frontmatter___date], order: DESC})`. Сохраните это, и порядок сортировки должен быть исправлен.

Попробуйте открыть Graph_i_QL и игра с разными параметрами сортировки. Вы можете сортировать
`allFile` соединение вместе с другими соединениями.

## Вызов

Попробуйте создать новую страницу, содержащую сообщение в блоге, и посмотреть, что происходит со списком сообщений в блоге на главной странице!

## Что будет дальше?

Отлично! Мы только что создали хорошую страницу индекса, где мы запрашиваем наши файлы Markdown и создаем список названий блога и выдержки. Но мы не хотим просто видеть выдержки, нам нужны фактические страницы для наших файлов Markdown.

Мы могли бы продолжать создавать страницы, размещая React компонентов в `src/pages`. Однако мы узнаем, как _програмно_ создавать страницы из _данных_. Gatsby _не ограничивает_
 создавать страницы из таких файлов, как многие статические генераторы сайтов. Gatsby позволяет использовать GraphQL для запроса _данных_ и _map_ данных в _pages_—все во время сборки. Это действительно мощная идея. Мы изучим его последствия и способы его использования в следующем учебном пособии, где вы узнаете, как [программно создавать страницы из данных](/tutorial/part-seven/).
