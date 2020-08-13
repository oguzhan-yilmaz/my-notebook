# Git: General


## Integrity
Everything in Git is checksummed before it is stored and is then referred to by that checksum. This means it’s impossible to change the contents of any file or directory without Git knowing about it. You can’t lose information in transit or get file corruption without Git being able to detect it.

The mechanism that Git uses for this checksumming is called a SHA-1 hash. This is a 40-character string composed of hexadecimal characters (0–9 and a–f) and calculated based on the contents of a file or directory structure in Git. A SHA-1 hash looks something like this: 
`24b9da6552252987aa493b52f8696cd6d3b00373`

## The Three States
Git has three main states that your files can reside in: *modified*, *staged*, and *committed*:
- **Modified** means that you have changed the file but have not committed it to your database yet.
- **Staged** means that you have marked a modified file in its current version to go into your next commit snapshot.
- **Committed** means that the data is safely stored in your local database.

## First-Time Git Setup
Now that you have Git on your system, you’ll want to do a few things to customize your Git environment.

Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates.


|  option   |      which file      |  about |
|---------- |:-------------:|------|
| `--system`  | `[path]/etc/gitconfig` | Applied to every user on the system |
| `--global`  |    `~/.gitconfig `   |   Values specific personally to you, the user. |
| `--local`  | `.git/config` | Specific to that single repository |


### How to use `git config`

```bash
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

### How to see your git configs

```bash
git config --list --show-origin
```

### Defining a default editor
```bash
git config --global core.editor vim
```

### Defining default branch name
By default Git will create a branch called master when you create a new repository with `git init`.
```bash
git config --global init.defaultBranch main
```

