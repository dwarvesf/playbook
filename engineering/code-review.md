# Code review

How we do code review at Dwarves Foundation.

## Code review system

Every project member must have another team member or team leader to review his/her code, and every changes to the code must be achieved via a Pull request to the main development branch. On larger project (10 developers+) we may requires 2 reviewers. All of this is to ensure correctness, quality of the code and learning opportunities for both assignee (who develop) and reviewer.

We review everything on the Pull request - code, tests, documents, config files, .etc.

## Code review flow

- Feature/bug assignee finished coding on a new branch based off `develop` branch
- Assignee self-review the code he/her wrote one last time before assigning it to reviewer
- Assignee open a Pull request on project repository and assign the PR to a reviewer, removes any [WIP] prefix in PR title to let reviewer knows this PR is ready for review
- Assignee write down brief information about this PR (we have a PR template for this)
- Reviewer read and review PR's information, make sure to understand why do we have this PR
- Reviewer review/refactor the code with assignee
- Reviewer sign off on the pull request with a 👍 or “Ready to merge” comment
- Reviewer merge PR and clean up remote branch

## Code review standard

At minimal:

- The code must work and tests must pass
- No typos
- No debugging code .ie `console.log`/`fmt.Print`/.etc unless you have a good reason for it
- Never commit environment files to source control (.ie `.env` files for example), unless you have a good reason for it

### About testing

We don't follow TDD, we don't need to have tests for everything but we must have test to cover critical parts of the application. Test cases ensure that our app is working and without bugs whenever we introduce new features into it. Test cases also gives us the affordance to refactor and improve our code quality. With refactoring, we learn better coding practices and improve ourselves as a coder.

### Naming convention

This topic varies depends what programming language we use as each language have their own best practices, but a general rule is to make it clear about our intention - ie. if you have no idea what a particular variable is for while reviewing a helper function, then that variable needs more comments or a new name to better reflects it's intention.

### Single Responsibility Principle

Every class and method should only do one thing. We should not lump every logic and responsibility into a God class or a super method. This makes code difficult to understand and hard to change.

There are more, but we try to only put important items in this section, the rest would be better explained/introduced during coding/reviewing while working with your team.

## Dos and Don'ts

- Review the code, don't review the coder
- Check for correctness of code, not how you would do it
- Discuss possibilities & trade-offs, not what's right & what's wrong
- Try not to go into lengthy discussion and keep code review to less than 30 minutes

![](/engineering/img/code-review-hierarchy.png)