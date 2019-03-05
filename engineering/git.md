# Source Version Control

## Q & A

Q: I just committed a secret - how can I remove it from the git history?
A: use git reset to remove them from the commit history. If you already pushed your changes to a public branch, ensure that all contributors that pulled these changes will pull the now updated branch.

If you have published these changes to a public repo like GitHub, you will first need to change the secrets. Then follow the instructions to [remove sensitive data from a repository](https://help.github.com/en/articles/removing-sensitive-data-from-a-repository). This can be accomplished through the git filter-branch command or using the BFG Repo-Cleaner and both options are covered in the instructions.