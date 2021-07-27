# grsync — golang rsync wrapper

[![Build Status](https://travis-ci.com/schnes4/go-rsync.svg)](https://travis-ci.com/schnes4/grsync)
[![codecov](https://codecov.io/gh/schnes4/go-rsync/branch/master/graph/badge.svg)](https://codecov.io/gh/schnes4/grsync)

This lib is a lightweight fork from [zloylos/grsync](https://github.com/zloylos/grsync) with focus on rsync itself and some fixes.
The stdout of the rsync command is that good to print it to stdout if necessary instead of wrapping it.

The history/fork was removed because of problems during `go get`.
The work is based on the v1.5.0 release of zloylos/grsync.

## Usage

All rsync configurations are available in the `RsyncOptions` struct.

To enable the detailed rsync output set the `progress` option to `true`.

To start the rsync process call the `Run()` method.

## Example

```golang
package main

import (
	"github.com/schnes4/grsync"
)

func main () {
	rsyncOptions := grsync.RsyncOptions{
		Archive: true,
		Executability: true,
		Recursive: true,
		Progress: true,
		Delete: true,
		//...
	}

	rsync := grsync.NewRsync("source", "destination", rsyncOptions)
	err := rsync.Run()
}
```
