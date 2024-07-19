+++
title = 'Getting started'
date = 2024-07-13T00:33:02+07:00
draft = true
weight = 100
+++

## Introduction

Gobs control how an application will start. It manage all service instances in 5 life-cycles: `init`, `setup`, `start`, `stop`. For these characteristics, we need to know:
1. [How to create and run the Gobs](/docs/getting-started/create-run-gobs)
1. [How to setup a service instance and put to gobs](/docs/getting-started/build-gobs-service)

But first, let's install the Gobs to your project.

## Installation

```bash
go get github.com/xarest/gobs
```

## Quick start

```golang
package main

import (
  "context"
	"syscall"

	"github.com/traphamxuan/gobs"
	gUtils "github.com/traphamxuan/gobs/utils"
)

type API struct {}

func main() {
  ctx := context.Background()
  bs := gobs.NewBootstrap()
  bs.AddOrPanic(&API{})
  bs.StartBootstrap(ctx)
}
```
For the details of each step, please go to [create and run the Gobs](/docs/getting-started/create-run-gobs)
