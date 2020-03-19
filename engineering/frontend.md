# Best practices in Frontend development

## React

We use React for most projects, so this guide mostly leans toward best practices for React and libraries that use (or work well with) React.

## Project structure

There are tons of opinion around project structure, each with it's own pros and cons. We currently use our own [template](https://github.com/dwarvesf/template-react-app/tree/master/template) for most (Web application) projects. It also opinionated and made a handful of choices that worked well for us:

- Global state management: [Redux Bundler](https://reduxbundler.com/)
- Client-side routing: [Redux Bundler's route bundle](https://reduxbundler.com/api/included-bundles.html#createurlbundleoptionsobject), since Redux-bundler already manage our global state, leveraging it to handle routing states is obvious choice
- I18n: [react-localize-redux](https://github.com/ryandrewjohnson/react-localize-redux) provides simple solution for translations in React, it also support lazy-loading translation files which is a huge win, in contrast other alternatives like [react-intl](https://github.com/yahoo/react-intl) requires you to provide **all** translation files on initialize. Check [here](https://github.com/dwarvesf/template-react-app/blob/master/template/src/bundles/localize.js) to see how easy it is to integrate react-localize-redux with Redux-bundler
- Styling: [TailwindCSS](https://tailwindcss.com/docs/what-is-tailwind/) - a ultility-based CSS library to make writing CSS in React fast and enjoyable. Combine with [Linaria](https://github.com/callstack/linaria) we can easily write complex inline styles that later compiled to raw CSS at build time. We also plugged in [PurgeCSS](https://github.com/dwarvesf/template-react-app/blob/master/template/purgecss.config.js) to remove unused CSS ultility classes from Tailwind in production build
- [Pre-configured ESLint](https://github.com/dwarvesf/template-react-app/blob/master/template/.eslintrc.js) with Prettier integration

## States handling

With the introduction of [React Hooks](https://reactjs.org/docs/hooks-intro.html), function components can now handle their own states and effects (previously known as "life-cycle") without using Class component. Our team has been early adopter of React Hooks since alpha and having good success, thus we recommend using React Hooks for local states and effects handling, this [awesome article](https://overreacted.io/how-are-function-components-different-from-classes/) by Dan Abramov explains some pitfalls and mis-conceptions of developers while using Class components and how React Hooks make them obvious to avoid by default.

## Coding style

We currenty don't have a strict rule for writing Javascript as it would limit developer's freedom. However we do have some small practices has been established internally:

- Never use `var`
- Prefer `forEach`/`for/of` over `for (let i = 0; i < items.length; i++)`
- Prefer functional & immutable array methods .ie `map`/`filter`/`reduce`/`some`/`every` over any types of mutable `for` loop
- Prefer "return early" coding style, more about it [here](https://medium.com/@matryer/line-of-sight-in-code-186dd7cdea88)
- Don't obsess over performance of code, obsess over making it clear

## Writing custom styles

TailwindCSS covers most things, but if you ever feel the need of writing custom styles for a single floating icon or a fancy hero banner transition, use [CSS Modules](https://github.com/css-modules/css-modules).

## Continous integration & deployment

Use [Netlify](https://www.netlify.com/) for web applications and static websites, it handles both CI and deployment for free, which is awesome.

## Monitoring

Use [Sentry](https://sentry.io/) to monitor errors and centralized stacktrace in production.
