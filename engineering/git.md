# Source Version Control

It's like a time machine. We can save and reload anytime we want to, like the old day of Game Boy Advance. We can work in parallel universes of our source code, experimenting without fear of losing work, rolling back if something goes wrong.

[We use git](https://egghead.io/courses/how-to-contribute-to-an-open-source-project-on-github). Git is one of the most popular distributed source version control. We use [Github](github.com/dwarvesf/) to open source our works, playbook and practices. We also have a [self-host Gitlab](git.d.foundation) to store all of our source code.

#### Branching

There is only one eternal branch – you can call it master, develop, current, next – whatever. I personally like **“master”**, and that’s the name I’ll use in the rest of the document, as it’s convention by now in the Git world and immediately conveys its purpose.

All other branches (feature, release, hotfix, and whatever else you need) are temporary and only used as a convenience to share code with other developers and as a backup measure. They are always removed once the changes present on them land on master.

## Write a feature

Create a local `feature/` branch based off develop.

```
git checkout develop
git pull
git checkout -b feature/<branch-name>
```

Rebase frequently to incorporate upstream changes.

```
git fetch origin
git rebase develop
```

Resolve conflicts. When feature is complete and tests pass, stage the changes and commit them.

```
git add --all
git status
git commit --verbose
```

## Merge/Rebase

If you've created more than one commit, [use git rebase interactively](https://help.github.com/articles/about-git-rebase/) to squash like "Fix whitespace" into cohesive commits with good messages:

```
git fetch origin
git rebase -i origin/master
git push --force-with-lease origin <branch-name>
```

## Write a good commit message

Your commit messages are your personal legacy. They are the record of why and what you've done to a codebase

* Separate subject from body with a blank line
* Limit the subject line to 50 characters.
* Capitalize the subject line
* Do not end the subject line with a period
* Use the imperative mood in the subject line
* Wrap the body at 72 characters
* Use the body to explain what and why vs. how

[Read more](https://chris.beams.io/posts/git-commit/)

Once your work is committed, [submit a WIP pull request](#wip-pull-request).

## Pull Request

#### Open a WIP Pull Request as early as possible

A perfect implementation of the wrong specification is worthless. Before doing anything, write down how you want to implement the feature and submit for review.

It gives you a chance to **think through the solution** without the overhead of having to change code every time you change your mind about how something should be organized.

**Open a Pull Request as early as possible:** Pull Requests are a great way to start a conversation of a feature, so start one as soon as possible- even before you are finished with the code. Your team can comment on the feature as it evolves, instead of providing all their feedback at the very end.

## Review Pull Request & Feature

We practice using pull request in Github or merge request in Gitlab to send in code for new features. The Pull Request will be merged if passes both of conditions below:

- [Code review](/engineering/code-review.md) is done on the request before merging into the master.
- [Feature review](#link-to-testing-environment) is verified 

<!-- Quang viet review feature released -->

## Release

The master branch should be in production ready at all times. Use git tag with [semantic version](/engineering/versioning.md) when it's about time to release.

## Use template

To make things easier, we have adopted Issue template and Pull Request template that we think they are great to help the team to improve the productivity

- [Issue Template](https://github.com/dwarvesf/.github/blob/master/ISSUE_TEMPLATE.md)
- [Pull Request Template](https://github.com/dwarvesf/.github/blob/master/PULL_REQUEST_TEMPLATE.md)

## Q & A

**Q: I just committed a secret - how can I remove it from the git history?**

A: use git reset to remove them from the commit history. If you already pushed your changes to a public branch, ensure that all contributors that pulled these changes will pull the now updated branch.

If you have published these changes to a public repo like GitHub, you will first need to change the secrets. Then follow the instructions to [remove sensitive data from a repository](https://help.github.com/en/articles/removing-sensitive-data-from-a-repository). This can be accomplished through the git filter-branch command or using the BFG Repo-Cleaner and both options are covered in the instructions.

**Q: What if the client wants their own repository?**

A: If a client has their own GitHub team and wishes to own the repository, there will be a fork of the project that should be used for all issues, pull requests, comments, etc.
