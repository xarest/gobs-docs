+++
title = '1 instance'
date = 2024-07-13T02:31:50+07:00
draft = false
weight = 151
slug = '1-instance'
+++

# Sample
At repository: https://github.com/xarest/gobs/tree/main/samples/1_instance

The sample describe the basic usage to initiate and run an instance using gobs.

```go {style=tokyonight-night,filename=service.go}
package main

import (
	"context"

	"github.com/traphamxuan/gobs"
)

type Service struct{}

var _ gobs.ServiceInit = (*Service)(nil)
func (a *Service) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  fmt.Println("Service Init")
	return nil, nil
}

var _ gobs.ServiceSetup = (*Service)(nil)
func (a *Service) Setup(ctx context.Context) error {
  fmt.Println("Service Setup")
	return nil
}

var _ gobs.ServiceStart = (*Service)(nil)
func (a *Service) Start(ctx context.Context) error {
  fmt.Println("Service Start")
	return nil
}

var _ gobs.ServiceInit = (*Service)(nil)
func (a *Service) Stop(ctx context.Context) {
  fmt.Println("Service Stop")
}

```
```go {style=tokyonight-night,filename=main.go}
package main

import (
	"context"

	"github.com/traphamxuan/gobs"
)

func main() {
	ctx := context.Background()
	bs := gobs.NewBootstrap()
	bs.AddOrPanic(&Service{})
	bs.Start(ctx)
}

```
# Output
```bash
Service Init
Service Setup
Service Start
Service Stop
```