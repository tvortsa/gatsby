---
title: Введение в использование CSS в Gatsby
typora-copy-images-to: ./
---

Добро пожаловать во вторую часть урока по Gatsby!

## Что в этом учебнике?

В этой части мы рассмотрим варианты стилизации сайтов Gatsby и погрузимся глубже в использование компонентов React для создания сайтов.

## Сборка на компонентах

Один из больших умственных сдвигов, которые вы делаете, когда начинаете строить с помощью компонентов (если вы уже являетесь разработчиком), заключается в том, что теперь ваши CSS, HTML и JavaScript тесно связаны и часто живут даже в одном файле.

Хотя, казалось бы, простое изменение, это имеет глубокие последствия для того, как вы думаете о создании сайтов.

Возьмем пример создания пользовательской кнопки. Раньше вы создавали бы класс CSS (возможно, `.primary-button`) с вашими настраиваемыми стилями, а затем всякий раз, когда вы хотите применить эти стили, например

```html
<button class="primary-button">
  Нажми на меня
</button>
```

В мире компонентов, вы вместо этого создаете `PrimaryButton` компонент с вашими стилями кнопок и используйте его на своем сайте, например:

<!-- prettier-ignore -->
```jsx
<PrimaryButton>Нажми на меня</PrimaryButton>
```

Компоненты становятся базовыми строительными блоками вашего сайта. Вместо того, чтобы ограничиваться тем, что предоставляет браузер, например `<button />`, вы можете легко создавать новые строительные блоки, которые элегантно отвечают потребностям ваших проектов.

## Создание глобальных стилей

Каждый сайт имеет своего рода глобальный стиль. Сюда относятся такие вещи, как типография и цвет фона. Эти стили устанавливают общее ощущение сайта - так же, как цвет и текстура стены создают общее ощущение комнаты.

Часто люди используют что-то вроде Bootstrap или Foundation для своих глобальных стилей. Проблема с ними, однако, заключается в том, что их трудно настроить, и они не предназначены для работы с компонентами React.

В этом уроке давайте рассмотрим библиотеку JavaScript, называемую
[Typography.js](https://github.com/kyleamathews/typography.js) который генерирует глобальные стили и особенно хорошо работает с Gatsby и React.

### Typography.js

Typography.js это библиотека JavaScript, которая генерирует типографский CSS.

Вместо прямого задания `font-size` различных HTML-элементов, вы говорите
Typography.js такие вещи, как ваши `baseFontSize` и `baseLineHeight` и
на основе этого, он генерирует базу CSS для всех ваших элементов.

Это делает тривиальным изменить размер шрифта для всех элементов на сайте без необходимости прямого изменения десятков CSS правил.

Использование этого выглядит примерно так:

```javascript
import Typography from "typography";

const typography = new Typography({
  baseFontSize: "18px",
  baseLineHeight: 1.45,
  headerFontFamily: [
    "Avenir Next",
    "Helvetica Neue",
    "Segoe UI",
    "Helvetica",
    "Arial",
    "sans-serif",
  ],
  bodyFontFamily: ["Georgia", "serif"],
});
```

## Gatsby плагины

Но прежде чем мы сможем вернуться к созданию и опробовать Typography.js, давайте быстро перейдем и поговорим о плагинах Gatsby.

Вероятно, вы знакомы с идеей плагинов. Многие программные системы поддерживают добавление пользовательских плагинов для добавления новых функциональных возможностей или даже изменения основной работы программного обеспечения.

Плагины Gatsby работают также.

Члены сообщества (такие как ты!) могут вносить плагины (небольшие количества
JavaScript кода) которые другие могут затем использовать при создании сайтов Гэтсби.

Там уже десятки плагинов! Проверьте их на
[plugins section of the site](/docs/plugins/).

Наша цель с плагинами Gatsby - сделать их простыми для установки и использования. Почти на каждом сайте Gatsby, который вы создаете, вы будете устанавливать плагины. Во время работы над остальной частью учебника у вас будет много возможностей для практики установки и использования плагинов.

## Установка вашего первого плагина Gatsby

Начнем с создания нового сайта. На данный момент, вероятно, имеет смысл закрыть окна терминала, которые вы использовали для создания учебника-части-одного, чтобы вы случайно не приступили к созданию учебника-части-2 в неположенном месте. Если вы не закроете учебник-часть-один до создания учебника-части-2, вы увидите, что учебник-часть-два появляется на localhost: 8001 вместо localhost: 8000.

Как и в первой части, откройте новое окно терминала и запустите следующие коммиты, чтобы создать новый сайт Gatsby в каталоге под названием`tutorial-part-two`. Затем перейдите в этот новый каталог:

```shell
gatsby new tutorial-part-two https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

Это создает новый сайт со следующей структурой.

```shell
├── package.json
├── src
│   └── pages
│       └── index.js
```

Это минимальная настройка для сайта Гэтсби.

Чтобы установить плагин, есть два шага. Во-первых, вы устанавливаете NPM-пакет плагина, а во-вторых, вы добавляете плагин на сайт `gatsby-config.js`.

Typography.js имеет плагин Gatsby, поэтому давайте установим это, запустив:

```shell
npm install --save gatsby-plugin-typography
```

Затем в редакторе кода создайте файл в корне вашей папки проекта с именем`gatsby-config.js`.
Здесь вы добавляете плагины вместе с другой конфигурацией сайта.

Скопируйте следующее в `gatsby-config.js`

```javascript
module.exports = {
  plugins: [`gatsby-plugin-typography`],
};
```

Gatsby считывает файл конфигурации сайта при запуске. Здесь мы говорим, что он ищет плагин с именем `gatsby-plugin-typography`. Гэтсби знает, что нужно искать плагины, которые являются пакетами NPM, поэтому он найдет тот пакет, который мы установили ранее.

Теперь запустите `gatsby develop`. После загрузки сайта, если вы проверите сгенерированный HTML-код с помощью инструментов разработчика Chrome, вы увидите, что плагин типографии добавил ` <style>` element к `<head>` элементу с его сгенерированным CSS.

![typography-styles](typography-styles.png)

Скопируйте следующее в ваш `src/pages/index.js` поэтому мы можем лучше увидеть эффект типографского CSS, созданного Typography.js.

```jsx
import React from "react";

export default () => (
  <div>
    <h1>Richard Hamming on Luck</h1>
    <div>
      <p>
        From Richard Hamming’s classic и must-read talk, “<a href="http://www.cs.virginia.edu/~robins/YouиYourResearch.html">
          You и Your Research
        </a>”.
      </p>
      <blockquote>
        <p>
          На самом деле есть элемент удачи, и нет, нет. Готовый ум рано или поздно находит что-то важное и делает это. Так что да, это удача.{" "}
          <em>
            Особая вещь, которую вы делаете, это удача, но что вы делаете что-то не.
          </em>
        </p>
      </blockquote>
    </div>
    <p>Posted April 09, 2011</p>
  </div>
);
```

Теперь ваш сайт должен выглядеть так::

![typography-not-centered](typography-not-centered.png)

Давайте сделаем быстрое улучшение. На многих сайтах имеется один столбец с центром в центре страницы. Чтобы создать это, добавьте следующие стили в
`<div>` в `src/pages/index.js`.

```jsx{4,25}
import React from "react";

export default () =>
  <div style={{ margin: '3rem auto', maxWidth: 600 }}>
    <h1>Richard Hamming on Luck</h1>
    <div>
      <p>
        From Richard Hamming’s classic и must-read talk, “<a href="http://www.cs.virginia.edu/~robins/YouиYourResearch.html">
          You и Your Research
        </a>”.
      </p>
      <blockquote>
        <p>
          На самом деле есть элемент удачи, и нет, нет. Готовый ум рано или поздно находит что-то важное и делает это. Так что да, это удача.{" "}
          <em>
            Особая вещь, которую вы делаете, это удача, но что вы делаете что-то не.
          </em>
        </p>
      </blockquote>
    </div>
    <p>Posted April 09, 2011</p>
  </div>
```

![basic-typography-centered](typography-centered.png)

Ах, это начинает выглядеть красиво! То, что мы видим здесь, это CSS CSS по умолчанию. Однако мы можем легко настроить его. Давайте сделаем это.

Создайте новый каталог на своем сайте. `src/utils`. В этом каталоге создайте файл с именем
`typography.js`. В этом файле добавьте следующий код.

```javascript
import Typography from "typography";

const typography = new Typography({ baseFontSize: "18px" });

export default typography;
```

Затем установите этот модуль для использования `gatsby-plugin-typography` в качестве его конфигурации в нашем файле` gatsby-config.js`.

```javascript{2..9}
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

ОСтановите `gatsby develop` набравg <kbd>Ctrl + c</kbd> в окно терминала, в котором запущен процесс разработки. Затем запустите `gatsby develop` снова, чтобы перезапустить его. Это позволит изменить наши плагины.

Теперь все размеры шрифта текста должны быть немного больше. Попробуйте изменить
`baseFontSize` на `24px` затем `12px`. Все элементы изменяются как их
`font-size` основывается на `baseFontSize`.

_Обратите внимание, что если вы используете `gatsby-plugin-typography` с начальным стартом по умолчанию вам нужно удалить дефолтный index.css используемый этим стартером, поскольку он переопределяет Typography.js CSS_

Есть
[многие темы доступныe](https://github.com/KyleAMathews/typography.js#published-typographyjs-themes)
для Typography.js. Попробуем пару. В своем терминале в корне вашего сайта запустите:

```shell
npm install --save typography-theme-bootstrap typography-theme-lawton
```

Чтобы использовать Bootstrap тему, измените ваш typography код на:

```javascript{2,4}
import Typography from "typography";
import bootstrapTheme from "typography-theme-bootstrap";

const typography = new Typography(bootstrapTheme);

export default typography;
```

![typography-bootstrap](typography-bootstrap.png)

Темы также могут добавлять Google Fonts. Тема Lawton, которую мы установили вместе с темой Bootstrap, делает это. Замените код модуля типографии следующим, а затем перезапустите сервер dev (необходимый для загрузки новых шрифтов Google).

```javascript{2-3,5}
import Typography from "typography";
// import bootstrapTheme from "typography-theme-bootstrap"
import lawtonTheme from "typography-theme-lawton";

const typography = new Typography(lawtonTheme);

export default typography;
```

![typography-lawton](typography-lawton.png)

_Challenge:_ Typography.js has more than 30 themes!
[Try them live](http://kyleamathews.github.io/typography.js) or check out
[the complete list](https://github.com/KyleAMathews/typography.js#published-typographyjs-themes) и try installing one on your current Gatsby site.

## Компонент CSS

У Gatsby есть множество возможностей для компоновки компонентов. В этом уроке мы рассмотрим один очень популярный метод: CSS-модули.

### CSS-in-JS

Пока мы не будем раскрывать CSS-in-JS в этом первоначальном учебном пособии мы рекомендуем вам изучить CSS-in-JS libraries потому что они решают многие проблемы с традиционным CSS плюс помочь сделать ваши компоненты React более умными. There are mini-tutorials for two libraries, [Glamor](/docs/glamor/) и [Styled Components](/docs/styled-components/). Check out the following resources for background reading on CSS-in-JS:

[Кристофер "vjeux" Chedeau's 2014 презентация, которая вызвала это движение](https://speakerdeck.com/vjeux/react-css-in-js)
as well as
[Mark Dalgleish's последнее сообщение "A Unified Styling Language"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660).

### CSS Modules

Давайте исследовать **CSS Modules**.

Цитата из
[the CSS Module homepage](https://github.com/css-modules/css-modules):

> **CSS Module** это CSS-файл, в котором все имена классов и имена анимаций
> локальны по умолчанию.

CSS Modules очень популярен, так как позволяет писать CSS как обычно, но с гораздо большей безопасностью. Инструмент автоматически делает имена классов и анимаций уникальными, поэтому вам не нужно беспокоиться о столкновениях имен селекторных имен.

CSS Modules настоятельно рекомендуется для тех, кто работает с Gatsby (и
React в целом).

Gatsby работает из коробки с CSS Modules.

Давайте построим страницу, используя CSS Modules.

Сначала давайте создадим новый `Container` компонент, который мы будем использовать для каждого из
CSS-in-JS примера. Создайте папку `components` в `src/components` и
тогда, в этом каталоге создайте файл с именем `container.js` и вставьте следующее:

```javascript
import React from "react";

export default ({ children }) => (
  <div style={{ margin: "3rem auto", maxWidth: 600 }}>{children}</div>
);
```

Затем создайте новый component page создав файл на
`src/pages/about-css-modules.js`:

```javascript
import React from "react";

import Container from "../components/container";

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
  </Container>
);
```

Вы заметите, что мы импортировали `Container` компонент, который мы только что создали.

Теперь ваша страница должна выглядеть так::

![css-modules-1](css-modules-1.png)

Давайте создадим список людей с именами, аватарами и краткой латинской биографией.

Сначала давайте создадим файл для CSS в
`src/pages/about-css-modules.module.css`. Вы заметите, что имя файла заканчивается на `.module.css` а не `.css` как обычно. Так мы говорим Гэтсби, что этот CSS-файл должен обрабатываться как модули CSS.

Вставьте в файл следующее::

```css
.user {
  display: flex;
  align-items: center;
  margin: 0 auto 12px auto;
}

.user:last-child {
  margin-bottom: 0;
}

.avatar {
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
}

.description {
  flex: 1;
  margin-left: 18px;
  padding: 12px;
}

.username {
  margin: 0 0 12px 0;
  padding: 0;
}

.excerpt {
  margin: 0;
}
```

Теперь импортируйте этот файл в `about-css-modules.js` страницу, которую мы создали ранее, добавив следующие строки 2 и 3.
( `console.log(styles)` код регистрирует полученный импорт, чтобы мы могли видеть, как выглядит обработанный файл).

```javascript
import styles from "./about-css-modules.module.css";
console.log(styles);
```

Если вы откроете консоль разработчика (используя, например, инструменты разработчика Firefox или Chrome) в вашем браузере вы увидите:

![css-modules-console](css-modules-console.png)

Если вы сравните это с нашим CSS-файлом, вы увидите, что каждый класс теперь является ключом в импортированном объекте, указывающем на длинную строку e.g. `avatar` указывает на
`about-css-modules-module---avatar----hYcv`. Это имена классов, которые генерируют модули CSS. Они гарантированно будут уникальными на вашем сайте. и потому, что вам нужно импортировать их для использования классов, никогда не возникает вопросов о том, где используется какой-либо CSS.

Давайте используем наши стили для созданиякомпонента `User`.

Давайте создадим новый компонент inline в `about-css-modules.js` page
component. Общее правило - это: если вы используете компонент в нескольких местах на сайте, он должен быть в собственном файле модуля в папке `components`. Но, если он используется только в одном файле, создавайте его в inline.

измените `about-css-modules.js` поэтому он выглядит следующим образом:

```jsx{6-17,23-30}
import React from "react";
import styles from "./about-css-modules.module.css";
console.log(styles);

import Container from "../components/container";

const User = props =>
  <div className={styles.user}>
    <img src={props.avatar} className={styles.avatar} alt="" />
    <div className={styles.description}>
      <h2 className={styles.username}>
        {props.username}
      </h2>
      <p className={styles.excerpt}>
        {props.excerpt}
      </p>
    </div>
  </div>

export default () =>
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
      excerpt="I'm Jane Doe. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
      excerpt="I'm Bob smith, a vertically aligned type of guy. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
  </Container>
```

Готовая страница теперь должна выглядеть так::

![css-modules-final](css-modules-final.png)

### Другие варианты CSS

Gatsby поддерживает практически все возможные варианты стилизации (если еще нет плагина для вашего любимого варианта CSS,
[пожалуйста, сделайте его!](/docs/how-to-contribute/))

* [Sass](/packages/gatsby-plugin-sass/)
* [Emotion](/packages/gatsby-plugin-emotion/)
* [JSS](/packages/gatsby-plugin-jss/)
* [Stylus](/packages/gatsby-plugin-stylus/)
* и more!

## Что дальше

Теперь продолжайте [Part Three](/tutorial/part-three/) учебника.
