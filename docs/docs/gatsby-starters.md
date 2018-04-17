---
title: 'Gatsby Starters'
---

Инструмент командной строки Gatsby CLI позволяет установить "starters". Это частично построенные сайты, предварительно сконфигурированные, чтобы помочь вам быстрее двигаться по созданию определенного типа сайта.

При создании нового сайта вы можете указать стартер для создания нового сайта, например

`gatsby new [SITE_DIRECTORY] [URL_OF_STARTER_GITHUB_REPO]`

Например, чтобы быстро создать блог используя Gatsby, вы можете установить Gatsby
Starter Blog запустив:

`gatsby new blog https://github.com/gatsbyjs/gatsby-starter-blog`

Что загрузит файлы и инициализирует сайт, комантдой `npm install`

Если вы не укажете starter, ваш сайт будет создан из
[default starter](https://github.com/gatsbyjs/gatsby-starter-default).

Есть несколько стартеров, которые были созданы. Создайте PR чтобы включить ваши собственные!

Официальные:

* [gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default)
  [(demo)](http://gatsbyjs.github.io/gatsby-starter-default/)
* [gatsby-starter-blog](https://github.com/gatsbyjs/gatsby-starter-blog)
  [(demo)](http://gatsbyjs.github.io/gatsby-starter-blog/)
* [gatsby-starter-hello-world](https://github.com/gatsbyjs/gatsby-starter-hello-world)
  [(demo)](https://aberrant-fifth.surge.sh/)

Сообщество:

* [gatsby-starter-blog-no-styles](https://github.com/noahg/gatsby-starter-blog-no-styles)
  [(demo)](http://capricious-spring.surge.sh/)

  Особенности:

  * То же, что и официальный блог gatsby-starter-blog, но при удалении всего стиля

* [gatsby-material-starter](https://github.com/Vagr9K/gatsby-material-starter)
  [(demo)](https://vagr9k.github.io/gatsby-material-starter/)

  Особенности:

  * React-MD для Material design
  * Sass/SCSS
  * Tags
  * Categories
  * Google Analytics
  * Disqus
  * Offline support
  * Web App Manifest
  * SEO
  * [Full list here!](https://github.com/Vagr9K/gatsby-material-starter#features)

* [gatsby-typescript-starter](https://github.com/fabien0102/gatsby-starter)
  [(demo)](https://fabien0102-gatsby-starter.netlify.com/)

  Особенности:

  * Semantic-ui for styling
  * TypeScript
  * Offline support
  * Web App Manifest
  * Jest/Enzyme testing
  * Storybook
  * Markdown linting
  * [Full list here!](https://github.com/fabien0102/gatsby-starter#whats-inside)

* [gatsby-starter-bootstrap](https://github.com/jaxx2104/gatsby-starter-bootstrap)
  [(demo)](https://jaxx2104.github.io/gatsby-starter-bootstrap/)

  Особенности:

  * Bootstrap CSS framework
  * Single column layout
  * Basic components: SiteNavi, SitePost, SitePage

* [gatsby-blog-starter-kit](https://github.com/dschau/gatsby-blog-starter-kit)
  [(demo)](https://dschau.github.io/gatsby-blog-starter-kit/)

  Особенности:

  * Blog post listing with previews for each blog post
  * Navigation between posts with a previous/next post button
  * Tags and tag navigation

* [gatsby-starter-casper](https://github.com/haysclark/gatsby-starter-casper)
  [(demo)](https://haysclark.github.io/gatsby-starter-casper/)

  Особенности:

  * Page pagination
  * CSS
  * Tags
  * Google Analytics
  * Offline support
  * Web App Manifest
  * SEO
  * [Full list here!](https://github.com/haysclark/gatsby-starter-casper#features)

* [gatsby-advanced-starter](https://github.com/Vagr9K/gatsby-advanced-starter)
  [(demo)](https://vagr9k.github.io/gatsby-advanced-starter/)

  Features:

  * Отлично подходит для изучения расширенных функций и их реализаций
  * Не содержит каких-либо фреймворков пользовательского интерфейса
  * Обеспечивает только skeleton
  * Tags
  * Categories
  * Google Analytics
  * Disqus
  * Offline support
  * Web App Manifest
  * SEO
  * [Full list here!](https://github.com/Vagr9K/gatsby-advanced-starter#features)

* [glitch-gatsby-starter-blog](https://github.com/100ideas/glitch-gatsby-starter-blog/)
  ([demo](https://gatsby-starter-blog.glitch.me))

  Особенности:

  * [live-edit](https://glitch.com/edit/#!/remix/gatsby-starter-blog) a temp,
    anon copy of app
  * same code as
    [gatsby-starter-blog](https://github.com/gatsbyjs/gatsby-starter-blog)
    (mostly)
  * free hosting & web IDE on glitch.com
  * HMR working w/ glitch IDE (see
    [note](https://github.com/100ideas/glitch-gatsby-starter-blog/blob/5fce8999bd952087ecdc74c9787a0cb3cb884371/README.md#enabling-hmr))
  * caution:
    * app running in **develop** mode
    * glitch serves assets over CDN, API unclear
    * virtual server container provides
      [**128MB** for app](https://glitch.com/faq#restrictions) (512MB for
      assets)
    * server can't install certain gatsby plugins (`sharp`-based; out of mem?)

* [gatsby-starter-grommet](https://github.com/alampros/gatsby-starter-grommet)
  [(demo)](https://alampros.github.io/gatsby-starter-grommet/)

  Features:

  * Barebones конфигурации для использования [Grommet](https://grommet.github.io/)
    система проектирования
  * Uses Sass (with CSS modules support)

* [gatsby-starter-basic](https://github.com/PrototypeInteractive/gatsby-react-boilerplate)
  [(demo)](https://prototypeinteractive.github.io/gatsby-react-boilerplate/)

  Features:

  * Basic configuration and folder structure
  * Uses PostCSS and Sass (with autoprefixer and pixrem)
  * Uses Bootstrap 4 grid
  * Leaves the styling to you
  * Uses data from local json files
  * Contains Node.js server code for easy, secure, and fast hosting

* [gatsby-starter-typescript](https://github.com/haysclark/gatsby-starter-typescript)
  [(demo)](https://haysclark.github.io/gatsby-starter-typescript/)

  Features:

  * TypeScript

* [gatsby-starter-default-i18n](https://github.com/angeloocana/gatsby-starter-default-i18n)
  [(demo)](https://gatsby-starter-default-i18n.netlify.com)

  Features:

  * localization (Multilanguage)

* [gatsby-starter-contentful-i18n](https://github.com/mccrodp/gatsby-starter-contentful-i18n) [(demo)](https://gatsby-starter-contentful-i18n.netlify.com/)

  Features:

  * Localization (Multilanguage)
  * Dynamic content from Contentful CMS
  * Integrates [i18n plugin starter](https://github.com/angeloocana/gatsby-starter-default-i18n) and [using-contentful](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-contentful) repos

* [gatsby-starter-gatsbythemes](https://github.com/saschajullmann/gatsby-starter-gatsbythemes)
  [(demo)](https://themes.gatsbythemes.com/gatsby-starter/)

  Особенности:

  * CSS-in-JS via [Emotion](https://github.com/emotion-js/emotion).
  * Jest и Enzyme для тестирования.
  * Eslint в dev mode с airbnb config и prettier правила форматирования.
  * React 16.
  * Основной блог с сообщениями под src/pages/blog. Существует также скрипт, который создает новую запись в блоге (post.sh).
  * Data per JSON files.
  * Несколько основных компонентов (Navigation, Footer, Layout).
  * Layout components make use of
    [Styled-System](https://github.com/jxnblk/styled-system).
  * Google Analytics (you just have to enter your tracking-id).
  * Gatsby-Plugin-Offline который включает в себя Service Workers.
  * [Prettier](https://github.com/prettier/prettier) для единой кодовой базы.
  * [Normalize](https://github.com/necolas/normalize.css/) css (7.0).
  * [Feather](https://feather.netlify.com/) icons.
  * Font styles taken from [Tachyons](http://tachyons.io/).

* [gatsby-starter-netlify-cms](https://github.com/AustinGreen/gatsby-starter-netlify-cms)
  [(demo)](https://gatsby-netlify-cms.netlify.com/)

  Особенности:

  * A simple blog built with Netlify CMS
  * Basic directory organization
  * Uses [Bulma](https://bulma.io/) for styling
  * Visit [the repo](https://github.com/AustinGreen/gatsby-starter-netlify-cms)
    to learn how to set up authentication, and begin modeling your content.

* [gatsby-starter-portfolio-emma](https://github.com/LeKoArts/gatsby-starter-portfolio-emma)
  [(demo)](https://portfolio-emma.netlify.com/)

  Целевая аудитория - это дизайнеры и фотографы.

  Особенности:

  * Full-width photo grid-layout (with [gatsby-image](https://using-gatsby-image.gatsbyjs.org/))
  * Minimalistic light theme with large images
  * Создайте свои проекты в Markdown
  * Styling with SCSS and
    [Typography.js](https://kyleamathews.github.io/typography.js/)
  * Легко настраиваемый
  * И другие хорошие вещи (SEO, Offline Support, WebApp Manifest Support)

* [gatsby-starter-portfolio-emilia](https://github.com/LeKoArts/gatsby-starter-portfolio-emilia)
  [(demo)](https://portfolio-emilia.netlify.com/)

 Целевая аудитория - это дизайнеры и фотографы.

  Features:

  * Фокус на большие изображения (with [gatsby-image](https://using-gatsby-image.gatsbyjs.org/))
  * Темная тема с хэдером HeroPatterns
  * CSS Grid и Styled сомпоненты
  * One-Page layout с sub-pages для проектов
  * Легко настраиваемый
  * React Overdrive transitions
  * Создайте свои проекты в Markdown
  * И другие хорошие вещи (SEO, Offline Support, WebApp Manifest Support)

* [gatsby-starter-bootstrap-netlify](https://github.com/konsumer/gatsby-starter-bootstrap-netlify)
  [(demo)](https://gatsby-starter-bootstrap-netlify.netlify.com)

  Features:

  * Very similar to
    [gatsby-starter-netlify-cms](https://github.com/AustinGreen/gatsby-starter-netlify-cms),
    slightly more configurable (eg set site-title in `gatsby-config`) with
    Bootstrap/Bootswatch instead of bulma

* [open-crowd-fund](https://github.com/rwieruch/open-crowd-fund)
  [(demo)](https://www.roadtolearnreact.com/)

  Features:

  * Open source crowdfunding for your own ideas
  * Alternative for Kickstarter, GoFundMe, etc.
  * Secured Credit Card payments with Stripe
  * Storing of funding information in Firebase

* [gatsby-starter-dimension](https://github.com/ChangoMan/gatsby-starter-dimension)
  [(demo)](http://gatsby-dimension.surge.sh/)

  Особенности:

  * Исходя из Dimension site template. Разработано на
    [HTML5 UP](https://html5up.net/dimension)
  * Простой сайт на одну страницу, который идеально подходит для личных портфолио
  * Fully Responsive
  * Styling with SCSS

* [gatsby-starter-docs](https://github.com/ericwindmill/gatsby-starter-docs)
  [(demo)](https://gatsby-docs-starter.netlify.com/)

  Features:

  * Все функции из
    [gatsby-advanced-starter](https://github.com/Vagr9K/gatsby-advanced-starter),
    плюс:
  * Предназначен для Documentation / Tutorial Веб-сайтов
  * 'Table of Contents' Component: Автогенерирует ToC из posts - просто следует файловому frontmatter конвенций от markdown файлов в 'lessons'.
  * Styled Components w/ ThemeProvider
  * Basic UI
  * Несколько дополнительных компонентов
  * собственная prismjs theme
  * React Icons

* [gatsby-starter-personal-blog](https://github.com/greglobinski/gatsby-starter-personal-blog)
  [(demo)](https://gatsby-starter-personal-blog.greglobinski.com/)

  Особенности:

  * Готов к использованию, но легко настраиваемый полностью оборудованный стартер темы
  * Легко редактируемый контент в Markdown файлы (posts, pages и parts)
  * 'Like an app' layout transitions
  * Легко рестайлинг через объект темы
  * Styling с JSS
  * Page transitions
  * Comments (Facebook)
  * Post categories
  * Post list filtering
  * Full text searching (Algolia)
  * Contact form (Netlify form handling)
  * Material UI (@next)
  * RSS feed
  * Full screen mode
  * User adjustable articles' body copy font size
  * Social sharing (Twitter, Facebook, Google, LinkedIn)
  * PWA (manifes.json, offline support, favicons)
  * Google Analytics
  * Favicons generator (node script)
  * Components leazy loading with AsyncComponent (social sharing, info box)
  * ESLint (google config)
  * Prettier code styling
  * Custom webpack CommonsChunkPlugin settings
  * Webpack BundleAnalyzerPlugin
  * [README](https://github.com/greglobinski/gatsby-starter-personal-blog) / [DEMO](https://gatsby-starter-personal-blog.greglobinski.com/)

* [gatsby-starter-deck](https://github.com/fabe/gatsby-starter-deck)
  [(demo)](https://gatsby-deck.netlify.com/)

  Features:

  * Создание презентаций / слайдов с использованием Gatsby.
  * Offline support.
  * Page transitions.

* [gatsby-starter-forty](https://github.com/ChangoMan/gatsby-starter-forty)
  [(demo)](http://gatsby-forty.surge.sh/)

  Features:

  * Основано на Forty site template. Designed by
    [HTML5 UP](https://html5up.net/forty)
  * Colorful homepage, а также включает Landing Page и Generic Page components.
  * Доступны многие элементы, including buttons, forms, tables, и pagination.
  * Styling with SCSS

* [gatsby-firebase-authentication](https://github.com/rwieruch/gatsby-firebase-authentication) [(demo)](https://react-firebase-authentication.wieruch.com/)

  Features:

  * Sign In, Sign Up, Sign Out
  * Password Forget
  * Password Change
  * Protected Routes with Authorization
  * Realtime Database with Users

* [gatsby-starter-ceevee](https://github.com/amandeepmittal/gatsby-starter-ceevee) [(demo)](http://gatsby-starter-ceevee.surge.sh/)

  Features:

  * На основе Ceevee шаблоне сайта, design by [Styleshout](https://www.styleshout.com/)
  * Одностраничный сайт резюме / Портфолио 
  * Target audience Developers, Designers, etc.
  * Используемые CSS Модули, просты в управлении
  * FontAwsome Library for icons
  * Responsive Design, optimized for Mobile devices

- [gatsby-starter-product-guy](https://github.com/amandeepmittal/gatsby-starter-product-guy) [(demo)](http://gatsby-starter-product-guy.surge.sh/)

  Features:

  * Single Page
  * A portfolio Developers and Product launchers alike
  * Using [Typography.js](https://kyleamathews.github.io/typography.js/) easy to switch fonts
  * All your Project/Portfolio Data in Markdown, server by GraphQL
  * Responsive Design, optimized for Mobile devices

* [gatsby-starter-strata](https://github.com/ChangoMan/gatsby-starter-strata)
  [(demo)](http://gatsby-strata.surge.sh/)

  Features:

  * Based off of the Strata site template. Designed by
    [HTML5 UP](https://html5up.net/strata)
  * Super Simple, single page portfolio site
  * Lightbox style React photo gallery
  * Fully Responsive
  * Styling with SCSS

* [verious](https://github.com/cpinnix/verious-boilerplate)
  [(demo)](https://www.verious.io/)

  Особенности:

  * Только компоненты. Bring your own data, plugins, etc.
  * Bootstrap inspired grid system with Container, Row, Column components.
  * Simple Navigation and Dropdown components.
  * Baseline grid built in with modular scale across viewports.
  * Abstract measurements utilize REM for spacing.
  * One font to rule them all: Helvetica.

* [gatsby-starter-lumen](https://github.com/alxshelepenok/gatsby-starter-lumen)
  [(demo)](https://lumen.netlify.com/)

  Особенности:

  * Lost Grid.
  * Красивая типография, вдохновленная [matejlatin/Gutenberg](https://github.com/matejlatin/Gutenberg).
  * [Mobile-First](https://medium.com/@mrmrs_/mobile-first-css-48bc4cc3f60f) approach in development.
  * Стили, построенные с использованием Sass и [BEM](http://getbem.com/naming/)-Style naming.
  * Подсветка синтаксиса в блоках кода.
  * Sidebar menu построенный с использованием блока конфигурации.
  * Archive организованные по тегам и категориям.
  * Automatic RSS generation.
  * Automatic Sitemap generation.
  * Offline support.
  * Google Analytics support.
  * Disqus Comments support.

* [gatsby-starter-strict](https://github.com/kripod/gatsby-starter-strict)
  [(demo)](https://gatsby-starter-strict.netlify.com)

  Особенности:

  * A set of strict linting rules (based on the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript))
    * `lint` script
  * Encourage automatic code formatting
    * `format` script
  * Prefer using [Yarn](https://yarnpkg.com) for package management
  * Use [EditorConfig](http://editorconfig.org) to maintain consistent coding styles between different editors and IDEs
  * Integration with [Visual Studio Code](https://code.visualstudio.com)
    * Pre-configured auto-formatting on file save
  * Based on [gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default)

* [gatsby-hampton-theme](https://github.com/davad/gatsby-hampton-theme)
  [(demo)](http://dmwl.net/gatsby-hampton-theme)

  Features:

  * Eslint in dev mode with the airbnb config and prettier formatting rules
  * [Emotion](https://github.com/emotion-js/emotion) for CSS-in-JS
  * A basic blog, with posts under src/pages/blog
  * A few basic components (Navigation, Layout, Link wrapper around `gatsby-link`))
  * Based on [gatsby-starter-gatsbytheme](https://github.com/saschajullmann/gatsby-starter-gatsbythemes)

* [gatsby-wordpress-starter](https://github.com/ericwindmill/gatsby-starter-wordpress)
  [(demo)](https://gatsby-wordpress-starter.netlify.com/)

  Features:

  * All the features from
    [gatsby-advanced-starter](https://github.com/Vagr9K/gatsby-advanced-starter),
    plus:
  * Leverages the [WordPress plugin for Gatsby](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-source-wordpress) for data
  * Configured to work with WordPress Advanced Custom Fields
  * Auto generated Navigation for your Wordpress Pages
  * Minimal UI and Styling -- made to customize.
  * Styled Components

* [gatsby-starter-simple-landing](https://github.com/greglobinski/gatsby-starter-simple-landing)
  [(demo)](https://gssl.greglobinski.com/)

  Features:

  * CSS-in-JS via [JSS](https://github.com/cssinjs/jss)
  * легко рестайлинг через объект темы
  * текстовое содержимое через Markdown файлы
  * автоматически создаваемые размеры и типы (png, webp) для фона и hero изображений
  * favicons generator
  * webfonts with [webfontloader](https://github.com/typekit/webfontloader)

* [gatsby-starter-typescript-plus](https://github.com/resir014/gatsby-starter-typescript-plus)
  [(demo)](https://gatsby-starter-typescript-plus.netlify.com/)

  Features:

  * TypeScript
  * TSLint (with custom TSLint rules)
  * Markdown рендеринг с Remark
  * Basic component structure
  * Styling with [styled-components](https://www.styled-components.com/)

* [gatsby-orga](https://github.com/xiaoxinghu/gatsby-orga)
  [(demo)](https://xiaoxinghu.github.io/gatsby-orga/)

  Features:

  * Parses [org-mode](http://orgmode.org) files with [Orga](https://github.com/xiaoxinghu/orgajs).

* [gatsby-starter-minimal-blog](https://github.com/LeKoArts/gatsby-starter-minimal-blog)
  [(demo)](https://minimal-blog.netlify.com/)

  Features:

  * Minimal and clean white layout
  * Offline Support, WebApp Manifest, SEO
  * Automatic Favicons
  * Typography.js
  * Part of a german tutorial series on Gatsby. The starter will change over time to use more advanced stuff (feel free to express your ideas)

* [gatsby-starter-redux](https://github.com/caki0915/gatsby-starter-redux)
  [(demo)](https://caki0915.github.io/gatsby-starter-redux/)

  Features:

  * [Redux](https://github.com/reactjs/redux) and [Redux-devtools](https://github.com/gaearon/redux-devtools).
  * [Emotion](https://github.com/emotion-js/emotion) with a basic theme and SSR
  * [Typography.js](https://kyleamathews.github.io/typography.js/)
  * Eslint rules based on [Prettier](https://prettier.io/) and [Airbnb](https://www.npmjs.com/package/eslint-config-airbnb)

* [gatsby-contentful-starter](https://github.com/contentful-userland/gatsby-contentful-starter) [(demo)](https://contentful-userland.github.io/gatsby-contentful-starter/)

  Features:

  * Based on the [Gatsby Starter Blog](https://github.com/gatsbyjs/gatsby-starter-blog)
  * [Includes Contentful Delivery API for production build](https://www.contentful.com/developers/docs/references/content-delivery-api/)
  * [Includes Contentful Preview API for development](https://www.contentful.com/developers/docs/references/content-preview-api/)

* [gatsby-starter-gcn](https://github.com/ryanwiemer/gatsby-starter-gcn) [(demo)](https://gcn.netlify.com/)

  Features:

  * Inspired by [gatsby-contentful-starter](https://github.com/contentful-userland/gatsby-contentful-starter)
  * Contentful integration with ready to go placeholder content
  * Netlify integration including a pre-built contact form
  * Minimal responsive design - made to customize or tear apart
  * Styled components
  * SEO friendly

* [gatsby-starter-timeline-theme](https://github.com/amandeepmittal/gatsby-portfolio-v3) [(Demo)](http://portfolio-v3.surge.sh/)

  Features:

  * Single Page, Timeline View
  * A portfolio Developers and Product launchers
  * Bring in Data, plug-n-play
  * Responsive Design, optimized for Mobile devices
  * Seo Friendly
  * Uses Flexbox

* [gatsby-starter-stellar](https://github.com/codebushi/gatsby-starter-stellar)
  [(demo)](http://gatsby-stellar.surge.sh/)

  Features:

  * Основан на шаблоне сйата Stellar. Designed by
    [HTML5 UP](https://html5up.net/stellar)
  * Дружелюбный к прокрутке, отзывчивый сайт. Может использоваться как одностраничный или многостраничный сайт.
  * Sticky Navigation when scrolling.
  * Scroll spy и smooth scrolling to different sections of the page.
  * Styling with SCSS

* [gatsby-starter-tailwind](https://github.com/taylorbryant/gatsby-starter-tailwind)
  [(demo)](https://quizzical-mcclintock-0226ac.netlify.com/)

  Features:

  * Based on [gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default)
  * [Tailwind CSS](https://tailwindcss.com) Framework
  * Removes unused CSS with [Purgecss](https://www.purgecss.com/)
  * Includes responsive navigation and form examples

* [gatsby-starter-bloomer](https://github.com/Cethy/gatsby-starter-bloomer)
  [(demo)](https://gatsby-starter-bloomer.netlify.com/)

  Features:

  * Based on [gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default)
  * [Bulma CSS Framework](https://bulma.io/) with its [Bloomer react components](https://bloomer.js.org/)
  * [Font-Awesome icons](https://fontawesome.com/icons)
  * Includes a simple fullscreen hero w/ footer example

* [gatsby-starter-i18n-lingui](https://github.com/dcroitoru/gatsby-starter-i18n-lingui)
  [(demo)](https://gatsby-starter-i18n-lingui.netlify.com/)

  Features:

  * Localization (Multilanguage) provided by [js-lingui](https://github.com/lingui/js-lingui)
  * Message extraction
  * Avoids code duplication - generates pages for each locale
  * Possibility of translated paths

* [gatsby-starter-photon](https://github.com/codebushi/gatsby-starter-photon)
  [(demo)](http://gatsby-photon.surge.sh/)

  Особенности:

  * Основан на шаблоне сайта Photon. Designed by
    [HTML5 UP](https://html5up.net/photon)
  * Single Page, Responsive Site
  * Custom grid made with CSS Grid
  * Styling with SCSS

* [gatsby-starter-business](https://github.com/v4iv/gatsby-starter-business)
  [(demo)](https://gatsby-starter-business.netlify.com/)

  Особенности:

  * Полный пакет бизнес-сайтов - Home Page, About Page, Pricing Page, Contact Page и Blog
  * Netlify CMS для Content Management
  * SEO Friendly (Sitemap, Schemas, Meta Tags, GTM etc)
  * Bulma а также Sass Support для стилизации
  * Прогрессивное веб-приложение и автономная поддержка
  * Tags and RSS Feed for Blog
  * Disqus and Share Support

* [gatsby-advanced-blog](https://github.com/wonism/gatsby-advanced-blog)
  [(demo)](https://kind-cori-836fe1.netlify.com/)

  Особенности:

  * Список сообщений в блоге с превью (image + summary) для каждого сообщения в блоге
  * Категории и теги для сообщений в блогах с разбивкой на страницы
  * Поиск сообщения с ключевым словом
  * Put react application / tweet into post
  * Скопируйте несколько кодов в сообщении с помощью кнопки
  * портфолио
  * Resume
  * Redux для управления состоянием (с redux-saga / reselect)

* [gatsby-starter-procyon](https://github.com/danielmahon/gatsby-starter-procyon)
  [(demo)](https://gatsby-starter-procyon.netlify.com/)

  Features:
  
  * [Gatsby](https://www.gatsbyjs.org/) + [ReactJS](https://reactjs.org/) (server side rendering)
  * [GraphCMS](https://graphcms.com/) Headless CMS
  * [DraftJS](https://draftjs.org/) (in-place) [Medium](https://medium.com)-like Editing
  * [Apollo GraphQL](https://www.apollographql.com/) (client-side)
  * Local caching between builds
  * [Material-UI](https://material-ui-next.com/) (layout, typography, components, etc)
  * Styled-Components™-like API via Material-UI
  * [Netlify](https://www.netlify.com/) Deployment Friendly
  * [Netlify Identity](https://www.netlify.com/docs/identity/) Authentication (enables editing)
  * Automatic versioning, deployment and CHANGELOG
  * Automatic rebuilds with GraphCMS and Netlify web hooks
  * PWA (Progressive Web App)
  * [Google Fonts](https://fonts.google.com/)
  
* [gatsby-starter-2column-portfolio](https://github.com/praagyajoshi/gatsby-starter-2column-portfolio)
  [(demo)](http://2column-portfolio.surge.sh/)

  Features:

  * Designed as a minimalistic portfolio website
  * Grid system using flexboxgrid
  * Styled using SCSS
  * Font icons using font-awesome
  * Google Analytics integration
  * Open Sans font using Google Fonts
  * Prerendered Open Graph tags for rich sharing
