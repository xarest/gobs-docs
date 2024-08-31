+++
title = '3 instances'
date = 2024-07-13T02:31:50+07:00
draft = false
weight = 152
slug = '3-instances'
+++

# Sample
At repository: https://github.com/xarest/gobs/tree/main/samples/3_instances

The sample describe the usages to initiate and run 3 separate instances using gobs to manage their life-cycles.

{{< tabs items="A,B,C" >}}
{{< tab >}}
```go {style=tokyonight-night,filename=a.go}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type A struct{}

var _ gobs.IServiceInit = (*A)(nil)

func (s A) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("A Init")
	return nil, nil
}

var _ gobs.IServiceSetup = (*A)(nil)

func (s A) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("A Setup")
	return nil
}

var _ gobs.IServiceStart = (*A)(nil)

func (s A) Start(ctx context.Context) error {
	fmt.Println("A Start")
	return nil
}

var _ gobs.IServiceStop = (*A)(nil)

func (s A) Stop(ctx context.Context) error {
	fmt.Println("A Stop")
	return nil
}

```
{{< /tab >}}

{{< tab >}}
```go {style=tokyonight-night,filename=a.go}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type B struct{}

var _ gobs.IServiceInit = (*B)(nil)

func (s B) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("B Init")
	return nil, nil
}

var _ gobs.IServiceSetup = (*B)(nil)

func (s B) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("B Setup")
	return nil
}

var _ gobs.IServiceStart = (*B)(nil)

func (s B) Start(ctx context.Context) error {
	fmt.Println("B Start")
	return nil
}

var _ gobs.IServiceStop = (*B)(nil)

func (s B) Stop(ctx context.Context) error {
	fmt.Println("B Stop")
	return nil
}

```
{{< /tab >}}


{{< tab >}}
```go {style=tokyonight-night,filename=a.go}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type C struct{}

var _ gobs.IServiceInit = (*C)(nil)

func (s C) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("C Init")
	return nil, nil
}

var _ gobs.IServiceSetup = (*C)(nil)

func (s C) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("C Setup")
	return nil
}

var _ gobs.IServiceStart = (*C)(nil)

func (s C) Start(ctx context.Context) error {
	fmt.Println("C Start")
	return nil
}

var _ gobs.IServiceStop = (*C)(nil)

func (s C) Stop(ctx context.Context) error {
	fmt.Println("C Stop")
	return nil
}

```
{{< /tab >}}
{{< /tabs >}}


```go {style=tokyonight-night,filename=main.go}
package main

import (
	"context"
	"sample/services"
	"syscall"

	"github.com/xarest/gobs"
)

func main() {
	ctx := context.Background()
	bs := gobs.NewBootstrap()
	bs.AddMany(&services.A{}, &services.B{}, &services.C{})
	bs.StartBootstrap(ctx, syscall.SIGINT, syscall.SIGTERM)
}

```
# Output
```bash
A Init
B Init
C Init

A Setup
B Setup
C Setup

A Start
B Start
C Start

A Stop
B Stop
C Stop
```