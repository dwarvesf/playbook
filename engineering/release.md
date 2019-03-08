# Release Checklist

Just remember to not mess up with the end users. Our responsibility is to keep the system stable with seamless deployment.

- [ ] Do NOT release on Friday or weekend. We can't handle if any issue happens.
- [ ] The milestone was fully implemented.

**Git**

- [ ] The PRs contained context and information for tracing back.
- [ ] Git commits message was squashed up.
- [ ] All the test cases passed.
- [ ] New version was bumped and pushed using git tag.

**Changelog**

- [ ] Changelog was included in the new build.
- [ ] Changelog was [useful](/engineering/changelog.md).
- [ ] Known issues and problems was specified.

**Dependencies**

- [ ] Any packages need to update? (ie. security issue?)

**Database**

- [ ] Need to do migration on the new build?
- [ ] Need to back up the database before migration?

**Variables**

- [ ] Is config stored in environment variables?
- [ ] Is new variables deployed?

**Everyone aware of new release**

- [ ] Email sent to customers.
- [ ] Email sent to the product owner.
- [ ] Email sent to the project team.