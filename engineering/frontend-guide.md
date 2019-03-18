# Frontend Guide

Learning guide for modern Frontend Development.

## Single-page Apps (SPAs)

Web developers these days refer to the products they build as web apps, rather than websites. While there is no strict difference between the two terms, web apps tend to be highly interactive and dynamic, allowing the user to perform actions and receive a response for their action. Traditionally, the browser receives HTML from the server and renders it. When the user navigates to another URL, a full-page refresh is required and the server sends fresh new HTML for the new page. This is called server-side rendering.

However in modern SPAs, client-side rendering is used instead. The browser loads the initial page from the server, along with the scripts (frameworks, libraries, app code) and stylesheets required for the whole app. When the user navigates to other pages, a page refresh is not triggered. The URL of the page is updated via the [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API). New data required for the new page, usually in JSON format, is retrieved by the browser via [AJAX](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started) requests to the server. The SPA then dynamically updates the page with the data via JavaScript, which it has already downloaded in the initial page load. This model is similar to how native mobile apps work.

The benefits:

- The app feels more responsive and users do not see the flash between page navigations due to full-page refreshes.
- Fewer HTTP requests are made to the server, as the same assets do not have to be downloaded again for each page load.
- Clear separation of the concerns between the client and the server; you can easily build new clients for different platforms (e.g. mobile, chatbots, smart watches) without having to modify the server code. You can also modify the technology stack on the client and server independently, as long as the API contract is not broken.

The downsides:

- Heavier initial page load due to loading of framework, app code, and assets required for multiple pages.<sup><a href="#fn1" id="ref1">1</a></sup>
- There's an additional step to be done on your server which is to configure it to route all requests to a single entry point and allow client-side routing to take over from there.
- SPAs are reliant on JavaScript to render content, but not all search engines execute JavaScript during crawling, and they may see empty content on your page. This inadvertently hurts the Search Engine Optimization (SEO) of your app. <sup><a href="#fn2" id="ref2">2</a></sup>. However, most of the time, when you are building apps, SEO is not the most important factor, as not all the content needs to be indexable by search engines. To overcome this, you can either server-side render your app or use services such as [Prerender](https://prerender.io/) to "render your javascript in a browser, save the static HTML, and return that to the crawlers".

While traditional server-side rendered apps are still a viable option, a clear client-server separation scales better for larger engineering teams, as the client and server code can be developed and released independently.

As web developers are now building apps rather than pages, organization of client-side JavaScript has become increasingly important. In server-side rendered pages, it is common to use snippets of jQuery to add user interactivity to each page. However, when building large apps, just jQuery is insufficient. After all, jQuery is primarily a library for DOM manipulation and it's not a framework; it does not define a clear structure and organization for your app.

JavaScript frameworks have been created to provide higher-level abstractions over the DOM, allowing you to keep state in memory, out of the DOM. Using frameworks also brings the benefits of reusing recommended concepts and best practices for building apps. A new engineer on the team who is unfamiliar with the code base, but has experience with a framework, will find it easier to understand the code because it is organized in a structure that they are familiar with. Popular frameworks have a lot of tutorials and guides, and tapping on the knowledge and experience from colleagues and the community will help new engineers get up to speed.

#### Study Links

- [Single Page App: advantages and disadvantages](http://stackoverflow.com/questions/21862054/single-page-app-advantages-and-disadvantages)
- [The (R)Evolution of Web Development](http://blog.isquaredsoftware.com/presentations/2016-10-revolution-of-web-dev/)
- [Here's Why Client Side Rendering Won](https://medium.freecodecamp.com/heres-why-client-side-rendering-won-46a349fadb52)

## New-age JavaScript

Before you dive into the various aspects of building a JavaScript web app, it is important to get familiar with the language of the web - JavaScript, or ECMAScript. JavaScript is an incredibly versatile language which you can also use to build [web servers](https://nodejs.org/en/), [native mobile apps](https://facebook.github.io/react-native/) and [desktop apps](https://electron.atom.io/).

Prior to 2015, the last major update was ECMAScript 5.1, in 2011. However, in the recent years, JavaScript has suddenly seen a huge burst of improvements within a short span of time. In 2015, ECMAScript 2015 (previously called ECMAScript 6) was released and a ton of syntactic constructs were introduced to make writing code less unwieldy. If you are curious about it, Auth0 has written a nice article on the [history of JavaScript](https://auth0.com/blog/a-brief-history-of-javascript/). Till this day, not all browsers have fully implemented the ES2015 specification. Tools such as [Babel](https://babeljs.io/) enable developers to write ES2015 in their apps and Babel transpiles them down to ES5 to be compatible for browsers.

Spend a day or two revising ES5 and exploring ES2015. The more heavily used features in ES2015 include "Arrows and Lexical This", "Classes", "Template Strings", "Destructuring", "Default/Rest/Spread operators", and "Importing and Exporting modules".

**Estimated Duration: 3-4 days.** You can learn/lookup the syntax as you learn the other libraries and try building your own app.

#### Study Links

- [Learn ES5 on Codecademy](https://www.codecademy.com/learn/learn-javascript)
- [Learn ES6 on Codecademy](https://www.codecademy.com/learn/introduction-to-javascript)
- [Learn ES2015 on Babel](https://babeljs.io/learn-es2015/)
- [ES6 Katas](http://es6katas.org/)
- [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) (Advanced content, optional for beginners)
- [Answers to Front End Job Interview Questions — JavaScript](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

## User Interface - React

<img alt="React Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/react-logo.svg" width="256px" />

If any JavaScript project has taken the front end ecosystem by storm in recent years, that would be [React](https://facebook.github.io/react/). React is a library built and open-sourced by the smart people at Facebook. In React, developers write components for their web interface and compose them together.

React brings about many radical ideas and encourages developers to [rethink best practices](https://www.youtube.com/watch?v=DgVS-zXgMTk). For many years, web developers were taught that it was a good practice to write HTML, JavaScript and CSS separately. React does the exact opposite, and encourages that you write your HTML and [CSS in your JavaScript](https://speakerdeck.com/vjeux/react-css-in-js) instead. This sounds like a crazy idea at first, but after trying it out, it actually isn't as weird as it sounds initially. Reason being the front end development scene is shifting towards a paradigm of component-based development. The features of React:

- **Declarative** - You describe what you want to see in your view and not how to achieve it. In the jQuery days, developers would have to come up with a series of steps to manipulate the DOM to get from one app state to the next. In React, you simply change the state within the component and the view will update itself according to the state. It is also easy to determine how the component will look like just by looking at the markup in the `render()` method.

- **Functional** - The view is a pure function of `props` and `state`. In most cases, a React component is defined by `props` (external parameters) and `state` (internal data). For the same `props` and `state`, the same view is produced. Pure functions are easy to test, and the same goes for functional components. Testing in React is made easy because a component's interfaces are well-defined and you can test the component by supplying different `props` and `state` to it and comparing the rendered output.

- **Maintainable** - Writing your view in a component-based fashion encourages reusability. We find that defining a component's `propTypes` make React code self-documenting as the reader can know clearly what is needed to use that component. Lastly, your view and logic is self-contained within the component, and should not be affected nor affect other components. That makes it easy to shift components around during large-scale refactoring, as long as the same `props` are supplied to the component.

- **High Performance** - You might have heard that React uses a virtual DOM (not to be confused with [shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Shadow_DOM)) and it re-renders everything when there is a change in state. Why is there a need for a virtual DOM? While modern JavaScript engines are fast, reading from and writing to the DOM is slow. React keeps a lightweight virtual representation of the DOM in memory. Re-rendering everything is a misleading term. In React it actually refers to re-rendering the in-memory representation of the DOM, not the actual DOM itself. When there's a change in the underlying data of the component, a new virtual representation is created, and compared against the previous representation. The difference (minimal set of changes required) is then patched to the real browser DOM.

- **Ease of Learning** - Learning React is pretty simple. The React API surface is relatively small compared to [this](https://angular.io/docs/ts/latest/api/); there are only a few APIs to learn and they do not change often. The React community is one of the largest, and along with that comes a vibrant ecosystem of tools, open-sourced UI components, and a ton of great resources online to get you started on learning React.

- **Developer Experience** - There are a number of tools that improves the development experience with React. [React Developer Tools](https://github.com/facebook/react-devtools) is a browser extension that allows you to inspect your component, view and manipulate its `props` and `state`. [Hot reloading](https://github.com/gaearon/react-hot-loader) with webpack allows you to view changes to your code in your browser, without you having to refresh the browser. Front end development involves a lot of tweaking code, saving and then refreshing the browser. Hot reloading helps you by eliminating the last step. When there are library updates, Facebook provides [codemod scripts](https://github.com/reactjs/react-codemod) to help you migrate your code to the new APIs. This makes the upgrading process relatively pain-free. Kudos to the Facebook team for their dedication in making the development experience with React great.

Over the years, new view libraries that are even more performant than React have emerged. React may not be the fastest library out there, but in terms of the ecosystem, overall usage experience and benefits, it is still one of the greatest. Facebook is also channeling efforts into making React even faster with a [rewrite of the underlying reconciliation algorithm](https://github.com/acdlite/react-fiber-architecture). The concepts that React introduced has taught us how to write better code, more maintainable web apps and made us better engineers. We like that.

We recommend going through the [tutorial](https://reactjs.org/tutorial/tutorial.html) on building a tic-tac-toe game on the React homepage to get a feel of what React is and what it does. For more in-depth learning, check out the Egghead course, [Build Your First Production Quality React App](https://egghead.io/courses/build-your-first-production-quality-react-app). It covers some advanced concepts and real-world usages that are not covered by the React documentation. [Create React App](https://github.com/facebookincubator/create-react-app) by Facebook is a tool to scaffold a React project with minimal configuration and is highly recommended to use for starting new React projects.

React is a library, not a framework, and does not deal with the layers below the view - the app state. More on that later.

**Estimated Duration: 3-4 days.** Try building simple projects like a to-do list, Hacker News clone with pure React. You will slowly gain an appreciation for it and perhaps face some problems along the way that isn't solved by React, which brings us to the next topic...

#### Study Links

- [React Official Tutorial](https://facebook.github.io/react/tutorial/tutorial.html)
- [Overreacted.io](https://overreacted.io/)
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.5iexphyg5)

## State Management - Flux/Redux

<img alt="Redux Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/redux-logo.svg" width="256px" />

As your app grows bigger, you may find that the app structure becomes a little messy. Components throughout the app may have to share and display common data but there is no elegant way to handle that in React. After all, React is just the view layer, it does not dictate how you structure the other layers of your app, such as the model and the controller, in traditional MVC paradigms. In an effort to solve this, Facebook invented Flux, an app architecture that complements React's composable view components by utilizing a unidirectional data flow. Read more about how Flux works [here](https://facebook.github.io/flux/docs/in-depth-overview.html). In summary, the Flux pattern has the following characteristics:

- **Unidirectional data flow** - Makes the app more predictable as updates can be tracked easily.
- **Separation of concerns** - Each part in the Flux architecture has clear responsibilities and are highly decoupled.
- **Works well with declarative programming** - The store can send updates to the view without specifying how to transition views between states.

As Flux is not a framework per se, developers have tried to come up with many implementations of the Flux pattern. Eventually, a clear winner emerged, which was [Redux](http://redux.js.org/). Redux combines the ideas from Flux, [Command pattern](https://www.wikiwand.com/en/Command_pattern) and [Elm architecture](https://guide.elm-lang.org/architecture/) and is the de facto state management library developers use with React these days. Its core concepts are:

- App **state** is described by a single plain old JavaScript object (POJO).
- Dispatch an **action** (also a POJO) to modify the state.
- **Reducer** is a pure function that takes in current state and action to produce a new state.

The concepts sound simple, but they are really powerful as they enable apps to:

- Have their state rendered on the server, booted up on the client.
- Trace, log and backtrack changes in the whole app.
- Implement undo/redo functionality easily.

The creator of Redux, [Dan Abramov](https://github.com/gaearon), has taken great care in writing up detailed documentation for Redux, along with creating comprehensive video tutorials for learning [basic](https://egghead.io/courses/getting-started-with-redux) and [advanced](https://egghead.io/courses/building-react-applications-with-idiomatic-redux) Redux. They are extremely helpful resources for learning Redux.

**Combining View and State**

While Redux does not necessarily have to be used with React, it is highly recommended as they play very well with each other. React and Redux have a lot of ideas and traits in common:

- **Functional composition paradigm** - React composes views (pure functions) while Redux composes pure reducers (also pure functions). Output is predictable given the same set of input.
- **Easy To Reason About** - You may have heard this term many times but what does it actually mean? We interpret it as having control and understanding over our code - Our code behaves in ways we expect it to, and when there are problems, we can find them easily. Through our experience, React and Redux makes debugging simpler. As the data flow is unidirectional, tracing the flow of data (server responses, user input events) is easier and it is straightforward to determine which layer the problem occurs in.
- **Layered Structure** - Each layer in the app / Flux architecture is a pure function, and has clear responsibilities. It is relatively easy to write tests for pure functions. You have to centralize changes to your app within the reducer, and the only way to trigger a change is to dispatch an action.
- **Development Experience** - A lot of effort has gone into creating tools to help in debugging and inspecting the app while development, such as [Redux DevTools](https://github.com/gaearon/redux-devtools).

Your app will likely have to deal with async calls like making remote API requests. [redux-thunk](https://github.com/gaearon/redux-thunk) were created to solve those problems. Our advice is to deal with it only when you need it.

[react-redux](https://github.com/reactjs/react-redux) is an official React binding for Redux and is very simple to learn.

**Estimated Duration: 4 days.** The egghead courses can be a little time-consuming but they are worth spending time on. After learning Redux, you can try incorporating it into the React projects you have built. Does Redux solve some of the state management issues you were struggling with in pure React?

#### Study Links

- [Flux Homepage](http://facebook.github.io/flux)
- [Redux Homepage](http://redux.js.org/)
- [Egghead Course - Getting Started with Redux](https://egghead.io/courses/getting-started-with-redux)
- [Egghead Course - Build React Apps with Idiomatic Redux](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)
- [React Redux Links](https://github.com/markerikson/react-redux-links)
- [You Might Not Need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)

#### Alternatives

- [MobX](https://github.com/mobxjs/mobx)
- [Redux Bundler](https://reduxbundler.com/)

## Coding with Style - TailwindCSS + CSS Modules

<img alt="TailwindCSS Logo" src="https://raw.githubusercontent.com/github/explore/882462b8ecc337fd9c9b2572bc463a1cbc88fb6a/topics/tailwind/tailwind.png" width="256px" height="256px" />
<img alt="CSS Modules Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/css-modules-logo.svg" width="256px" height="256px" />

CSS (Cascading Style Sheets) are rules to describe how your HTML elements look. Writing good CSS is hard. It usually takes many years of experience and frustration of shooting yourself in the foot before one is able to write maintainable and scalable CSS. CSS, having a global namespace, is fundamentally designed for web documents, and not really for web apps that favor a components architecture. Hence, experienced front end developers have designed methodologies to guide people on how to write organized CSS for complex projects, such as using [SMACSS](https://smacss.com/), [BEM](http://getbem.com/), [SUIT CSS](http://suitcss.github.io/), etc.

However, the encapsulation of styles that these methodologies bring about are artificially enforced by conventions and guidelines. They break the moment developers do not follow them.

As you might have realized by now, the front end ecosystem is saturated with tools, and unsurprisingly, tools have been invented to [partially solve some of the problems](https://speakerdeck.com/vjeux/react-css-in-js) with writing CSS at scale. "At scale" means that many developers are working on the same large project and touching the same stylesheets. There is no community-agreed approach on writing [CSS in JS](https://github.com/MicheleBertoli/css-in-js) at the moment, and we are hoping that one day a winner would emerge, just like Redux did, among all the Flux implementations.

For now, we are banking on [TailwindCSS](https://tailwindcss.com/docs/what-is-tailwind/) and [CSS Modules](https://github.com/css-modules/css-modules). Tailwind is a utility-first aproach to writing CSS, it provides highly composable, low-level utility classes that make it easy to build complex user interfaces. CSS modules is an improvement over existing CSS that aims to fix the problem of global namespace in CSS; it enables you to write styles that are local by default and encapsulated to your component. This feature is achieved via tooling thus it's important to remember CSS modules are still being compiled into normal globally-namespaced CSS that browsers recognize, and it is still important to learn and understand how raw CSS works.

If you are a total beginner to CSS, Codecademy's [HTML & CSS course](https://www.codecademy.com/learn/learn-html-css) will be a good introduction to you. Next, read up on the [Sass preprocessor](http://sass-lang.com/), an extension of the CSS language which adds syntactic improvements and encourages style reusability.

**Estimated Duration: 3-4 days.** Try styling up your app using the SMACSS/BEM approach and/or CSS modules. The key is to understand *why* we need these techniques and what problem they solve, then dig deeper to examine how each tool/technique solved the problems. Lastly, learn Tailwind and you will see how it speeds up your CSS workflow while solving CSS class-naming problem at the same time.

#### Study Links

- [Learn HTML & CSS course on Codecademy](https://www.codecademy.com/learn/learn-html-css)
- [Intro to HTML/CSS on Khan Academy](https://www.khanacademy.org/computing/computer-programming/html-css)
- [Sass Homepage](http://sass-lang.com/)
- [CSS Modules Specification](https://github.com/css-modules/css-modules)
- [TailwindCSS Homepage](https://tailwindcss.com/docs/what-is-tailwind/)
- [Answers to Front End Job Interview Questions — HTML](https://github.com/yangshun/tech-interview-handbook/blob/master/front-end/interview-questions.md#html-questions)
- [Answers to Front End Job Interview Questions — CSS](https://github.com/yangshun/tech-interview-handbook/blob/master/front-end/interview-questions.md#css-questions)

#### Alternatives

- [Emotion](https://github.com/emotion-js/emotion)
- [Styled Components](https://github.com/styled-components/styled-components)

## Testing - Jest + react-testing-library

TBD.

#### Study Links

- [Jest Homepage](http://facebook.github.io/jest/)
- [Testing React Applications with Jest](https://auth0.com/blog/testing-react-applications-with-jest/)
- [react-testing-library](https://github.com/kentcdodds/react-testing-library)

## Linting and Formatting JavaScript - ESLint + Prettier

<img alt="ESLint Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/eslint-logo.svg" width="256px" height="256px" />
<img alt="Prettier Logo" src="https://prettier.io/icon.png" width="256px" height="256px" />
<br/>

A linter is a tool to statically analyze code and finds problems with them, potentially preventing bugs/runtime errors and at the same time, enforcing a coding style. Time is saved during pull request reviews when reviewers do not have to leave nitpicky comments on coding style. [ESLint](http://eslint.org/) is a tool for linting JavaScript code that is highly extensible and customizable. Teams can write their own lint rules to enforce their custom styles.

We cooked ourselves a [customized](https://github.com/dwarvesf/template-react-app/blob/master/template/.eslintrc.js) ESlint configuration, which uses [Prettier](https://prettier.io/) and Airbnb's [`eslint-config-airbnb`](https://www.npmjs.com/package/eslint-config-airbnb) preset under the hood, that has already been configured with the common good coding style in the [Airbnb JavaScript style guide](https://github.com/airbnb/javascript).

For the most part, using ESLint is as simple as tweaking a configuration file in your project folder. There's nothing much to learn about ESLint if you're not writing new rules for it. Just be aware of the errors when they surface and Google it to find out the recommended style.

**Estimated Duration: 1/2 day.** Nothing much to learn here. Add ESLint to your project and fix the linting errors!

#### Study Links

- [ESLint Homepage](http://eslint.org/)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Prettier](https://prettier.io/)
- [Standard](https://github.com/standard/standard)

## Build System - Webpack

<img alt="webpack Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/webpack-logo.svg" width="256px" />

This part will be kept short as setting up webpack can be a tedious process and might be a turn-off to developers who are already overwhelmed by the barrage of new things they have to learn for front end development. In a nutshell, [webpack](https://webpack.js.org/) is a module bundler that compiles a front end project and its dependencies into a final bundle to be served to users. Usually, projects will already have the webpack configuration set up and developers rarely have to change it. Having an understanding of webpack is still a good to have in the long run. It is due to webpack that features like hot reloading and CSS modules are made possible.

We have found the [webpack walkthrough](https://survivejs.com/webpack/foreword/) by SurviveJS to be the best resource on learning webpack. It is a good complement to the official documentation and we recommend following the walkthrough first and referring to the documentation later when the need for further customization arises.

**Estimated Duration: 2 days (Optional).**

#### Study Links

- [Webpack Homepage](https://webpack.js.org/)
- [SurviveJS - Webpack: From apprentice to master](https://survivejs.com/webpack/foreword/)

#### Alternatives

- [Rollup](https://rollupjs.org/)
- [Parcel](https://parceljs.org/)

## Package Management - Yarn

<img alt="Yarn Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/yarn-logo.png" width="256px" />

If you take a peek into your `node_modules` directory, you will be appalled by the number of directories that are contained in it. Each babel plugin, lodash function, is a package on its own. When you have multiple projects, these packages are duplicated across each project and they are largely similar. Each time you run `npm install` in a new project, these packages are downloaded over and over again even though they already exist in some other project in your computer.

There was also the problem of non-determinism in the installed packages via `npm install`. Some of our CI builds fail because at the point of time when the CI server installs the dependencies, it pulled in minor updates to some packages that contained breaking changes. This would not have happened if library authors respected [semver](http://semver.org/) and engineers did not assume that API contracts would be respected all the time.

[Yarn](https://yarnpkg.com/) solves these problems. The issue of non-determinism of installed packages is handled via a `yarn.lock` file, which ensures that every install results in the exact same file structure in `node_modules` across all machines. Yarn utilizes a global cache directory within your machine, and packages that have been downloaded before do not have to be downloaded again. This also enables offline installation of dependencies!

The most common Yarn commands can be found [here](https://yarnpkg.com/en/docs/usage). Most other yarn commands are similar to the `npm` equivalents and it is fine to use the `npm` versions instead. One of our favorite commands is `yarn upgrade-interactive` which makes updating dependencies a breeze especially when the modern JavaScript project requires so many dependencies these days. Do check it out!

npm@5.0.0 was [released in May 2017](https://github.com/npm/npm/releases/tag/v5.0.0) and it seems to address many of the issues that Yarn aims to solve. Do keep an eye on it!

**Estimated Duration: 2 hours.**

#### Study Links

- [Yarn Homepage](https://yarnpkg.com/)
- [Yarn: A new package manager for JavaScript](https://code.facebook.com/posts/1840075619545360)

#### Alternatives

- [Good old npm](https://github.com/npm/npm/releases/tag/v5.0.0)

## Continuous Integration + Hosting

We use [Netlify](https://www.netlify.com/) for our continuous integration (CI) pipeline. Netlify provides a single, simplified workflow to deploy web projects.

We configured Netlify to do the following when changes are pushed to Git repo:

- Run lint + unit tests for the project
- Build the source code
- Deploy the source code to Netlify's domain
- Post a notification to Slack to inform about build result and a URL to the deployed branch/PR

#### Study Links

- [Neltify Homepage](https://www.netlify.com/features/)

#### Alternatives

- [GitLab CI/CD](https://about.gitlab.com/product/continuous-integration/)
- [CircleCI](https://circleci.com/)
- [TravisCI](https://travis-ci.com/)

## Monitoring

After shipping the product, you would also want to monitor it for any errors.

Apart from network level monitoring from our CDN service provider and hosting provider, we use [Sentry](https://sentry.io/) to monitor errors that originates from the app logic.

#### Study Links

- [Sentry Homepage](https://sentry.io/)

### The Journey has Just Begun

Congratulations on making it this far! Front end development today is [hard](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f), but it is also more interesting than before. There are many more things to be learnt, but building up a solid foundation in the essentials will aid in learning the rest of the technologies. This helpful [front end web developer roadmap](https://github.com/kamranahmedse/developer-roadmap#-front-end-roadmap) shows the alternative technologies available for each aspect.

### More Reading

**General**

- [State of the JavaScript Landscape: A Map for Newcomers](http://www.infoq.com/articles/state-of-javascript-2016)
- [The Hitchhiker's guide to the modern front end development workflow](http://marcobotto.com/the-hitchhikers-guide-to-the-modern-front-end-development-workflow/)
- [How it feels to learn JavaScript in 2016](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f#.tmy8gzgvp)
- [Roadmap to becoming a web developer in 2017](https://github.com/kamranahmedse/developer-roadmap#-frontend-roadmap)
- [Modern JavaScript for Ancient Web Developers](https://trackchanges.postlight.com/modern-javascript-for-ancient-web-developers-58e7cae050f9)

**Other Study Plans**

- [A Study Plan To Cure JavaScript Fatigue](https://medium.freecodecamp.com/a-study-plan-to-cure-javascript-fatigue-8ad3a54f2eb1#.c0wnrrcwd)
- [JS Stack from Scratch](https://github.com/verekia/js-stack-from-scratch)
- [A Beginner’s JavaScript Study Plan](https://medium.freecodecamp.com/a-beginners-javascript-study-plan-27f1d698ea5e#.bgf49xno2)

### Footnotes

<p id="fn1">1. This can be solved via <a href="https://webpack.js.org/guides/code-splitting/">Webpack code splitting</a>. <a href="#ref1" title="Jump back to footnote 1 in the text.">↩</a></p>

<p id="fn2">2. <a href="https://medium.com/@mjackson/universal-javascript-4761051b7ae9">Universal JS</a> to the rescue! <a href="#ref2" title="Jump back to footnote 1 in the text.">↩</a></p>

### Acknowledgements

Thanks to [Grab's Frontend Guide](https://github.com/grab/front-end-guide). This document is a forked version based on their guideline.
