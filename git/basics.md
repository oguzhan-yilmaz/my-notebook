# Git: Basics

## Initialize A Repository

```bash
git init
```

This creates a new subdirectory named `.git` that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet.

## Cloning A Repository

```bash
git clone https://github.com/oguzhan-yilmaz/pyCrossfade.git

# You can also give it a new name
git clone https://github.com/oguzhan-yilmaz/pyCrossfade.git MYPYCROSSFADE
```

## Recording Changes to the Repository

Remember that each file in your working directory can be in one of two states: tracked or untracked. Tracked files are files that were in the last snapshot; they can be unmodified, modified, or staged. In short, tracked files are files that Git knows about.

## Checking the Status of Your Files

```bash
git status
> On branch master
> Your branch is up to date with 'origin/master'.
# to get a short description use option -s or --short
git status -s 
```

## Tracking New Files
In order to begin tracking a new file, you use the command `git add`. 
```bash
# add single file
git add [filename]
# add all .py files under a folder
git add [foldername]/*.py
```


## Ignoring Files
These are generally automatically generated files such as log files or files produced by your build system. In such cases, you can create a file listing patterns to match them named .gitignore. Here is an example .gitignore file:

```.gitignore
# ignore all .a files
*.a
# but do track lib.a, even though you're ignoring .a files above
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO
# ignore all files in any directory named build
build/
# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt
# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```