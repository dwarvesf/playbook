# Production Checklist

- [ ] Are long-running processes such as email delivery being run in background jobs?
- [ ] Are there redundant (at least two) web and background processes running?
- [ ] Are we using SSL?
- [ ] Are API requests being made via a separate subdomain (api.example.com)? Even if the same app, this gives us architectural flexibility in the future.
- [ ] Is config stored in environment variables?
- [ ] Are deploys done manually at a scheduled time when teammates are fresh and available if something goes wrong?
- [ ] Do deploys follow a well-documented script?
- [ ] Are we sending logs to a remote logging service?
- [ ] Are we backing up our production database?
- [ ] Are we monitoring performance and uptime?
- [ ] Are we tracking errors?