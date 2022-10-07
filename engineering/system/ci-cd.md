# Continuous Integration

## What

Continuous Integration (CI) is a development practice where developers integrate code into a shared repository frequently, preferably several times a day. Each integration can then be verified by an automated build and automated tests. While automated testing is not strictly part of CI it is typically implied.

One of the key benefits of integrating regularly is that you can detect errors quickly and locate them more easily. As each change introduced is typically small, pinpointing the specific change that introduced a defect can be done quickly.

## Importance of CI

In order to understand the importance of CI, it's helpful first to discuss some pain points that often arise due to the absence of CI. Without CI, developers must manually coordinate and communicate when they are contributing code to the end product. This coordination extends beyond the development teams to the rest of the organization, as well. Product teams must coordinate when to sequentially launch features and fixes and which team members will be responsible.

Without a robust CI pipeline, a disconnect between the engineering team and the rest of the org can form. Communication between product and engineering can be cumbersome. Engineering becomes a black box which the rest of the team inputs requirements and features and maybe gets expected results back. It will make it harder for engineering to estimate time of delivery on requests because the time to integrate new changes becomes an unknown risk.

## CI at Dwarves

- Write tests, mostly integration: we don't do TDD, but we do write necessary unit tests, and lots of integration tests (because they ensure everything works as a whole), both kind of tests should be able to run on CI 
- Pull requests and code review: our CI system handles both individual commit pushs and PRs, in case of PRs - we create a separated deployment preview (for web apps), so everyone can visit and verify if everything works as intended
- Optimize pipeline speed: Given that the CI pipeline is going to be a central and frequently used process, it is essential to optimize its execution speed. Any small delay in the CI workflow will compound exponentially as the rate of feature releases, team size, and codebase size grows. It is a best practice to measure the CI pipeline speed and optimize as necessary.
  - customized Docker image: have a dedicated Docker image which includes prebuilding steps .eg migration tools and CLI binaries like `goose`, `psql`, `yarn`, `prettier`
  - dependencies caching: cached go vendor or `node_modules` dir can significantly improve pipeline speed
- Collect build artifacts: can be helpful to re-verify previous builds manually

# Continuous Delivery

Continuous integration, deployment, and delivery are three phases of an automated software release pipeline. These three phases take software from idea to delivery to the end-user. The integration phase is the first step in the process. Integration covers the process of multiple developers attempting to merge their code changes with the master code repository of a project.

Development teams have seen enormous benefits from implementing continuous integration (CI) and continuous delivery (CD) as well as continuous deployment practices. By setting up a CI/CD pipeline, teams can merge small changes often, release code to production frequently, and feel confident doing so because any changes trigger a build that is tested automatically before being released.

## CD at Dwarves

At Dwarves Foundation, achieving an efficient deployment pipeline is done by following these best practices:

- The process for releasing/deploying software MUST be repeatable and reliable
- Automate everything - automate all the tasks you repeatedly do, and this tends to lead to reliability
- Done means "released."
- Build quality comes first - take the time to invest in your quality metrics. A project with good, targeted quality metrics will invariably be better than one without, and easier to maintain in the long run
- Tagging your builds and smoke test your deployments

# Setup CI/CD pipeline

- Pick your own CI platform: Gitlab CI, Circle CI or Bitrise for iOS
- Write the CI instruction file: Most of the time the configuration file should be a YAML file; file structure should be different following your chosen CI platform.
- There are a few stages that we need to specify: build, test, packaging, and push artifacts.
- CI Server will trigger the target server to run the deployment script. Depends on the current setup, we may use Ansible or K8s to make it happen.
- For mobile applications, the deployment may be varied. Some will use Fastlane to build and Fabric/Testflight for app distribution.

# Checklist

## CI
To what extent did the CI cover unit tests, linting and build the codebase with correct environment configuration?
- [ ] The pipeline covers test, build, packaging and push artifacts stage
- [ ] CI environment is cleaned from starting
- [ ] Pipeline is fast as possible
- [ ] Use cache (vendor, node_modules, ...)
- [ ] Save artifacts after building stage
- [ ] Environment variables won't display when running CICD
- [ ] When building the app with docker, use alpine image
- [ ] When building the app with docker, the image should have tag:
    - as `develop` for `develop` branch.
    - as `semantic versioning` when releasing tag and mark it as latest as well. For FE, you may need to build more than 1 version for a released tag e.g. v1.0.0-rc, v1.0.0-beta
- [ ] For mobile, remember to write a script to have increase build number automatically
- [ ] The API target should be configured appropriately to reflect the current build environment. Avoiding pointing to different API version or environment.

## CD
To what extent did the CD deliver correct version, make sure configuration is up-to-date and can be rollback
- [ ] Make sure environment variables has been up-to-date after deploying
- [ ] Use image tag in different environments:
    - develop use tag `develop`
    - staging use tag `rc` or v1.0.0-rc (for FE)
    - production use `specific semantic tag` e.g. v1.0.0
- [ ] Make sure the deployment has been shipped to correct environment (production, staging, ...)
- [ ] For development environment, the database has been setup auto migrate after deploying new version

## Deployment Checklist
To make sure that will the next release won't break current production by figuring out potential errors which is happening in development env (if any)

### Check Last Stable Deployment
- [ ] Was there a rollback previously?
- [ ] Check ArgoCD for hash of commit → cross reference with github to see the tag

### Check monitoring tools for nominal signals
- [ ] Sentry - are new issues present in development environment since last release? Alarms?
- [ ] Grafana - do dashboards all show normal utilisation rates?
- [ ] Upptime - are there any alarms since the last release? If so, have they been addressed?
- [ ] Loki - if manual review of logs is required.

### Actions

- [ ] Run smoke tests 

### Post Deployment
- [ ] Check all monitoring tools listed above for issues post deployment