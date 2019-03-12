# Release Checklist

When you are ready to release, remember to check off everything on your release checklist.
Just remember to not mess up with the end users. Our responsibility is to keep the system stable with seamless deployment.

- [ ] Do NOT release on Friday or weekend. We can't handle if any issue happens.
- [ ] The milestone was fully implemented.

**Git**

- [ ] The PRs contained context and information for tracing back.
- [ ] Git commits message was squashed up.
- [ ] New version was bumped and pushed using git tag. Any version of the product should be easily mappable to a state of the code base

**Testing**

- [ ] All the test cases passed or with known issues.
- [ ] The product has been tested from the networks from where it will be used (e.g. public Internet, customer LAN)
- [ ] The product has been tested with all of the targeted devices

**Changelog**

- [ ] Changelog was included in the new build.
- [ ] Changelog was [useful](/engineering/changelog.md).
- [ ] Known issues and problems was specified.

**Dependencies**

- [ ] Any packages need to update? (ie. security issue?)

**Database**

- [ ] Backups are running
- [ ] Rolling back a deployment is possible
- [ ] Need to do migration on the new build?
- [ ] Need to back up the database before migration?

**Variables**

- [ ] No secrets are stored in version control
- [ ] Is config stored in environment variables?
- [ ] Is new variables deployed?
- [ ] Server environments are up-to-date

**Monitoring**

- [ ] Logging is turned on
- [ ] There is a well defined process for accessing and searching through logs
- [ ] Logging includes exceptions and stack traces where appropriate
- [ ] Errors can be mapped to stack traces
- [ ] The product has been load tested? *

**Environment**

- [ ] Deploying works the same no matter which environment you are deploying to
- [ ] All environments have well defined names, and they are referred to using those names
- [ ] All environments have the same underlying software stack
- [ ] All environment configuration is version controlled (web server config, CI build scripts etc.)
- [ ] There is a simple way to find out what code is running in any given environment

**Everyone aware of new release**

- [ ] Email sent to customers.
- [ ] Email sent to the product owner.
- [ ] Email sent to the project team.
