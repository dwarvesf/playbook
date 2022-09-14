# Environments

This section describes the environments you should have, at a minimum. It might sound like a lot, but there is a purpose for each one.

- [Local development](#local-development-environment)
- [Continuous integration](#continuous-integration-environment)
- [Testing](#testing-environment)
- [Staging](#staging-environment)
- [Production](#production-environment)

We use docker and docker-compose to set up our environments. Before getting more profound, we suggest you install our [dotfiles](https://github.com/dwarvesf/dotfiles), where we share the configuration and setting.

## Local development environment

This is your device, where you write code. The product you're building can be run here, and any changes to it can be tested with minimal delay. Integrations to external services are replaced with mockups or stubs to avoid daily work being interrupted by network issues or server downtime. In particular, any databases are local. However, highly available public services may be used as such.

## Continuous integration environment

Continuous Integration (CI) builds your product and runs all automated tests as soon as anything changes in the codebase. Failures are reported immediately. CI also runs end-to-end integration tests.

## Testing environment

The Testing environment used by dedicated testers doubles as a production-like playground for developers. External integrations are by default set up to staging-level versions of other services. Developers can experiment with and try out new integrations here—this environment is also known as Integration Testing.

## Staging environment

Staging is set up precisely like production. No changes to the production environment happen before having been rehearsed here first. Any mysterious production issues can be debugged here.

## Production environment

The big iron. Logged, monitored, cleaned up periodically, squared away and secured.

This set helps developers to come up with the best technical solutions and resolve issues quickly, and users will only ever notice a new release by the product's increased awesomeness.


## Environment variables
- All environment must have exact list of variables
- Your code should support load env variable from both `.env` file or from the `process`
- In local development we use `.env` file to store all required environment variable. 
- In production we use platform secret manager to set environment variables
- **Do not** commit your `.env` file or hard-coded secret variables. We must set up the `.gitignore` file in our project.
- Variant of `.env` file can be
  - `.env.example` can commit source code store default public variables, all private or secret values must not include in this file
  - `.env.local` or `.env.development` - indicate that this store values for local environment
  - `.env.test` - indicate that this store values for test environment
  - `.env.staging` - indicate that this store values for staging environment
  - `.env.production` - indicate that this store values for test environment