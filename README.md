# Git Tips

# Table of Contents

* [Legend](https://github.com/Michaelangel007/git_tips#legend)
* [Branches](https://github.com/Michaelangel007/git_tips#branches)
* [Config](https://github.com/Michaelangel007/git_tips#config)
* [Commit](https://github.com/Michaelangel007/git_tips#commit)
* [Diffs](https://github.com/Michaelangel007/git_tips#diffs)
* [Preview](https://github.com/Michaelangel007/git_tips#preview)

## Legend:

|Placeholder  | Description                 |
|:------------|:----------------------------|
|`<branch>`   | Branch name                 |
|`[<parent>]` | Optional parent branch name |
|`<branch1>`  | Branch name                 |
|`<branch2>`  | Branch name                 |
|`<commit>`   | Commit SHA1 hash            |
|`<file>`     | File to add to repository   |
|`<old>`      | Old filename or branch name |
|`<new>`      | New filename or branch name |
|`<user>`     | Git Hub user                |
|`<project>`  | Git Hub repository          |

## Branches

* Create from existing files and switch to branch: `git checkout -b <branch> [<parent>]`
* Compare file in different branchs: `git diff <branch1> <branch2> <file>`
 or `git diff <branch1>:<file1> <branch2>:<file2>`
* Delete **local** branch: `git branch -d <branch>`
* Delete **remote** branch: `git push origin -D <branch>`
* Merge single commit from different branch: `git cherry-pick <commit>`
* Rename branch: `git branch -mv <old> <new>`
* Resolve conflict: `git merge <branch>`
* Show branches: `git log --graph --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %C(dim green)(%cr) %C(cyan)<%an>%Creset' --abbrev-commit`

## Config

|Where  | Command                  |
|:------|:-------------------------|
|Global | `$(EDITOR) ~/.gitconfig` |
|Project| `$(EDITOR) .git/config`  |

### Current config

* Show currnt global configs: `git config --global -l`


### Colors

* Color Names: `git log -1 --pretty=format:"%Credred%Creset %Cgreengreen%Creset %C(Yellow)yellow%Creset %Cblueblue%Creset %C(magenta)magenta%Creset %C(cyan)cyan%Creset %C(white)white%Creset"`

* Color

    | Color    |
    |:---------|
    | `blue`   |
    | `cyan`   |
    | `green`  |
    | `magenta`|
    | `red`    |
    | `white`  |
    | `yellow` |

* Attribute

    | Attribute |
    |:----------|
    | `blink`   |
    | `bold`    |
    | `dim`     |
    | `italic`  |
    | `reverse` |
    | `ul`      |

* 24-bit color: `git log --format="%h%C(#ff69b4)%d%C(reset) %s"`

* Underline hash via `ul` and `noul`: `git log --format="%C(magenta ul)%h%C(white noul) %s"`

* Status colors

    ![Before](pics/status_before.png)

    ```
    [color "status"]
      added = yellow
      changed = green  # default: red
      untracked = cyan # default: red
    ```

    ![After](pics/status_after.png)


### Email and Name

    [user]
        name  = First Last
        email = First_Last@test.com

## Commit

* Create new repo: `git init`

* Commit file: `git add <file>; git commit -m "Message"`

* Push to Remote

    |Description          | Command                                                         |
    |:--------------------|:----------------------------------------------------------------|
    |Set remote (initial) | `git remote add origin https://github.com/<User>/<project>.git` |
    |Push remote (initial)| `git push -u origin master`                                     |
    |Push remote          | `git push`                                                      |

* Append files to last commit

    ```
    git add <file>
    git commit --amend
    ```

* List file names only

    ```
    git diff-tree --no-commit-id -r --name-status <commit>
    ```

    or of last commit:

    ```
    git diff-tree --no-commit-id -r --name-status $(git log -1 --format=format:%H)
    ```

* Move or Rename file: `git mv <old> <new>`

* (Add) partial change(s) of file: `git -p <file>` (See [SO #1085162](http://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git))

* Reset file: `git checkout -- <file>`

* Show commit SHA1 only: `git log --format=format:%H`

* Show SHA1 hash of last commit: `git log -1 --format=format:%H`

* Undo: `git reset --hard <commit>`


## Diffs

* Compare staged file (already added): `git diff --staged <file>`

* View Whitespace

    ```
    git config --global alias.df 'diff --ws-error-highlight=all'
    git df <file>
    ```

### P4Merge

To use P4Merge for Diffs and 3-Way Merges instead of built-in git tools:

* For diff

    ```
    git config --global diff.tool p4merge
    git config --global difftool.p4merge.path "/Applications/p4merge.app/Contents/Resources/launchp4merge"
    git config --global difftool.keepTemporaries false
    git config --global difftool.prompt false
    git config --global difftool.trustExitCode false
    ```

    Which updates git config as:

    ```
    [diff]
        tool = p4merge
    [difftool "p4merge"]
        path = /Applications/p4merge.app/Contents/Resources/launchp4merge
    [difftool]
        keepTemporaries = false
        trustExitCode = false
        prompt = false
    ```

* For merge

    ```
    git config --global merge.tool p4merge
    git config --global mergetool.p4merge.path "/Applications/p4merge.app/Contents/MacOS/p4merge"
    git config --global mergetool.keepTemporaries false
    git config --global mergetool.prompt false
    git config --global mergetool.trustExitCode false
    ```

## Preview

### HTML

* Preview HTML in a git repo:

    ```
    http://htmlpreview.github.io/?https://raw.githubusercontent.com/<user>/<project>/<branch>/<file>
    ```

Note: Branch is usually `master`

### Markdown

* Editor: [https://jbt.github.io/markdown-editor/](https://jbt.github.io/markdown-editor/)

* Add `png` to `README.md`: `![alt tag](http://url/to/img.png)`

* [x] TODO list: use `* [x]`

