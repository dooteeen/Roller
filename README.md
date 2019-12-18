Note: This repository is alpha version: planning phase now.

# Roller

Conflict resolving plug-in for Windows [scoop](https://github.com).

## Install

```
PS> scoop bucket add plugins dooteeen/scoop-plugins
PS> scoop install plugin-roller
PS> scoop_roller check

# Recommends:
PS> scoop alias _in "scoop install $*;   scoop_roller update"
PS> scoop alias _un "scoop uninstall $*; scoop_roller update"
PS> scoop alias _up "scoop update; scoop update $*; scoop_roller update"
```

## Usage

```
scoop_roller: conflit resolving plug-in for scoop.
Subcommands:
  check              Write/Update conflicting commands list
                     to "$SCOOP_DIR\roller\list.csv".
  get [CMD]          Get current state.
  help               Show this texts.
  resolve            Resolve all conflicts with CSV file.
  set [CMD] [PKG]    Resolve selected command's conflict.
  update             Alias of 'roller check; roller resolve'.
```

## Examples

```
PS> cat $DOTFILES\scoop\conflicts.csv >$SCOOP_DIR\roller\list.csv

PS> scoop_roller resolve
All 14 conflicts have resolved.
```

```
PS> scoop install gow vim

PS> scoop_roller set vim gow
Set vim's shim as main/gow's one.

PS> scoop_roller get vim
vim: main/gow (others: main/vim)
```

```
PS> scoop update; scoop update *

PS> scoop_roller check
All 15 conflicts have resolved.
```

## Plans

### Flows

`check`:

1. Find `$SCOOP_DIR\roller\list.csv`. If not, make it.
2. Collect all manifests `bin` definitions.
3. Store to powershell's array.
4. Convert PS's list to CSV.
5. (Over)Write CSV file to `$SCOOP_DIR\roller\list.csv`.

`resolve`:

1. Find `$SCOOP_DIR\roller\list.csv`. If not, throw error and exit.
2. Import data from CSV to PS's array.
3. (Re)Make junctions with array.

### CSV

Mock:

```
awk,nil,main/gawk,main/gow
bzip2,nil,main/gow,main/bzip2
git,main/git,main/git,main/git-with-openssh
make,nil,main/gow,main/make
vim,main/vim,main/gow,main/vim
```

Each columns contents:

- 1st: command name
- 2nd: resisted manifest name to use, or `nil`.
- 3rd and more: list of all defining manifests with bucket name.
