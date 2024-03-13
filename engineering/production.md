# Production Checklist
- [ ] Long-running processes such as email should delivery being run in background jobs?
- [ ] Are there redundant (at least two) web and background processes running?
- [ ] Are we using SSL? Never send credentials unencrypted over public network. Always use encryption (such as HTTPS, SSL, etc.).
- [ ] Are API requests being made via a separate subdomain (api.example.com)? Even if the same app, this gives us architectural flexibility in the future.
- [ ] Are we monitoring performance and uptime?
- [ ] Are deploys done manually at a scheduled time when teammates are fresh and available if something goes wrong?
- [ ] Do deploys follow a well-documented script?
- [ ] A method exists for replicating the state of one environment in another (e.g. copy prod to QA to reproduce an error)
- [ ] All repeating release processes have been automated

**Variables**

- [ ] No secrets are stored in version control
- [ ] Is config stored in environment variables?
- [ ] Are new variables deployed?
- [ ] Server environments are up-to-date
- [ ] A plan for updating the server environments exists

**Back up**

- [ ] Are we backing up our production database?
- [ ] Backups are running
- [ ] Restoring from a backup has been tested
- [ ] Rolling back a deployment is possible

**Logging**

- [ ] Logging is turned on
- [ ] There is a well defined process for accessing and searching through logs
- [ ] Logging includes exceptions and stack traces where appropriate
- [ ] Are we sending logs to a remote logging service?

**Error Tracking**

- [ ] Are we tracking errors?
- [ ] Errors can be mapped to stack traces
