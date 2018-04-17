---
title: Programmatically create pages from data
typora-copy-images-to: ./
---

## Что в этом учебнике?

В предыдущем учебном пособии мы создали приятную страницу индекса, которая запрашивает файлы Markdown и создает список названий блога и выдержки.Но мы не хотим просто видеть выдержки, нам нужны фактические страницы для каждого файла Markdown.

Мы могли бы продолжать создавать страницы, размещая React компонентов в `src/pages`. Однако теперь мы узнаем, как _программно_ создавать страницы из _данных_. Gatsby _не ограничивает_
создавать страницы из таких файлов, как многие статические генераторы сайтов. Gatsby позволяет использовать GraphQL для запроса _данных_ и _map_ данных в _pages_—все во время сборки. Это действительно мощная идея. Мы будем изучать его последствия и способы использовать его для остальной части учебника.

Давайте начнем.

## Создание slugs для страниц

Создание новых страниц имеет два шага:

1. Создание "path" или "slug" для страницы.
2. Создание страницы.

Чтобы создать наши Markdown страницы, мы научимся использовать два Gatsby APIs
[`onCreateNode`](/docs/node-apis/#onCreateNode) и
[`createPages`](/docs/node-apis/#createPages). Это две рабочие лошади APIs
вы увидите, что используется на многих сайтах и ​​плагинах.

Мы делаем все возможное, чтобы упростить внедрение API Gatsby. Чтобы реализовать API, вы экспортируете функцию с именем API из `gatsby-node.js`.

Итак, давайте сделаем это. В корне вашего сайта создайте файл с именем
`gatsby-node.js`. Затем добавьте к нему следующее. Эта функция будет вызываться Gatsby всякий раз, когда создается новый узел (или обновляется).

```javascript
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type);
};
```

Остановите и перезапустите сервер разработки. Как и вы, вы увидите, что несколько недавно созданных узлов вошли в консоль терминала.

Давайте используем этот API для добавления slugs для наших страниц Markdown d `MarkdownRemark`
nodes.

Измените нашу функцию, чтобы она теперь искала `MarkdownRemark` узлы.

```javascript{2-4}
exports.onCreateNode = ({ node }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
};
```

Мы хотим использовать каждое имя файла Markdown для создания slug страницы. Так
`pandas-and-bananas.md"` станет `/pandas-and-bananas/`. Но как мы получаем имя файла из `MarkdownRemark` узла? Чтобы получить это, нам нужно _traverse_
 "node graph" к его _parent_ `File` node, как `File` nodes содержит данные, которые нам нужны о файлах на диске. Для этого снова измените нашу функцию:

```javascript{1,3-4}
exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    const fileNode = getNode(node.parent)
    console.log(`\n`, fileNode.relativePath)
  }
};
```

Там, в вашем терминале, вы должны увидеть относительные пути для наших двух файлов Markdowns.

![markdown-relative-path](markdown-relative-path.png)

Теперь давайте создадим slugs. Как логика для создания slugs из имен файлов может оказаться сложным, плагин `gatsby-source-filesystem`  поставляется с функцией для создания
slugs. Давайте использовать это.

```javascript{1,5}
const { createFilePath } = require(`gatsby-source-filesystem`);

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` }))
  }
};
```

Функция обрабатывает поиск родителя `File` node наряду с созданием slug. Запустите сервер разработки еще раз, и вы должны увидеть лог-вывод в терминале
двух slugs, по одному на каждый Markdown файл.

Теперь добавим новые slugs прямо в `MarkdownRemark` nodes. Это мощно, так как любые данные, которые мы добавляем к узлам, доступны для запроса позже. GraphQL.
Таким образом, будет легко получить slug когда придет время создавать страницы.

Для этого мы будем использовать функцию, переданную в нашу реализацию API, называемую
[`createNodeField`](/docs/bound-action-creators/#createNodeField). Эта функция позволяет нам создавать дополнительные поля на узлах, созданных другими плагинами. Только оригинальный создатель узла может напрямую изменять узел - все остальные плагины
(включая наш `gatsby-node.js`) должны использовать эту функцию для создания дополнительных полей.

```javascript{3,4,6-11}
const { createFilePath } = require(`gatsby-source-filesystem`);

exports.onCreateNode = ({ node, getNode, boundActionCreators }) => {
  const { createNodeField } = boundActionCreators
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
};
```

Перезапустите сервер разработки и откройте или обновите Graph_i_QL. Затем запустите этот запрос, чтобы увидеть наш новые slugs.

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        fields {
          slug
        }
      }
    }
  }
}
```

Теперь, когда slugs создаются, мы можем создавать страницы.

## Создание страниц

В том же файле `gatsby-node.js` добавьте следующее. Здесь мы рассказываем Гэтсби о наших страницах - каковы их пути, какой компонент шаблона они используют и т. Д.

```javascript{15-34}
const { createFilePath } = require(`gatsby-source-filesystem`);

exports.onCreateNode = ({ node, getNode, boundActionCreators }) => {
  const { createNodeField } = boundActionCreators
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
};

exports.createPages = ({ graphql, boundActionCreators }) => {
  return new Promise((resolve, reject) => {
    graphql(`
      {
        allMarkdownRemark {
          edges {
            node {
              fields {
                slug
              }
            }
          }
        }
      }
    `).then(result => {
      console.log(JSON.stringify(result, null, 4))
      resolve()
    })
  })
};
```

Мы добавили реализацию
[`createPages`](/docs/node-apis/#createPages) API которую Gatsby вызывает для добавления страниц. Мы используем переданный в `graphql` функцию для запроса Markdown
slugs только что созданный. Затем мы выходим из результата запроса, который должен выглядеть так::

![query-markdown-slugs](query-markdown-slugs.png)

Нам нужна еще одна вещь для создания страниц: это page template компонент. Как все в Gatsby, программные страницы это React компоненты. При создании страницы, мы должны указать, какой компонент использовать.

Создайте каталог в `src/templates` а затем добавьте следующее в файл с именем
`src/templates/blog-post.js`.

```jsx
import React from "react";

export default () => {
  return <div>Hello blog post</div>;
};
```

Затем обновите `gatsby-node.js`

```javascript{1,17,32-41}
const path = require(`path`);
const { createFilePath } = require(`gatsby-source-filesystem`);

exports.onCreateNode = ({ node, getNode, boundActionCreators }) => {
  const { createNodeField } = boundActionCreators
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
};

exports.createPages = ({ graphql, boundActionCreators }) => {
  const { createPage } = boundActionCreators
  return new Promise((resolve, reject) => {
    graphql(`
      {
        allMarkdownRemark {
          edges {
            node {
              fields {
                slug
              }
            }
          }
        }
      }
    `).then(result => {
      result.data.allMarkdownRemark.edges.forEach(({ node }) => {
        createPage({
          path: node.fields.slug,
          component: path.resolve(`./src/templates/blog-post.js`),
          context: {
            // Data passed to context is available in page queries as GraphQL variables.
            slug: node.fields.slug,
          },
        })
      })
      resolve()
    })
  })
};
```

Перезагрузите сервер разработки, и наши страницы будут созданы! Легкий способ найти новые страницы, которые вы создаете при разработке, - это перейти к случайному пути, где Гэтсби поможет вам показать список страниц на сайте. Если вы перейдете на
<http://localhost:8000/sdf> вы увидите новые страницы, которые мы создали.

![new-pages](new-pages.png)

Посетите один из них, и мы увидим:

![hello-world-blog-post](hello-world-blog-post.png)

Это немного скучно. Давайте возьмем данные из нашего сообщения Markdown. + Изменить
`src/templates/blog-post.js` to:

```jsx
import React from "react";

export default ({ data }) => {
  const post = data.markdownRemark;
  return (
    <div>
      <h1>{post.frontmatter.title}</h1>
      <div dangerouslySetInnerHTML={{ __html: post.html }} />
    </div>
  );
};

export const query = graphql`
  query BlogPostQuery($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`;
```

И…

![blog-post](blog-post.png)

Ура!

Последний шаг - связать наши новые страницы с индексной страницы.

Вернуться к `src/pages/index.js` и давайте запросим наш Markdown slugs и создадим ссылки.

```jsx{3,18-19,29,47-49}
import React from "react";
import g from "glamorous";
import Link from "gatsby-link";

import { rhythm } from "../utils/typography";

export default ({ data }) => {
  return (
    <div>
      <g.H1 display={"inline-block"} borderBottom={"1px solid"}>
        Amazing Pandas Eating Things
      </g.H1>
      <h4>
        {data.allMarkdownRemark.totalCount} Posts
      </h4>
      {data.allMarkdownRemark.edges.map(({ node }) =>
        <div key={node.id}>
          <Link
            to={node.fields.slug}
            css={{ textDecoration: `none`, color: `inherit` }}
          >
            <g.H3 marginBottom={rhythm(1 / 4)}>
              {node.frontmatter.title}{" "}
              <g.Span color="#BBB">— {node.frontmatter.date}</g.Span>
            </g.H3>
            <p>
              {node.excerpt}
            </p>
          </Link>
        </div>
      )}
    </div>
  )
}

export const query = graphql`
  query IndexQuery {
    allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC }) {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          fields {
            slug
          }
          excerpt
        }
      }
    }
  }
`
```

И вот мы идем! Рабочий, хотя и небольшой, блог!

Попробуйте больше играть с сайтом. Попробуйте добавить еще несколько файлов Markdown. Изучите запрос других данных с узлов MarkdownRemark и добавьте их на страницы главной страницы или блога.

В этой части учебника мы изучили основы построения с помощью слоя данных Gatsby. Вы узнали, как использовать _source_ и _transform_ данные с помощью плагинов. Как использовать GraphQL для _map_ данных на страницах. Затем, как создать _page template components_, где вы запрашиваете данные для каждой страницы.

## Вызов

Следующим шагом является попытка выполнить описанные выше инструкции с помощью нового набора страниц.

## Что будет дальше?

Теперь, когда вы создали сайт Gatsby, куда вы идете дальше?

* Share your Gatsby site on Twitter and see what other people have created by searching for #gatsbytutorial! Make sure to mention @gatsbyjs in your Tweet, and include the hashtag #gatsbytutorial :)
* You could take a look at some [example sites](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
* Explore more [plugins](/docs/plugins/)
* See what [other people are building with Gatsby](https://github.com/gatsbyjs/gatsby/#showcase)
* Check out the documentation on [Gatsby's APIs](/docs/api-specification/), [nodes](/docs/node-interface/) or [GraphQL](/docs/graphql-reference/)
