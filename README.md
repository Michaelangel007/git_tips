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

Legend of commit hash aliases:

|Alias       |Description                          |
|:-----------|:------------------------------------|
|`<commit>`  |Specific commit                      |
|`HEAD`      |Current head                         |
|`HEAD~#`    |Previous #-th commit relative to head|
|`HEAD^`     |Parent of head                       |
|`<commit>^` |Parent of specific commit            |
|`<commit>^^`|Grand-parent of specific commit      |

* Create new repo: `git init`

* Show state of local files (_staged_, _modified_, _untracked_): `git status --short`

* Add (new or existing) file to be staged: `git add <file>`
  * Undo file from add (unstage): `git reset <file>`

* Add **partial change(s)** of file: `git add -p <file>` (See [SO #1085162](http://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git))

* Commit staged file(s): `git commit -m "Message"`
  * Add and commit all changed files only: `git commit -a -m "Message"`

* Append files to last commit

    ```
    git add <file>
    git commit --amend
    ```

* Push to Remote

    |Description          | Command                                                         |
    |:--------------------|:----------------------------------------------------------------|
    |View remote URLs set | `git remote -v`                                                 |
    |Set remote (initial) | `git remote add origin https://github.com/<User>/<project>.git` |
    |Push remote (initial)| `git push -u origin master`                                     |
    |Push remote          | `git push`                                                      |

* Merge

  |Description              |Command                         |
  |:------------------------|:-------------------------------|
  |Show SHA1 of last commit |`git log -1 --format=format:%H` |
  |Merge start              |`git mergetool`                 |
  |Undo (**local** merge)   |`git reset --hard <commit>`     |
  |Merge done               |`git commit`                    |
  |Undo (**remote** merge)  |`git revert -m 1 <commit>`      |

  **Notes:**

  * `mergetool` may default to _none_
  * Should NOT need to force push: `git push origin master --force`
  * Undo remote merge see [SO #11722533](http://stackoverflow.com/questions/11722533/rollback-a-git-merge)
    * `-m 1` first parent of the merge commit on the master branch
    * `-m 2` first parent on the develop branch where the merge came from initially

* Move or Rename file: `git mv <old> <new>`

* Show

  |Description                |Command                                                |Example|
  |:--------------------------|:------------------------------------------------------|:------|
  | File at specific commit   | `git show --format=format:%d <commit>:[<file>]`       | |
  | File for last `#` commits | `git show -# <file>`                                  | |
  | Log _all_ commits         | `git log`                                             | |
  | Log _last `#`_ commits    | `git log -#`                                          | |
  | Log summary               | `git log -2 --format=%B`                              |<pre>Merge branch 'master' of https://...<br><br>Move ...</pre>                                              |
  | Log of filenames only     | `git log -2 --name-only --pretty=format:   [<branch>]`|<pre><br>README.md </pre>                                                                                    |
  | Log of hash + filenames   | `git log -2 --name-only --pretty=format:%H [<branch>]`|<pre>868b8a2dc91eed45242e8d4ebb3aabd5b1965c47<br>b88f8a267bc93ab43c440b81e5ac95e6943d5c98<br>README.md </pre>|
  | Log of hash, summary, file| `git log -2 --name-only --oneline`                    |<pre>868b8a2 Merge branch 'master' of https://...<br>Move ...<br>README.md</pre>                             |
  | SHA1 hash of last commit  | `git log -2 --format:%H`                              |<pre>868b8a2dc91eed45242e8d4ebb3aabd5b1965c47<br>b88f8a267bc93ab43c440b81e5ac95e6943d5c98</pre>              |
  | List of changed filenames | `git diff-tree --no-commit-id -r --name-status HEAD^` |<pre>M      README.md</pre>                                                                                  |
  |                           | `git diff-tree --no-commit-id -r --name-status <commit>`| |
  | ... 2nd last commit       | `git diff-tree --no-commit-id -r --name-status $(git log -2 --format=format:%H)`| |
  | Summary of last commit +/-| `git diff --stat HEAD^!`                              |<pre> README.md &#124; 2 +-<br> 1 file changed, 1 insertion(+), 1 deletion(-)</pre>                          |

  **Note:**

  * Show is _relative_ to **root of repository**, _NOT_ your current directory.

* Undo all local changes to `<file>` (reset file): `git checkout -- <file>`

## Diffs

|Description                    | Command                    |
|:------------------------------|:---------------------------|
|Before add, all    file changes| `git diff`                 |
|Before add, single file changes| `git diff <file>`          |
|After  add, single file changes| `git diff --staged <file>` |
|Compare to previous commit     | `git diff HEAD^`           |

* View Whitespace

  * Use: `git diff --ws-error-highlight=all`

  * Make alias

    ```
    git config --global alias.df 'diff --ws-error-highlight=all'
    git df <file>
    ```

### P4Merge

To use P4Merge for Diffs and 3-Way Merges instead of built-in git tools:

* For `git difftool`

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

* For `git mergetool`

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

* [x] TODO list: use `* [x] Task`


# See:

* [Git Tips](https://github.com/Michaelangel007/git_tips)
* [Git Cheat Sheet](https://github.com/Michaelangel007/git_cheat_sheet)
