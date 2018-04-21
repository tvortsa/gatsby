---
title: Добавление списка записей блога Markdown
---

После того, как вы добавили страницы Markdown на свой сайт, вы всего на один шаг от возможности перечислить свои сообщения на отдельной странице индекса.

### Создание сообщений

Как описано [здесь](/docs/adding-markdown-pages), вам придется создавать свои сообщения в файлах Markdown, которые будут выглядеть так::

```md
---
path: "/blog/my-first-post"
date: "2017-11-07"
title: "My first blog post"
---

Кто-нибудь слышал о GatsbyJS все же?
```

### Создание страницы

Первым шагом будет создание страницы, на которой будут отображаться ваши сообщения., в `src/pages/`. Вы можете, например, использовать `index.js`.

```jsx
import React from "react";
import PostLink from "../components/post-link";

const IndexPage = ({ data: { allMarkdownRemark: { edges } } }) => {
  const Posts = edges
    .filter(edge => !!edge.node.frontmatter.date) // Вы можете отфильтровать свои сообщения по некоторым критериям
    .map(edge => <PostLink key={edge.node.id} post={edge.node} />);

  return <div>{Posts}</div>;
};

export default IndexPage;
```

### Создание запроса GraphQL

Во-вторых, вам необходимо предоставить данные вашему компоненту с помощью запроса GraphQL. Давайте добавим его, так что бы `index.js` выглядел так:

```jsx
import React from "react";
import PostLink from "../components/post-link";

const IndexPage = ({ data: { allMarkdownRemark: { edges } } }) => {
  const Posts = edges
    .filter(edge => !!edge.node.frontmatter.date) // Вы можете отфильтровать свои сообщения по некоторым критериям
    .map(edge => <PostLink key={edge.node.id} post={edge.node} />);

  return <div>{Posts}</div>;
};

export default IndexPage;

export const pageQuery = graphql`
  query IndexQuery {
    allMarkdownRemark(sort: { order: DESC, fields: [frontmatter___date] }) {
      edges {
        node {
          id
          excerpt(pruneLength: 250)
          frontmatter {
            date(formatString: "MMMM DD, YYYY")
            path
            title
          }
        }
      }
    }
  }
`;
```

### Creating the `PostLink` component

Осталось только добавить компонент `PostLink`. Создайте новый файл `post-link.js` в `src/components/` и добавьте следующее:

```jsx
import React from "react";
import Link from "gatsby-link";

const PostLink = ({ post }) => (
  <div>
    <Link to={post.frontmatter.path}>
      {post.frontmatter.title} ({post.frontmatter.date})
    </Link>
  </div>
);

export default PostLink;
```

Это должно дать вам страницу с вашими сообщениями, отсортированными по дате. Вы можете дополнительно настроить `frontmatter` и страницу и `PostLink` компонентов для получения желаемых эффектов!
