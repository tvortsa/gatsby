---
title: Querying for data in a blog
typora-copy-images-to: ./
---

Welcome to Part Four of the tutorial! Halfway through! Hope things are starting
to feel pretty comfortable 😀

## Recap of first half of the tutorial

So far, we've been learning how to use React.js—how powerful it is to be able to
create our _own_ components to act as custom building blocks for websites.

We’ve also explored styling components using CSS Modules.

## What's in this tutorial?

In the next four parts of the tutorial (including this one), we'll be diving into the Gatsby data layer, which is a powerful feature of Gatsby that lets you easily build sites from Markdown, WordPress, headless CMSs, and other data sources of all flavors.

**NOTE:** Gatsby’s data layer is powered by GraphQL. For an in-depth tutorial on
GraphQL, we recommend [How to GraphQL](https://www.howtographql.com/).

## Data in Gatsby

Веб-сайт состоит из четырех частей, HTML, CSS, JS, и данные. Первая половина учебника была посвящена первым трем. Давайте теперь узнаем, как использовать данные на сайтах Гэтсби.

Что такое данные?

Ответ на компьютерную науку - это: данные - это такие вещи, как `"strings"`,
integers (`42`), objects (`{ pizza: true }`), etc.

В целях работы в Gatsby, Однако, более полезный ответ: "все, что живет вне компонента React".

До сих пор мы писали текст и добавляли изображения _непосредственно_ в компонентах.
Это _отличный_ подход для многих сайтов. Но часто вы хотите хранить данные _вне_ компонентов, а затем вносить данные _внутрь_ компонентов по мере необходимости.

Например, если вы создаете сайт с WordPress (поэтому другие участники имеют приятный интерфейс для добавления и сохранения контента) и Gatsby, то _данные_
для сайта (страницы и сообщения) находятся в WordPress и ты _pull_ эти данные, по мере необходимости, в ваши компоненты.

Данные также могут использоваться в типах файлов, таких как Markdown, CSV, etc. а также в базах данных и API всех видов.

**Слой данных Gatsby позволяет нам извлекать данные из этих (и любого другого источника)
прямо в наши компоненты**—в нужной нам форме.

## Как слой данных Gatsby использует GraphQL для извлечения данных в компоненты

Существует множество вариантов загрузки данных в компоненты React. Одним из самых популярных и мощных из них является технология, называемая
[GraphQL](http://graphql.org/).

GraphQL был изобретен в Facebook чтобы помочь инженерам-разработчикам _pull_ Необходимые данные для компонентов.

GraphQL это **q**uery **l**anguage ( _QL_ часть его названия). Если вы знакомы с SQL, он работает очень схожим образом. Используя специальный синтаксис, вы описываете данные, которые вы хотите в своем компоненте, и затем данные вам даются.

Gatsby использует GraphQL чтобы компоненты могли объявлять нужные им данные.

## Наш первый запрос GraphQL

Давайте создадим еще один новый сайт для этой части учебника, как в предыдущих частях. Мы собираемся построить Markdown блог называемый "Pandas Eating Lots".
Он посвящен демонстрации лучших фотографий и видеороликов Pandas, которые много едят. По пути мы будем погружать наши пальцы в GraphQL и поддержку Markdown от Gatsby.

Откройте новое окно терминала и запустите следующие команды, чтобы создать новый сайт Gatsby в каталоге под названием `tutorial-part-four`. Затем перейдите в этот новый каталог:

```shell
gatsby new tutorial-part-four https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-four
```

Затем установите некоторые другие необходимые зависимости в корне проекта. Мы будем использовать тему Typography 
Kirkham + мы попробуем CSS-in-JS библиотеку
[Glamorous](https://glamorous.rocks/):

```shell
npm install --save gatsby-plugin-typography gatsby-plugin-glamor glamorous typography-theme-kirkham
```

Давайте создадим сайт, похожий на то, что мы закончили в третьей части. Этот сайт будет иметь layout компонент и два page componentа:

`src/pages/index.js`

```jsx
import React from "react";

export default () => (
  <div>
    <h1>Amazing Pandas Eating Things</h1>
    <div>
      <img
        src="https://2.bp.blogspot.com/-BMP2l6Hwvp4/TiAxeGx4CTI/AAAAAAAAD_M/XlC_mY3SoEw/s1600/panda-group-eating-bamboo.jpg"
        alt="Group of pandas eating bamboo"
      />
    </div>
  </div>
);
```

`src/pages/about.js`

```jsx
import React from "react";

export default () => (
  <div>
    <h1>About Pandas Eating Lots</h1>
    <p>
      Мы единственный сайт, работающий на вашем компьютере, предназначенный для показа лучших фотографий и видео панд, которые едят много еды.
    </p>
  </div>
);
```

`src/layouts/index.js`

```jsx
import React from "react";
import g from "glamorous";
import { css } from "glamor";
import Link from "gatsby-link";

import { rhythm } from "../utils/typography";

const linkStyle = css({ float: `right` });

export default ({ children }) => (
  <g.Div
    margin={`0 auto`}
    maxWidth={700}
    padding={rhythm(2)}
    paddingTop={rhythm(1.5)}
  >
    <Link to={`/`}>
      <g.H3
        marginBottom={rhythm(2)}
        display={`inline-block`}
        fontStyle={`normal`}
      >
        Pandas Eating Lots
      </g.H3>
    </Link>
    <Link className={linkStyle} to={`/about/`}>
      About
    </Link>
    {children()}
  </g.Div>
);
```

`src/utils/typography.js`

```javascript
import Typography from "typography";
import kirkhamTheme from "typography-theme-kirkham";

const typography = new Typography(kirkhamTheme);

export default typography;
```

`gatsby-config.js` (must be in the root of your project, not under src)

```javascript
module.exports = {
  plugins: [
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

Добавьте вышеуказанные файлы, а затем выполните `gatsby develop` как обычно, и вы должны увидеть следующее:

![start](start.png)

У нас есть еще один небольшой сайт с layout и двумя страницами.

Теперь давайте начнем запросы 😋

## Запрос названия сайта

При создании сайтов обычно требуется повторно использовать общие фрагменты данных по всему сайту. Как _site title_ например. Посмотрите на `/about/` страницу. Вы заметите, что у нас есть название сайта и в layout компоненте (в
header) и в заголовке страницы About. Но что, если мы хотим изменить название сайта в какой-то момент в будущем? Нам нужно будет искать по всем нашим компонентам точки, используя название сайта и редактировать каждый экземпляр заголовка. Этот процесс является громоздким и подверженным ошибкам, особенно, поскольку сайты становятся все более сложными. Гораздо лучше сохранить название в одном месте, а затем _pull_ этот заголовок в компоненты, когда нам это нужно.

Чтобы решить эту проблему, мы можем добавить сайт "metadata" — как в page title или description — в файле `gatsby-config.js`. Давайте добавим название нашего сайта в файле
`gatsby-config.js` файл, а затем запросим его в наш layout и about page!

Отредактируйте `gatsby-config.js`:

```javascript{2-4}
module.exports = {
  siteMetadata: {
    title: `Blah Blah Fake Title`,
  },
  plugins: [
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

Перезапустите сервер разработки.

Затем отредактируйте два компонента:

`src/pages/about.js`

```jsx{3,5-7,14-23}
import React from "react";

export default ({ data }) =>
  <div>
    <h1>
      About {data.site.siteMetadata.title}
    </h1>
    <p>
     Мы единственный сайт, работающий на вашем компьютере, предназначенный для показа лучших фотографий и видеороликов панд, которые едят много еды.
    </p>
  </div>

export const query = graphql`
  query AboutQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`
```

`src/layouts/index.js`

```jsx{10,19,28-33}
import React from "react";
import g from "glamorous";
import { css } from "glamor";
import Link from "gatsby-link";

import { rhythm } from "../utils/typography";

const linkStyle = css({ float: `right` })

export default ({ children, data }) =>
  <g.Div
    margin={`0 auto`}
    maxWidth={700}
    padding={rhythm(2)}
    paddingTop={rhythm(1.5)}
  >
    <Link to={`/`}>
      <g.H3 marginBottom={rhythm(2)} display={`inline-block`}>
        {data.site.siteMetadata.title}
      </g.H3>
    </Link>
    <Link className={linkStyle} to={`/about/`}>
      About
    </Link>
    {children()}
  </g.Div>

export const query = graphql`
  query LayoutQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`
```

Это сработало!! 🎉

![fake-title-graphql](fake-title-graphql.png)

Но давайте восстановим реальный заголовок.

Одним из основных принципов Gatsby является то, что создатели нуждаются в непосредственной связи с тем, что они создают
([шляпа к Брету Виктору](http://blog.ezyang.com/2012/02/transcript-of-inventing-on-principleb/)).
Или, другими словами, когда вы вносите какие-либо изменения в код, вы должны немедленно увидеть эффект от этого изменения. Вы управляете входом Gatsby, и вы видите, что новый вывод отображается на экране.

Поэтому почти везде изменения, которые вы вносите, немедленно вступят в силу.

Попробуйте отредактировать заголовок в `siteMetadata`—измените название на «Pandas Eating Lots». Это изменение должно появиться очень быстро в вашем браузере.

## Подождите — откуда появился тэг graphql?

Возможно, вы заметили, что мы использовали
[tag function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Tagged_template_literals)
под названием `graphql`, но мы никогда на самом деле не _import_ этот `graphql` tag. Итак ... как это не выдает ошибку?

Короткий ответ таков: во время процесса сборки Gatsby запросы GraphQL выводятся из исходного источника для синтаксического анализа.

Более длинный ответ немного больше: Гэтсби заимствует технику из
[Relay](https://facebook.github.io/relay/) который преобразует наш исходный код в
[абстрактное синтаксическое дерево (AST)](https://en.wikipedia.org/wiki/Abstract_syntax_tree)
во время этапа сборки. Все `graphql`-tagged шаблоны находятся в
[`file-parser.js`](https://github.com/gatsbyjs/gatsby/blob/v1.6.3/packages/gatsby/src/internal-plugins/query-runner/file-parser.js#L63)
и
[`query-compiler.js`](https://github.com/gatsbyjs/gatsby/blob/v1.6.3/packages/gatsby/src/internal-plugins/query-runner/query-compiler.js),
который эффективно удаляет их из исходного исходного кода. Это означает, что `graphql` tag не выполняется так, как мы могли бы ожидать, поэтому нет ошибки, несмотря на то, что мы технически используем неопределенный тег в нашем источнике.

## Что теперь?

Затем вы узнаете, как извлекать данные на свой сайт Gatsby с помощью GraphQL с исходными плагинами в [пятой части](/tutorial/part-five/) учебника.
