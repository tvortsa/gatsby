---
title: Создание тегов для записей в блогах
---

Создание страниц тегов для вашего сообщения в блоге - это способ позволить посетителям просматривать связанный контент.

Чтобы добавлять теги к сообщениям в блоге, вы сначала захотите настроить свой сайт, чтобы превратить ваши страницы разметки в сообщения в блоге. Чтобы настроить настройки вашего блога, см. [учебник по слою данных Гэтсби](/tutorial/part-four/) и [Adding Markdown Pages](/docs/adding-markdown-pages/).

Процесс по существу будет выглядеть следующим образом::

1. Добавление тегов в ваши `markdown` файлы
2. Напишисание запроса, для извлечения всех тегов для ваших сообщений
3. Сделать шаблон страницы тегов (для `/tags/{tag}`)
4. Изменение `gatsby-node.js` для рендеринг страниц с использованием этого шаблона
5. Сделать индексную страницу тегов (`/tags`) который отображает список всех тегов
6. _(опционально)_ Рендеринг тегов инлайн в постах блога

## Добавление тегов в ваши `markdown` файлы

You add tags by defining them in the `frontmatter` of your Markdown file. The `frontmatter` is the area at the top surrounded by dashes that includes post data like the title and date.

```md
---
title: "A Trip To the Zoo"
---

I went to the zoo today. It was terrible.
```

Fields can be strings, numbers, or arrays. Since a post can usually have many tags, it makes sense to define it as an array. Here we add our new tags field:

```md
---
title: "A Trip To the Zoo"
tags: ["animals", "Chicago", "zoos"]
---

I went to the zoo today. It was terrible.
```

If `gatsby develop` is running, restart it so Gatsby can pick up the new fields.

## Write a query to get all tags for your posts

Now these fields are available in the data layer. To use field data, query it using `graphql`. All fields are available to query inside `frontmatter`

Try running in Graph<em>i</em>QL (`localhost:8000/___graphql`) the following query

```graphql
{
  allMarkdownRemark(
    sort: { order: DESC, fields: [frontmatter___date] }
    limit: 1000
  ) {
    edges {
      node {
        frontmatter {
          path
          tags
        }
      }
    }
  }
}
```

The resulting data includes the `path` and `tags` frontmatter for each post, which is all the data we'll need to create pages for each tag which contain a list of posts under that tag. Let's make the tag page template now:

## Make a tags page template (for `/tags/{tag}`)

If you followed the tutorial for [Adding Markdown Pages](/docs/adding-markdown-pages/), then this process should sound familiar: we'll make a tag page template, then use it in `createPages` in `gatsby-node.js` to generate individual pages for the tags in our posts.

First, we'll add a tags template at `src/templates/tags.js`:

```jsx
import React from "react";
import PropTypes from "prop-types";

// Components
import Link from "gatsby-link";

const Tags = ({ pathContext, data }) => {
  const { tag } = pathContext;
  const { edges, totalCount } = data.allMarkdownRemark;
  const tagHeader = `${totalCount} post${
    totalCount === 1 ? "" : "s"
  } tagged with "${tag}"`;

  return (
    <div>
      <h1>{tagHeader}</h1>
      <ul>
        {edges.map(({ node }) => {
          const { path, title } = node.frontmatter;
          return (
            <li key={path}>
              <Link to={path}>{title}</Link>
            </li>
          );
        })}
      </ul>
      {/*
              This links to a page that does not yet exist.
              We'll come back to it!
            */}
      <Link to="/tags">All tags</Link>
    </div>
  );
};

Tags.propTypes = {
  pathContext: PropTypes.shape({
    tag: PropTypes.string.isRequired,
  }),
  data: PropTypes.shape({
    allMarkdownRemark: PropTypes.shape({
      totalCount: PropTypes.number.isRequired,
      edges: PropTypes.arrayOf(
        PropTypes.shape({
          node: PropTypes.shape({
            frontmatter: PropTypes.shape({
              path: PropTypes.string.isRequired,
              title: PropTypes.string.isRequired,
            }),
          }),
        }).isRequired
      ),
    }),
  }),
};

export default Tags;

export const pageQuery = graphql`
  query TagPage($tag: String) {
    allMarkdownRemark(
      limit: 2000
      sort: { fields: [frontmatter___date], order: DESC }
      filter: { frontmatter: { tags: { in: [$tag] } } }
    ) {
      totalCount
      edges {
        node {
          frontmatter {
            title
            path
          }
        }
      }
    }
  }
`;
```

**Note**: `propTypes` are included in this example to help you ensure you're getting all the data you need in the component, and to help serve as a guide while destructuring / using those props.

## Modify `gatsby-node.js` to render pages using that template

Now we've got a template. Great! I'll assume you followed the tutorial for [Adding Markdown Pages](/docs/adding-markdown-pages/) and provide a sample `createPages` that generates post pages as well as tag pages. In the site's `gatsby-node.js` file, include `lodash` (`const _ = require('lodash')`) and then make sure your [`createPages`](/docs/node-apis/#createPages) looks something like this:

```js
const path = require("path");

exports.createPages = ({ boundActionCreators, graphql }) => {
  const { createPage } = boundActionCreators;

  const blogPostTemplate = path.resolve("src/templates/blog.js");
  const tagTemplate = path.resolve("src/templates/tags.js");

  return graphql(`
    {
      allMarkdownRemark(
        sort: { order: DESC, fields: [frontmatter___date] }
        limit: 2000
      ) {
        edges {
          node {
            frontmatter {
              path
              tags
            }
          }
        }
      }
    }
  `).then(result => {
    if (result.errors) {
      return Promise.reject(result.errors);
    }

    const posts = result.data.allMarkdownRemark.edges;

    // Create post detail pages
    posts.forEach(({ node }) => {
      createPage({
        path: node.frontmatter.path,
        component: blogPostTemplate,
      });
    });

    // Tag pages:
    let tags = [];
    // Iterate through each post, putting all found tags into `tags`
    _.each(posts, edge => {
      if (_.get(edge, "node.frontmatter.tags")) {
        tags = tags.concat(edge.node.frontmatter.tags);
      }
    });
    // Eliminate duplicate tags
    tags = _.uniq(tags);

    // Make tag pages
    tags.forEach(tag => {
      createPage({
        path: `/tags/${_.kebabCase(tag)}/`,
        component: tagTemplate,
        context: {
          tag,
        },
      });
    });
  });
};
```

Some notes:

* Our graphql query only looks for data we need to generate these pages. Anything else can be queried again later (and, if you notice, we do this above in the tags template for the post title).
* While making the tag pages, note that we pass `tag` through in the `context`. This is the value that gets used in the `TagPage` query to limit our search to only posts tagged with the tag in the URL.

## Make a tags index page (`/tags`) that renders a list of all tags

Our `/tags` page will simply list out all tags, followed by the number of posts with that tag:

```jsx
import React from "react";
import PropTypes from "prop-types";

// Utilities
import kebabCase from "lodash/kebabCase";

// Components
import Helmet from "react-helmet";
import Link from "gatsby-link";

const TagsPage = ({
  data: { allMarkdownRemark: { group }, site: { siteMetadata: { title } } },
}) => (
  <div>
    <Helmet title={title} />
    <div>
      <h1>Tags</h1>
      <ul>
        {group.map(tag => (
          <li key={tag.fieldValue}>
            <Link to={`/tags/${kebabCase(tag.fieldValue)}/`}>
              {tag.fieldValue} ({tag.totalCount})
            </Link>
          </li>
        ))}
      </ul>
    </div>
  </div>
);

TagsPage.propTypes = {
  data: PropTypes.shape({
    allMarkdownRemark: PropTypes.shape({
      group: PropTypes.arrayOf(
        PropTypes.shape({
          fieldValue: PropTypes.string.isRequired,
          totalCount: PropTypes.number.isRequired,
        }).isRequired
      ),
    }),
    site: PropTypes.shape({
      siteMetadata: PropTypes.shape({
        title: PropTypes.string.isRequired,
      }),
    }),
  }),
};

export default TagsPage;

export const pageQuery = graphql`
  query TagsQuery {
    site {
      siteMetadata {
        title
      }
    }
    allMarkdownRemark(
      limit: 2000
      filter: { frontmatter: { published: { ne: false } } }
    ) {
      group(field: frontmatter___tags) {
        fieldValue
        totalCount
      }
    }
  }
`;
```

## _(optional)_ Render tags inline with your blog posts

The home stretch! Anywhere else you'd like to render your tags, simply add them to the `frontmatter` section of your `graphql` query and access them in your component like any other prop.
