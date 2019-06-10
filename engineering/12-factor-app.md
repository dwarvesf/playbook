# The 12-factor App

The 12-factor apps let we write modern software as a service which is easy to deploy, scale up, maximize portability, and minimize time, the cost for new developers joining the project. 

You can read more about the philosophy of the 12-factor app https://12factor.net. Here we only show how we those in real projects.

## Codebase

> One codebase tracked in revision control, many deploys

**A twelve-factor app is always tracked in a version control system.** We use [git](git.md) to track any changes in the code of a repo. 

There is only one codebase per app, but there will be many deploys of the app. We usually have one deploy for the following environment: local, development, staging and production.

The codebase is the same across all deploys, although different versions may be active in each deploy. The most stable release should be deployed on staging and production environment.

## Dependencies

> Explicitly declare and isolate dependencies

**A twelve-factor app never relies on implicit existence of system-wide packages**. It declares all dependencies, completely and exactly, via a dependency declaration manifest. Furthermore, it uses a dependency isolation tool during execution to ensure that no implicit dependencies “leak in” from the surrounding system. The full and explicit dependency specification is applied uniformly to both production and development.

- For golang code, we use [go dep](https://github.com/golang/dep) and [go
module](https://github.com/golang/go/wiki/Modules) to mangage dependencies.
- For javascript code, we use [npm](https://www.npmjs.com) to mangage
dependencies.
- For iOS / macOS app, we use [Carthage](https://github.com/Carthage/Carthage)
- For Android, we use Gradle which is a built-in tool in Android.

Every dependency should be pin a clear version for stability and should be cloned into the code repo.

## Config

> Store config in the environment

Apps sometimes store config as constants in the code. This is a violation of twelve-factor, which requires strict separation of config from code. Config varies substantially across deploys, code does not.

Your code must always be separated from the configuration (environment variables). We use a .env file to store all environment variables of the app, and this should be different among environments.

Example in Go, the binary file could be run in following options:

- With option to load config from .env files
- With option to load config from environment variables
- It also coule be run without any config file, loading default config.

## Backing services

Backing services (or infrastructure services) are datastores (such as MySQL or PostgreSQL), messaging/queueing systems (such as RabbitMQ or Beanstalkd), SMTP services for outbound email (such as Postfix), and caching systems (such as Memcached).

**Backing services should be treated as attached resources.**

The code for a twelve-factor app makes no distinction between local and third party services. To the app, both are attached resources, accessed via a URL or other locator/credentials stored in the config. A deploy of the twelve-factor app should be able to swap out a local MySQL database with one managed by a third party (such as Amazon RDS) without any changes to the app’s code.

We use environment variables to configure backing services (database for example), so we can easily change them.

## Build, Release and Run

You must strictly separate the Build (binary), Release (binary and + env config) and Run (exec runtime) stages. Our instances are immutable so we can't make change upstream (ex: it is impossible to make changes to the code at runtime since there is no way to propagate those changes back to the build stage.)

[We separate the environment into five](/engineering/environment.md), and which of them are isolated with each other. 

- We take advantage of Docker and Kubernetes for this factor. For golang code, we use the binary from **build** stage in docker image for **release** stage.

## Processes

**Execute the app as one or more stateless processes**.

Twelve-factor processes are stateless and share-nothing. Any data that needs to persist must be stored in a stateful backing service. That’s because resources in a cloud environment are ephemeral and should also be immutable. It, therefore, makes no sense to store files or session data in memory. We have a perfect match with containers because they are designed to run with just one scope, and of course, they are ephemeral.

## Port Binding

If we said in the backing service pattern that every service should be accessed via URL, that includes our app. Exporting services via port binding will allow us to become a backing service for another app via URL.

We define ports for all services in Dockerfile.

## Concurrency

In the twelve-factor app, processes are a first class citizen. Processes in the twelve-factor app take strong cues from the unix process model for running service daemons. Using this model, the developer can architect their app to handle diverse workloads by assigning each type of work to a process type

Although it might seem pretty obvious at first remember that if for any reason, you aren’t able to scale your app horizontally, it won’t be prepared for the cloud. The cloud must be a synonym of automation to ensure that we can create replicas of our application on-demand.

## Disposability

12-factor apps processes can be started and stopped at any time. Because of that, we have to strive to **minimize the startup time** and **shut down them gracefully**.

For instance, we shouldn’t stop an application when it’s writing to a backing service.

To do so, our app must be able to capture signals, ensure that we finish calls, and then stop the app.

## Dev/prod parity

[Keep development, staging, and production as similar as possible.](https://12factor.net/dev-prod-parity)

The twelve-factor app is designed for continuous deployment by keeping the gap between development and production small. Looking at the three gaps described above:

- Make the time gap small: a developer may write code and have it deployed hours or even just minutes later.
- Make the personnel gap small: developers who wrote code are closely involved in deploying it and watching its behavior in production.
- Make the tools gap small: keep development and production as similar as possible.

The twelve-factor developer resists the urge to use different backing services between development and production.

We achieve this using CI/CD flow with Docker containers. Every time developers merge to develop/master branch; this will be deployed automatically to development/production environment. Stacks between local/development/production/staging environments must be the same.

## Logs

**Treat logs as event streams**. Logs are the stream of aggregated, time-ordered events collected from the output streams of all running processes and backing services.

Services should never concern themselves with routing or storing logs. Instead, apps should be as agnostic as possible as to not depend on any other system or process. Then our app should just put logs in stdout and if we want to collect and ship them other processes/apps should be in charge of that.

- In development, the developer will view this stream in the foreground of their terminal to observe the app’s behavior.
- In staging or production deploys, each process’ stream will be captured by the execution environment, collated together with all other streams from the app, and routed to one or more final destinations for viewing and long-term archival.

The event stream for an app can be routed to a file, or watched via realtime tail in a terminal. Most significantly, the stream can be sent to a log indexing and analysis system such as Splunk, or a general-purpose data warehousing system such as Hadoop/Hive. These systems allow for great power and flexibility for introspecting an app’s behavior over time, including:

- Finding specific events in the past.
- Large-scale graphing of trends (such as requests per minute).
- Active alerting according to user-defined heuristics (such as an alert when the quantity of errors per minute exceeds a certain threshold).

## Admin Process

It's not an admin dashboard. An admin process is a way to interact with your app process to do one-off administrative or maintenance tasks for the app.

Twelve-factor strongly favors languages which provide a REPL shell out of the box, and which make it easy to run one-off scripts. 
- In a local deploy, developers invoke one-off admin processes by a direct shell command inside the app’s checkout directory. 
- In a production deploy, developers can use ssh or other remote command execution mechanism provided by that deploy’s execution environment to run such a process.