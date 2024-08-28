+++
title = 'Getting started'
date = 2024-07-13T00:33:02+07:00
draft = false
weight = 100
+++

## Introduction

Gobs control how an application will start. It manage all service instances in 5 life-cycles: `init`, `setup`, `start`, `stop`. For these characteristics, we need to know:
1. [How to create and run the Gobs](/docs/getting-started/gobs-instance)
1. [How to setup a service instance and put to gobs](/docs/getting-started/gobs-service)

But first, let's install the Gobs to your project.

## Installation

```bash
go get github.com/traphamxuan/gobs
```

## Quick start


```go {style=tokyonight-night,filename=main.go}
package main

import (
	"context"
	"fmt"

	"github.com/traphamxuan/gobs"
)

type API struct{}

func (a *API) Start(ctx context.Context) error {
	fmt.Println("API started")
	return nil
}

func (a *API) Stop(ctx context.Context) error {
	fmt.Println("API stopped")
	return nil
}

func main() {
	ctx := context.Background()
	bs := gobs.NewBootstrap()
	bs.AddOrPanic(&API{})
	bs.Start(ctx)
}
```
The output of the program
```bash
API started
API stopped
```
For the details of each step, please go to [create and run the Gobs](/docs/getting-started/create-run-gobs)
