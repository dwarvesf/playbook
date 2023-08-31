### Multiple git credentials

In some cases you might have multiple git accounts, say for example, company Github/Gitlab account, client project Github/Gitlab Enterprise. Update your SSH config will allow you work with multiple git accounts

#### SSH config

SSH allows we able to have multiple credentials base on hostname

- Generate new SSH key and add it to your git account by (follow this guide)[https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent]

- Update ssh config

**Use case 1: multiple hosts\***

```bash
# ~/.ssh/config
Host code.d.foundation
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_dwarves

Host gitlab.d.foundation
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_dwarves_gitlab
    # Or you can reuse the `id_dwarves` it's ok since it's two different host
    # IdentityFile ~/.ssh/id_dwarves
```

Usage:

```bash
git clone git@code.d.foundation:dwarvesf/playbook.git
#or
git clone git@gitlab.d.foundation:dwarvesf/playbook.git
```

**Use case 2: using host alias\***

```bash
Host df
    HostName code.d.foundation
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_dwarves
```

Usage:

```bash
# the original: git@code.d.foundation:dwarvesf/playbook.git
git clone git@df:dwarvesf/playbook.git
```

#### Update local repo config

git allows you to have local repo configuration. you can set your own git username and email for each local repo by running these command

```bash
git config user.email "team@d.foundation"
git config user.username="team"
```
