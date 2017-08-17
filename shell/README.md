# Shell Script Guidelines

Some simple guides to writing shell-scripts

## Use version control
Always version control your shell scripts. Shell scripts may often seem
like second-class code, but it's still important to version it. 

## Use ShellCheck
ShellCheck is a static shell code analysis tool that will help improve
your skills as you use it. It will save you from your self!

### Install

```bash
brew install shellcheck
```

## Use Unofficial Bash Strict Mode
Start all shell-scripts with these 3 lines

```bash
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'
```

`set -e` will exit the script if any command returns a non-zero status.
Sometimes commands will return non-zero status even when there is no error.
To prevent this you can use these two options:

```bash
command_returning_non_zero || true

#or

set +e
command_returning_non_zero
set -e
```

`set -u` will prevent the use of undefined variables (use defaults for
positional arguments)

`set -o pipefail` will force pipelines to fail on the first non-zero status

`IFS=$'\n\t'` makes iterations and splitting less surprising

## Cleanup after your shell scripts
Add the following to cleanup after your script (remove tmp files, restart
services, etc.)

```bash
cleanup() {
  # ...
}
trap cleanup EXIT

## Test your scripts!
Using the [shunit2 testing framework](https://github.com/kward/shunit2)
```

## Log what your script is doing
It's important to add logging to your script so you know what happened
when something goes right or wrong! This snippet makes it really easy to log.

```bash
readonly LOG_FILE="/tmp/$(basename "$0").log"
info()    { echo "[INFO]    $*" | tee -a "$LOG_FILE" >&2 ; }
warning() { echo "[WARNING] $*" | tee -a "$LOG_FILE" >&2 ; }
error()   { echo "[ERROR]   $*" | tee -a "$LOG_FILE" >&2 ; }
fatal()   { echo "[FATAL]   $*" | tee -a "$LOG_FILE" >&2 ; exit 1 ; }
```
