# git-ignore-command
A custom command for git to manage the gitignore file (with help from gitignore.io)

## Installation

Copy the file `git-ignore` somewhere on your PATH so that it can be found by git.

NOTE: this application has only been tested on Linux, although I have tried to
make it as cross-platform as possible. I am not sure if git on Windows will be
able to execute Python files with a shebang (`#!/usr/bin/env python3`) in
custom commands.

```bash
cp git-ignore ~/.local/bin/ && chmod +x ~/.local/bin/git-ignore
```

## Test

To ensure the command installed properly, run the following. If properly
installed, a list of all languages supported by gitignore.io will be displayed
in the terminal.

```bash
git ignore --list
```

## Usage

### Find the path of .gitignore file

This command will display the absolute path to the .gitignore file
that applies to the _top level_ git repository in your current directory
or a parent. If you are in a submodule, it will find the .gitignore
file of the parent module.

If this command is run outside of a git repository or a .gitignore file
does not exist, it will display nothing.

```bash
git ignore --find
```

Example output:

```
/home/me/prg/.gitignore
```

### List all available languages

This command will display a list, one per line, of all available languages
that have .gitignore templates on gitignore.io.

This command can be run outside of a git repository.

```bash
git ignore --list
```

Example output:

```
1c
1c-bitrix
a-frame
actionscript
...
```

### Display templates for a selection of languages

This command will query for a template from gitignore.io and print it
to the terminal. After `--list` put all of the languages you want in the
template. Your existing gitignore, if any, will not be modified.

This command can be run outside of a git repository.

```bash
git ignore --list c++ python
```

Example output:

```
# Created by https://www.gitignore.io/api/c++
# Edit at https://www.gitignore.io/?templates=c++

### C++ ###
# Prerequisites
*.d

# Compiled Object files
*.slo
*.lo
*.o
...
```

### Append a template for a language to your gitignore file

This command will query gitignore.io for one (or more) languages and append
the resulting template to your gitignore file. The .gitignore file
applies to the _top level_ git repository in your current directory
or a parent. If you are in a submodule, it will find the .gitignore
file of the parent module.

If no .gitignore file exists, one will be created in the root of your
_top level_ repository and named `.gitignore`.

The program will attempt to de-duplicate byte-identical directives that are in
the existing .gitignore file (if available). It cannot detect directives
that are slightly different (e.g. whitespace) but otherwise semantically
identical to an existing directive. It tries to keep comments/errors and is
_not_ idempotent if the same languages are appended multiple times.

NOTE: Since directives later in the file take priority over conflicting
directives earlier in the file, there is a chance that partially-de-duped
directives may end up out of the expected order. When dealing with a complex
.gitignore, it is recommended to double check the whole file and make sure it
still is sensible.

```bash
git ignore -a c++ python
```
