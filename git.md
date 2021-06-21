# Git

## Git config

Once per dev machine, configure git with the following:

```bash
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

git config --global core.autocrlf false
```

Remember to always use a text editor configured with **LF** newlines. Special care on Windows which the default is CR-LF.

## Git Flow

 * [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
 * [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
 * [GitFlow: la méthodologie et la pratique](https://blog.nathanaelcherrier.com/2016/07/11/gitflow-la-methodologie-et-la-pratique/)
 * [GitFlow CheatSheet](https://danielkummer.github.io/git-flow-cheatsheet/)
 * [git-flow commands VS raw git commands](https://gist.github.com/JamesMGreene/cdd0ac49f90c987e45ac)

### Windows Install

```bash
choco install gitflow-avh
```

### macOS Install

```bash
brew install git-flow-avh
```


## Best practices for Git Commit ([source](https://chris.beams.io/posts/git-commit/))
 1. Separate subject from body with a blank line
 2. Limit the subject line to 50 characters
 3. Capitalize the subject line
 4. Do not end the subject line with a period
 5. Use the imperative mood in the subject line ("If applied, this commit will __your subject line here__")
 6. Wrap the body at 72 characters
 7. Use the body to explain what and why vs. how

## Advanced usage

 * [Rules to Git By (Explaining git internal structure, rebase vs merge, bisect)](https://www.youtube.com/watch?v=yI0BtEzdGtw)
 * [CS Visualized: Useful Git Commands](https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1)

## External resources

 * [GIT.WTF!!](https://git.wtf)
 
## What should be left OUT?

 - All **generated** files
 - All dependencies managed through a package manager
 - All local config files
 - Your personal IDE config files
 - Big binary files (.psd, .zip, ...)

### `node_modules`

- https://byjoeybaker.com/why-you-should-never-commit-node-modules-in-nodejs
- https://flaviocopes.com/should-commit-node-modules-git/
- https://www.positronx.io/should-i-commit-the-node_modules-folder-to-git/
- https://stackoverflow.com/questions/18128863/should-node-modules-folder-be-included-in-the-git-repository


