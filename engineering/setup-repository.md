# Repository setup

How we expect projects to be setup.

## What we expect

- [README.md](readme-how.md) in the root of the repo is the docs
- Single command run
- Single command deploy
- Repeatable and re-creatable builds
- Build artifacts bundle a ["Bill of Materials"](backend.md#bill-of-materials)

### Technical

Project setup must be self-contained, no _"It works on my machine"_ bullshit. If its failed to run the setup script on your coworker's machine, it needs a fix 🔧

Every Backend codebase should have:

- `make up` install all dependencies
- [`make run`](https://github.com/huygn/stringsvc/blob/master/Makefile#L20-L21) to run the app in dev mode
- `make build` to compile for production

Every Frontend codebase should have:

- [`.nvmrc`](https://github.com/creationix/nvm#nvmrc) file to lock down the Nodejs version being used
- `yarn install` to install all dependencies
- `yarn start` to start development server
- `yarn build` to compile for production
- `yarn serve` to run/serve the compiled output

### Deployment

All engineers should be aware about deployment and how your application would behave/run on a cloud environment. At minimal, Backend/Frontend apps must be cloud-friendly:

- Every project must have a ready-to-deploy [Dockerfile](https://docs.docker.com/engine/reference/builder/), and all environment variables must be configurable .ie:

  ```sh
  docker build --build-arg API_ENDPOINT=http://localhost:9000  -t dwarvesf/fortress-web .
  ```

- Every Backend source code must have a healthcheck endpoint (`/api/health` for example)

### Documentation

At minimal:

- How to install/run/build/deploy the source code
- Required environment variables / configurations

Nice to have:

- Architecture diagram - if this is a complex application that contains more than one service (micro-services) or involves integrating with several 3rd party services
- Technical decisions - .ie why you chose this library over that 10k-stars-github-repo library? It would be really helpful to new comers as they go through the source code

## Best practices

- We recommend [Docker Compose](https://docs.docker.com/compose/) to make setting up a project on a new development machine fast and easy
- Your `docker-compose.yml` file should cover everything your app needs to function properly, including database and migrations. This also ensures everyone is locked on the same Go/Nodejs/Clojure/.etc version
- For Golang projects, leverage Docker Compose to build & run your app in Alpine/Linux environment, this ensures your app shall work correctly when deployed to the cloud
- Avoid putting secrets in the source code. Those should be stored in environment variables to make it configurable in CI/CD pipelines as well as deployment process
