# leaf

> General purpose hot-reloader for all projects.

Command leaf watches for changes in the working directory
and runs the specified set of commands whenever a file
updates. A set of filters can be applied to the watch and
directories can be excluded.

## Contents

1. [Installation](#installation)
    1. [Homebrew](#homebrew)
    1. [Using `go get`](#using-go-get)
    1. [Manual](#manual)
1. [Usage](#usage)
    1. [Command line help](#command-line-help)
    1. [Configuration file](#configuration-file)

## Installation

### Homebrew

You can use my homebrew tap to install Leaf.

```
❯ brew tap vrongmeal/tap
❯ brew install vrongmeal/tap/leaf
```

### Using `go get`

The following command will download and build Leaf in your
`$GOPATH/bin`.

```
❯ go get -u github.com/vrongmeal/leaf/cmd/leaf
```

### Manual

1. Clone the repository and `cd` into it.
1. Run `make build` to build the leaf as `build/leaf`.
1. Move the binary somewhere in your `$PATH`.

## Usage

```
$ leaf -x 'make build' -x 'make run'
```

The above command runs `make build` and `make run` commands
(in order).

### Command line help

The CLI can be used as described by the help message:

```
❯ leaf help

Command leaf watches for changes in the working directory and
runs the specified set of commands whenever a file updates.
A set of filters can be applied to the watch and directories
can be excluded.

Usage:
leaf [flags]
leaf [command]

Available Commands:
help        Help about any command
version     prints leaf version

Flags:
-c, --config string     config path for the configuration file (default "<CWD>/.leaf.yml")
    --debug             run in development (debug) environment
-d, --delay duration    delay after which commands are run on file change (default 500ms)
-e, --exclude strings   paths to exclude from watching (default [.git/,node_modules/,vendor/,venv/])
-x, --exec strings      exec commands on file change
-f, --filters strings   filters to apply to watch
-h, --help              help for leaf
-r, --root string       root directory to watch (default "<CWD>")

Use "leaf [command] --help" for more information about a command.
```

### Configuration file

In order to configure using a configuration file, create a
YAML or TOML or even a JSON file with the following structure
and pass it using the `-c` or `--config` flag. By default
a file named `.leaf.yml` in your working directory is taken
if no configuration file is found.

```yaml
# Leaf configuration file.

# Root directory to watch.
# Defaults to current working directory.
root: .

# Exclude directories while watching.
# If certain directories are not excluded, it might reach a
# limitation where watcher doesn't start.
exclude:
  - DEFAULTS # This includes the default ignored directories
  - build/
  - scripts/

# Filters to apply on the watch.
# Filters starting with '+' are includent and then with '-'
# are excluded. This is not like exclude, these are still
# being watched yet can be excluded from the execution.
# These can include any regex supported by filepath.Match
# method or even a directory.
filters:
  - '+ go.*'
  - '+ *.go'
  - '+ cmd/'

# Commands to be executed. These are run in the provided order.
exec:
  - make build

# Delay after which commands are executed.
delay: 1s
```

The above config file is suitable to use with the current
project itself. It can also be translated into a command
as such:

```
❯ leaf -x 'make build' -d '1s' \
  -e 'DEFAULTS' -e 'build' -e 'scripts' \
  -f '+ go.*' -f '+ *.go' -f '+ cmd/'
```

---

Made with **khoon**, **paseena** and **love** `:-)` by

Vaibhav ([vrongmeal](https://vrongmeal.github.io))
