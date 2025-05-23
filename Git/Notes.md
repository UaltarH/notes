# GIT guide

## Configure SSH key

```
ssh-keygen -t ed25519 -C "lo.gauthier.glo@gmail.com"
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519
```

## Basic Git config
```sh
git config --global user.name "Gauthier LO"
git config --global user.email "lo.gauthier.glo@gmail.com"
git config --global init.defaultBranch main
# to use notpad++ as git editor
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
# or to use vscode
git config --global core.editor "code --wait"
git config --global core.longpaths true
```
Now you can run `git config --global -e` and use VS Code as editor for configuring Git.

Add the following to enable support for using VS Code as diff tool:

```
[diff]
    tool = default-difftool
[difftool "default-difftool"]
    cmd = code --wait --new-window --diff $LOCAL $REMOTE
```

This leverages the new `--diff` option you can pass to VS Code to compare two files side by side.

To summarize, here are some examples of where you can use Git with VS Code:

- `git rebase HEAD~3 -i` allows to interactive rebase using VS Code
- `git commit` allows to use VS Code for the commit message
- `git add -p` followed by `e` for interactive add
- `git difftool <commit>^ <commit>` allows to use VS Code as diff editor for changes

## Ignore changes on a tracked file

### Ignore a tracked file

Use `--skip-tree` to ignore changes on a file such as an `ENV` file. Compare to `--assume-unchanged` option, it will not raise an error when switching branch.
```sh
# to ignore future changes on a tracked file
git update-index --skip-worktree [path/to/file.extension]

# to track changes on this file again
git update-index --no-skip-worktree skipworktree.txt
```
**Do not use this**
```sh
# to ignore future changes on a tracked file
git update-index --assume-unchanged [path/to/the/file.extension]

# to track changes on this file again
git update-index --no-assume-unchanged [path/to/the/file.extension]
```

### Ignore lines on tracked files

1. Create/Open gitattributes file:
   - `<project root>/.gitattributes` (will be committed into repo)
   - OR `<project root>/.git/info/attributes` (won't be committed into repo)
2. Add a line defining the files to be filtered:
`*.php filter=phpgitignore`, i.e. run filter named phpgitignore on all `.php` files
3. Define the phpgitignore filter in your gitconfig:
```sh
# delete lines that end with //ignore
git config --global filter.phpgitignore.clean "sed '/\/\/ignore$/d'"

# do nothing when pulling file from repo
git config --global filter.phpgitignore.smudge cat
```

## Sparse checkout

```sh
mkdir myrepo
cd myrepo
git init
git config core.sparseCheckout true
git remote add -f origin git://...
echo "path/within_repo/to/desired_subdir/*" > .git/info/sparse-checkout
git checkout [branchname] # ex: master
# to add more paths
git sparse-checkout add <path>
```

In Windows don't use quotes for pathname


## Clé GPG

### Génération de clé
```sh
$ gpg --full-gen-key
gpg (GnuPG) 2.2.23; Copyright (C) 2020 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
gpg: directory '/home/utilisateur/.gnupg' created
gpg: keybox '/home/utilisateur/.gnupg/pubring.kbx' created
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 1
```

```sh
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y
GnuPG needs to construct a user ID to identify your key.
Real name: Utilisateur De La Machine
Email address: utilisateur@machine.fr
Comment: démo
```

### Lister les clés GPG
```sh
gpg --list-secret-keys --keyid-format=long
```

### Exporter la clé
```sh
armor --export 1234AZER
```

### Signer un commit

```sh
$ git config --global user.signingkey A2A1B12FB3EBFA87
$ # git config --global gpg.program gpg2 # facultatif
$ # export GPG_TTY=$(tty) # facultatif
$ git config --global commit.gpgsign true
$ git commit -S -m "Signed commit"
```

### En cas d'erreur gpg
```sh
$ git config --global gpg.program gpg
$ git config --global gpg.program gpg2
$ export GPG_TTY=$(tty)
$ GIT_TRACE=1 git commit
$ gpg --status-fd=2 -bsau <your GPG key>
$ git config --global user.signingkey <your key>