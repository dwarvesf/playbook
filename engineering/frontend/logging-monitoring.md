# Logging and Monitoring

This document in intended to let you know some metrics we need to care when logging and monitoring on frontend application

## Errors tracing

Frontend application has to received a lot of user interactions and unexpected issues can come many and fast.

To help developer can catch and resolve these errors easily we need a tool can:

- [x] Integrate to staging/dev server first so we can catch an issue before it become a production bug
- [x] Group same error events so we don't lost in error table and can resolve same errors in once
- [x] Know which errors are impacting to application by happened times and effected users
- [x] Prioritize errors by impact so we can know which error is happening most
- [x] Send email, and notification to responsibility developers ASAP when production issue appear to take quick action
- [x] Link errors to management tools to don't miss these errors
- [x] Make sure issues status are up to date

Base on these criteria, we choose [Sentry](https://sentry.io/) to catch the error on both Frontend and Backend so our dev team can monitor whole system in one.

## Performance

It in intended to let us know how good our application are. Base [Performance Insight](https://developer.chrome.com/docs/devtools/performance-insights/) these metric we need to take care:

1. **Loading time:** main content should load fast enough

2. **Interactivity:** how many delay when user is interacting with our application

3. **Visual stability:** make sure our content visually stable, without unexpected layout shift

### Loading time

If application to compatible with SEO engines we should check it on [PageSpeed Insights](https://pagespeed.web.dev/), which will show all our application metric scores on each page URL and how to improve it so you can improve it metric by metric.

If application has authentication, `PageSpeed Insights` won't be unable to check it. So we have to use [Lighthouse Google Extension](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk) to check same as PageSpeed Insight from Chrome developer tools

To summary all page loading time we should install tracking code to your application like [Google Analytic](https://analytics.google.com/), [Mix Panel](https://mixpanel.com/) that can help generate detail report and we can explore and improve our application easier


### Interactivity

In Lighthouse, we usually fail `Reduce JavaScript execution time` you can find detail on this [document](https://web.dev/bootup-time/) and improve execution time

Beside that, we are using [Webpack Bundle Analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer) to know which bundle files are large to reduce it with code splitting or change 3rd party modules

We are using [https://bundlephobia.com/](https://bundlephobia.com/) to compare or find alias package to reduce bundle size



### Visual stability

This metric will be ready hard to monitor and maintaining because we have a lot of device sizes and browser engines today. To pass this problem, we need to follow responsive standard and use some automation tool to check on multiple devices and send report to us

We recommend to use [Browser Stack](https://www.browserstack.com/) to setup in CI/CD and send report to us after release something new on production

### Event and Data flow tracking

We are using [Google Analytic](https://analytics.google.com/) on our projects as default to log important user interactions. Beside that, we will add custom events and set goals to our Google Analytic base on Marketing team requirements

Base data on analytic tool, we can know which page we need to improve content to decrease bounce rate and fix error links



