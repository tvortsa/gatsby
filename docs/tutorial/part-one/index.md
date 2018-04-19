---
title: Введение в основы Gatsby
typora-copy-images-to: ./
---

Привет, парень Гэтсби! Добро пожаловать в _part one_ учебника нашего сообщества Gatsby.js.

## Что в этом учебнике?

В этом уроке вы будете осторожно представлены в среде разработки Gatsby, как создавать страницы компонентов, а также как создавать и развертывать сайты Gatsby.

## Проверьте свою среду разработки

Вам понадобится последняя версия Node.js.

Node.js is a programming tool for running JavaScript on servers and in your
computer's terminal. Gatsby is built using Node.js.

Открыть окно терминала. См
[terminal instructions for Mac users](http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/) and
[terminal instructions for Windows users](https://www.quora.com/How-do-I-open-terminal-in-windows). In your terminal window, type `node --version` and hit ENTER, then `npm --version` and hit ENTER (tip: to run a specified command, you must type the command into your terminal and then press ENTER. Then the command will run).

You should see something like:

![Check if node.js/npm is installed](check-versions.png)

Gatsby supports versions of Node back to v6 and npm to v3.

Если у вас нет Node.js , пройдите на кhttps://nodejs.org/ и установите рекомендованную версию для вашей операционной системы.

## Установите "Hello World" starter

Gatsby использует "starters" для запуска новых проектов. Starters
частично собранные Gatsby сайты которые предварительно настроены, чтобы помочь вам быстрее двигаться.
Есть несколько официальных стартеров, и многие другие участвовали в сообществе Гэтсби! [See the Starters page for the full list](/docs/gatsby-starters/).

Чтобы установить starter, сначала установите Gatsby's программу командной строки путем запуска команд:

```sh
npm install --global gatsby-cli
```

После установки, откройте новое окно терминала и запустите следующие команды, чтобы создать новый сайт Gatsby в каталоге под названием`tutorial-part-one` а затем перейдите в этот новый каталог:

```sh
gatsby new tutorial-part-one https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-one
```

Эта команда загружает файлы для стартера, а затем устанавливает необходимые пакеты npm.

Теперь давайте попробуем запустить Гэтсби!

У Gatsby есть встроенный сервер разработки. Давайте запустим его, выполнив следующую команду:

```sh
gatsby develop
```

Вы должны в ближайшее время увидеть текст, внизу, который говорит `The development server is listening at:` [http://localhost:8000](http://localhost:8000). Откройте этот адрес в своем браузере и...

![Gatsby.js hello world](hello-world.png)

Да! Работает!!!

Too cool 😎

Сервер разработки Gatsby является сервером «горячей перезагрузки», что означает любое изменение, которое вы делаете для своего React.js компоненты страницы (и позже мы узнаем, что ваши файлы данных)
будет сразу виден и / или загружен в браузере.

Это делает разработку намного быстрее и веселее.

Давай попробуем.

Вам понадобится программное обеспечение для редактирования кода для следующей части этого руководства.
[VS Code](https://code.visualstudio.com/) это хороший. С помощью редактора кода,
откройте папку на вашем компьютере "tutorial-part-one," который был автоматически создан в выбранном вами местоположении, когда вы запускали `gatsby new` команду терминала выше.

Как только вы откроете папку "tutorial-part-one" в редакторе кода наступит время редактирования вашего сайта. Вы увидите группы каталогов и файлов; найти файл в этом месте: `src/pages/index.js`. Когда вы откроете этот файл, попробуйте изменить "Hello
world!" в компоненте страницы "Hello Gatsby!". После сохранения изменений текст в браузере должен измениться в течение секунды (tip: вам всегда нужно сохранять изменения до того, как они появятся в вашем браузере.).

Попробуйте другие трюки, например, ниже:

1. Gatsby позволяет добавить "inline styles" через JavaScript style "prop" (мы узнаем о других вариантах стиля позже).

   Попробуйте заменить компонент страницы на этот:

```jsx
import React from "react";

export default () => <div style={{ color: `blue` }}>Hello Gatsby!</div>;
```

Измените цвет на "pink". Затем "tomato".

2. Добавь немного paragraph text.

```jsx{5-6}
import React from "react";

export default () =>
 <div style={{ color: `tomato` }}>
   <h1>Hello Gatsby!</h1>
   <p>What a world.</p>
 </div>
```

3. Добавьте изображение (в этом случае случайный из Unsplash)

```jsx{7}
import React from "react";

export default () =>
 <div style={{ color: `tomato` }}>
   <h1>Hello Gatsby!</h1>
   <p>What a world.</p>
   <img src="https://source.unsplash.com/random/400x200" alt="" />
 </div>
```

Теперь ваш экран должен выглядеть примерно так::

![Screen Shot 2017-06-03 at 11.57.10 AM](moving-along.png)

## Связывание страниц

Веб-сайты - это страницы и ссылки между страницами. В то время как у нас теперь есть довольно сладкая первая страница - одна страница, и никакие ссылки не очень чувствительны к webby. Итак, давайте создадим новую страницу.

Сначала создайте ссылку на новую страницу.

Для этого импортируйте `<Link>` компонента из `gatsby-link` пакет, который был установлен вместе со стартером.

В отличие от обычного HTML `<a>` element, Gatsby's `Link` component использования `to` для указания страницы, на которую вы хотите установить ссылку. Давайте установим ссылку на страницу с именем пути `/page-2/`. Попробуйте добавить, что. Как только вы закончите, компонент страницы должен выглядеть так:

```jsx{2,9-12}
import React from "react";
import Link from "gatsby-link";

export default () =>
  <div style={{ color: `tomato` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
    <br />
    <div>
      <Link to="/page-2/">Link</Link>
    </div>
  </div>
```

Если вы нажмете на эту ссылку в браузере, вы увидите:

![Gatsby.js development 404 page](dev-404.png)

То, что вы видите, это Gatsby.js development 404 page. Давайте сделаем то, что он говорит, и создадим React.js page component в `src/pages/page-2.js`.

Сделайте второй компонент страницы похожим на:

```jsx
import React from "react";
import Link from "gatsby-link";

export default () => (
  <div>
    <p>Hello world from my second Gatsby page</p>
    <Link to="/">back home</Link>
  </div>
);
```

Сохраните это, и теперь вы сможете щелкнуть назад и вперед между двумя страницами!

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/images/clicking-2.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

_Challenge_: Используя приведенные выше инструкции в качестве подсказок, посмотрите, можно ли создать третью страницу и связать ее с домашней страницей.

## Интерактивная страница

Одна хорошая вещь об использовании Gatsby для создания веб-сайтов и других инструментов заключается в том, что легче добавить интерактивность к вашим страницам. React.js был разработан для
Facebook.com и используется во многих других веб-приложениях мирового уровня.

Давайте посмотрим, как добавить интерактивные элементы на наши страницы. Начнем с счетчика.

Мы начнем с создания новой ссылки на страницу в `/counter`/ от нашего оригинала
`index.js` компонента страницы `<Link to="/counter/">Counter</Link>`.

```jsx{13-15}
import React from "react";
import Link from "gatsby-link";

export default () =>
  <div style={{ color: `tomato` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
    <br />
    <div>
      <Link to="/page-2/">Link</Link>
    </div>
    <div>
      <Link to="/counter/">Counter</Link>
    </div>
  </div>
```

Добавьте эту ссылку, нажмите на нее, а затем создадим компонент страницы «Hello World» для `/counter/` как прежде. Но вместо использования "functional component" как мы это делали раньше, на этот раз мы создадим "class" компонент в `src/pages/counter.js`.

```jsx
import React from "react";

class Counter extends React.Component {
  render() {
    return <div>Hello Class Component</div>;
  }
}

export default Counter;
```

Форма class в React позволяет нам иметь состояние компонента. Нам это понадобится для нашего счетчика.

Давайте продолжим заполнять наш счетчик. Давайте добавим две кнопки. Один для увеличения и один для уменьшения счетчика.

```jsx{5-12}
import React from "react";

class Counter extends React.Component {
  render() {
    return (
      <div>
        <h1>Counter</h1>
        <p>current count: 0</p>
        <button>plus</button>
        <button>minus</button>
      </div>
    )
  }
}

export default Counter
```

Теперь у нас есть все необходимое, чтобы сделать хороший счетчик. Давайте сделаем это вживую. Сначала мы настроим состояние компонента.

```jsx{4-7,13}
import React from "react";

class Counter extends React.Component {
  constructor() {
    super()
    this.state = { count: 0 }
  }

  render() {
    return (
      <div>
        <h1>Counter</h1>
        <p>current count: {this.state.count}</p>
        <button>plus</button>
        <button>minus</button>
      </div>
    )
  }
}

export default Counter
```

Теперь мы показываем текущий счет из состояния компонента.

Давайте теперь изменим состояние, когда мы нажимаем на наши кнопки.

```jsx{14-19}
import React from "react";

class Counter extends React.Component {
  constructor() {
    super()
    this.state = { count: 0 }
  }

  render() {
    return (
      <div>
        <h1>Counter</h1>
        <p>current count: {this.state.count}</p>
        <button onClick={() => this.setState({ count: this.state.count +
          1 })}>plus
        </button>
        <button onClick={() => this.setState({ count: this.state.count -
          1 })}>minus
        </button>
      </div>
    )
  }
}

export default Counter
```

ВОт и все! Работающий React.js counter внутри вашего статичного вебсайта 👌

_Bonus challenge_: Одна забавная вещь заключается в том, что горячая перезагрузка предназначена не только для контента и стилей; он работает и над кодом. В настоящее время, когда вы нажимаете кнопки на счетчике, цифры идут вверх и вниз с шагом 1. Попробуйте сделать счетчик вверх и вниз разным шагом (например, 5).

## Развертывание Gatsby.js сайтов

Gatsby.js это _static site generator_, что означает, что нет серверов для установки или создания сложных баз данных для развертывания. Instead, команда Gatsby `build` создает каталог статических файлов HTML и JavaScript, которые можно развернуть на статическом хостинге сайтов.

Попробуем использовать [Surge](http://surge.sh/) для развертывания нашего первого веб-сайта Gatsby. Surge является одним из многих "static site hosts" которые позволяют размещать сайты Гэтсби.

Если вы ранее не устанавливали и не настраивали Surge, откройте новое окно терминала и установите их терминальный инструмент:

```bash
npm install --global surge

# Then create a (free) account with them
surge
```

Затем создайте свой сайт, выполнив следующую команду в терминале в корне вашего сайта (подсказка: убедитесь, что вы используете эту команду в корневом каталоге вашего сайта, в этом случае в папке с учебником и частью, которая вы можете сделать, открыв новую вкладку в том же окне, которое вы использовали для запуска `gatsby develop`):

```bash
gatsby build
```

Здание должно 15-30 секунд. С этого момента, полезно взглянуть на файлы, которые команда `gatsby build` подготовила к развертыванию. Взгляните на список сгенерированных файлов, введя следующую команду терминала в корневой каталог вашего сайта, который позволит вам взглянуть на папку `public`:

```bash
ls public
```

Затем, наконец, разверните свой сайт, опубликовав сгенерированные файлы в surge.sh.

```bash
surge public/
```

Как только это закончится, вы должны увидеть в своем терминале что-то вроде:

![Screenshot of publishing Gatsby site with Surge](surge-deployment.png)

Откройте веб-адрес, указанный в нижней строке (`lowly-pain.surge.sh` в этом случае) и вы увидите свой недавно опубликованный сайт! Хорошая работа!

## Что дальше

В этом уроке вы установили Gatsby, поиграли в среде разработки и развернули свой первый сайт! Потрясающие! Надеемся, вы до сих пор наслаждаетесь собой. Не стесняйтесь теперь перейти к части второй учебника,
["Введение в использование CSS в Gatsby"](/tutorial/part-two/), или исследуйте остальные части сайта.
