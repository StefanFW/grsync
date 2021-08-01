# grsync — golang rsync wrapper

[![Build Status](https://travis-ci.com/schnes4/go-rsync.svg)](https://travis-ci.com/schnes4/grsync)
[![codecov](https://codecov.io/gh/schnes4/go-rsync/branch/master/graph/badge.svg)](https://codecov.io/gh/schnes4/grsync)

This lib is a lightweight fork from [zloylos/grsync](https://github.com/zloylos/grsync) with focus on rsync itself and some fixes.
The stdout of the rsync command is that good to print it to stdout if necessary instead of wrapping it.

The history/fork was removed because of problems during `go get`.
The work is based on the v1.5.0 release of zloylos/grsync.

## Configuration

All rsync configurations are available in the `RsyncOptions` struct.

To enable the detailed rsync output set the `progress` option to `true`.

Some configurations like `--rsh` and `--rsync-path` need different type of usage based on their usage (nested or direct with the `Run()` method).
To change the behaviour set the `MergeRsyncOptions` as described for the `--rsh` command with the value of `ssh -p 22000 -l root`:

| MergeRsyncOptions Value | Arguments List | Use Case |
| --- | --- | --- |
| `true` | `[..., --rsh='ssh -p 22000 -l root', ... ]` | Use in nested use case |
| `false` | `[..., --rsh, ssh -p 22000 -l root, ...]` | Use in direct use case with the `Run()` method |

## Usage

To start the rsync process call the `Run()` method.

If you only interested in the list of arguments and run the command yourself (e.g. as nested command in a ssh command like in the [gssh](https://github.com/schnes/gssh) lib) you could run the `GetFullArguments()` method.

## Example

```golang
package main

import (
	"fmt"
	"github.com/schnes4/grsync"
)

func main() {
	rsyncOptions := grsync.RsyncOptions{
		Archive:       true,
		Executability: true,
		Recursive:     true,
		Progress:      true,
		Delete:        true,
		//...
	}

	rsync := grsync.NewRsync("source", "destination", rsyncOptions)

	argList := rsync.GetFullArguments()
	fmt.Println(argList)

	rsync.Run()
}
```
