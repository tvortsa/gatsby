---
title: "Создание и изменение страниц"
---

Gatsby упрощает программный контроль ваших страниц.

Страницы могут быть созданы тремя способами:

* В вашем сайте gatsby-node.js реализуя API
  [`createPages`](/docs/node-apis/#createPages)
* Gatsby ядро автоматически преобразует React компоненты в папке `src/pages` в страницы
* Плагины также могут реализовывать `createPages` и создавать страницы для вас

Вы также можете реализовать API [`onCreatePage`](/docs/node-apis/#onCreatePage)
изменять страницы, созданные в ядре или плагинах, или создавать [client-only routes](/docs/building-apps-with-gatsby/).

## Debugging help

Чтобы узнать, какие страницы создаются вашим кодом или плагинами, вы можете запрашивать информацию о странице при разработке в Graph*i*QL.Вставьте следующий запрос в Graph*i*QL IDE для вашего сайта. Graph*i*QL IDE доступен при запуске
вашего сайта development server по `HOST:PORT/___graphql` e.g.
`localhost:8000/___graphql`.

```graphql
{
  allSitePage {
    edges {
      node {
        path
        component
        pluginCreator {
          name
          pluginFilepath
        }
      }
    }
  }
}
```

Вы также можете запросить `context` данные, добавленные вами или плагинами на страницы.

## Создание страниц в gatsby-node.js

Часто вам нужно программно создавать страницы. Например, у вас есть
markdown файлы, каждый из которых должна быть страницей.

В этом примере предполагается, что каждая markdown страница имеет "path" заданный во frontmatter
markdown файла.

```javascript
// Реализация Gatsby API “createPages”. Это вызывается один раз когда
// data layer загружается, чтобы плагины могли создать страницы из файлов.
exports.createPages = ({ boundActionCreators, graphql }) => {
  const { createPage } = boundActionCreators;

  return new Promise((resolve, reject) => {
    const blogPostTemplate = path.resolve(`src/templates/blog-post.js`);
    // Запрос для markdown узлов используемых в создании страниц.
    resolve(
      graphql(
        `
          {
            allMarkdownRemark(limit: 1000) {
              edges {
                node {
                  frontmatter {
                    path
                  }
                }
              }
            }
          }
        `
      ).then(result => {
        if (result.errors) {
          reject(result.errors);
        }

        // Создавайте страницы для каждого markdown файла.
        result.data.allMarkdownRemark.edges.forEach(({ node }) => {
          const path = node.frontmatter.path;
          createPage({
            path,
            component: blogPostTemplate,
            // Если у вас есть компонент layout component в src/layouts/blog-layout.js
            layout: `blog-layout`,
            // В шаблоне вашего блога graphql query,вы можете использовать путь
            // как GraphQL переменная для запроса данных из markdown файла.
            context: {
              path,
            },
          });
        });
      })
    );
  });
};
```

## Изменение страниц, созданных ядром или плагинами

Gatsby ядро и плагины могут автоматически создавать страницы для вас. Иногда по умолчанию не совсем то, что вы хотите, и вам нужно изменить созданные объекты страниц
objects.

### Удаление конечных слэшей

Общей причиной необходимости изменения автоматически создаваемых страниц является удаление конечных слэшей.

Чтобы сделать это, на вашем сайте `gatsby-node.js` добавить код, похожий на следующий:

_Note: Также есть плагин, который автоматически удалит все трейлинг-косые черты со страниц.:
[gatsby-plugin-remove-trailing-slashes](/packages/gatsby-plugin-remove-trailing-slashes/)_.

```javascript
// Реализуйте Gatsby API “onCreatePage”.
// оно вызывается всякий раз после создания страницы.
exports.onCreatePage = ({ page, boundActionCreators }) => {
  const { createPage, deletePage } = boundActionCreators;
  return new Promise(resolve => {
    const oldPage = Object.assign({}, page);
    // Удаляем trailing slash если страница /
    page.path = _path => (_path === `/` ? _path : _path.replace(/\/$/, ``));
    if (page.path !== oldPage.path) {
      // Заменить старую страницу на новую страницу
            deletePage(oldPage);
      createPage(page);
    }
    resolve();
  });
};
```

### Выбор макета страницы

По умолчанию все страницы будут использовать макет, найденный в `/layouts/index.js`.

Вы можете выбрать пользовательский макет для определенных страниц (например с удаленным
хедером и футером для landing pages). Вы можете выбрать компонент макета при создании страниц с помощью `createPage` action добавив ключ макета к объеут страницы
или изменять страницы, созданные в любом месте, используя `onCreatePage` API. Все компоненты в папке `/layouts/` автоматически доступны.

```javascript
// Реализуем Gatsby API “onCreatePage”.
// Который вызывается после создания каждой страницы.
exports.onCreatePage = async ({ page, boundActionCreators }) => {
  const { createPage } = boundActionCreators;

  return new Promise((resolve, reject) => {
    if (page.path.match(/^\/landing-page/)) {
      // Предполагается, что `landingPage.js` существует в папке `/layouts/`
      page.layout = "landingPage";

      // Обновляем страницу.
      createPage(page);
    }

    resolve();
  });
};
```
