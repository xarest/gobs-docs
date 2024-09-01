+++
title = '9 instaces with asynchronous processes'
date = 2024-07-13T02:31:50+07:00
draft = false
weight = 154
slug = 'complex-deps-async'
+++

# Sample
At repository: https://github.com/xarest/gobs/tree/main/samples/9_instances

The sample describe the usages to initiate and run 9 instances related with each others using gobs to manage their life-cycles and arrange to bootstrap in correct order without blocking from some async processes.

`S6`, `S7`, `S9`, `S10`, `S13` are the servvices contain asynchronous processes in `Setup` and `Stop`

> - All service instances only work when their dependencies are ready.
> - All service instances only stop when the ones depends on them are finished.

![](gobs-run-13-instances-async.gif "Flow of init/setup/start/stop service instances using Gobs")

## Services
{{< tabs items="S1,S2,S3,S4,S5,S6*,S7*,S8,S9*,S10*,S11,S12,S13*" >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type S1 struct {
	s2 *S2
	s3 *S3
}

var _ gobs.IServiceInit = (*S1)(nil)

func (s *S1) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S1 init")
	return &gobs.ServiceLifeCycle{
		Deps: gobs.Dependencies{new(S2), new(S3)},
	}, nil
}

var _ gobs.IServiceSetup = (*S1)(nil)

func (s *S1) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("S1 setup")
	if err := deps.Assign(&s.s2, &s.s3); err != nil {
		fmt.Println("Failed to assign dependencies", err)
		return err
	}
	return nil
}

var _ gobs.IServiceStart = (*S1)(nil)

func (s *S1) Start(ctx context.Context) error {
	fmt.Println("S1 start")
	return nil
}

var _ gobs.IServiceStop = (*S1)(nil)

func (s *S1) Stop(ctx context.Context) error {
	fmt.Println("S1 stop")
	return nil
}
```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type S2 struct {
	s4 *S4
	s5 *S5
}

var _ gobs.IServiceInit = (*S1)(nil)

func (s *S2) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S2 init")
	return &gobs.ServiceLifeCycle{
		Deps: gobs.Dependencies{new(S4), new(S5)},
	}, nil
}

var _ gobs.IServiceSetup = (*S1)(nil)

func (s *S2) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("S2 setup")
	if err := deps.Assign(&s.s4, &s.s5); err != nil {
		fmt.Println("Failed to assign dependencies", err)
		return err
	}
	return nil
}

var _ gobs.IServiceStart = (*S1)(nil)

func (s *S2) Start(ctx context.Context) error {
	fmt.Println("S2 start")
	return nil
}

var _ gobs.IServiceStop = (*S1)(nil)

func (s *S2) Stop(ctx context.Context) error {
	fmt.Println("S2 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type S3 struct {
	s6 *S6
	s7 *S7
	s8 *S8
}

var _ gobs.IServiceInit = (*S1)(nil)

func (s *S3) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S3 init")
	return &gobs.ServiceLifeCycle{
		Deps: gobs.Dependencies{new(S6), new(S7), new(S8)},
	}, nil
}

var _ gobs.IServiceSetup = (*S1)(nil)

func (s *S3) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("S3 setup")
	if err := deps.Assign(&s.s6, &s.s7, &s.s8); err != nil {
		fmt.Println("Failed to assign dependencies", err)
		return err
	}
	return nil
}

var _ gobs.IServiceStart = (*S1)(nil)

func (s *S3) Start(ctx context.Context) error {
	fmt.Println("S3 start")
	return nil
}

var _ gobs.IServiceStop = (*S1)(nil)

func (s *S3) Stop(ctx context.Context) error {
	fmt.Println("S3 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type S4 struct {
	s9  *S9
	s10 *S10
}

var _ gobs.IServiceInit = (*S4)(nil)

func (s *S4) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S4 init")
	return &gobs.ServiceLifeCycle{
		Deps: gobs.Dependencies{new(S9), new(S10)},
	}, nil
}

var _ gobs.IServiceSetup = (*S4)(nil)

func (s *S4) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("S4 setup")
	if err := deps.Assign(&s.s9, &s.s10); err != nil {
		fmt.Println("Failed to assign dependencies", err)
		return err
	}
	return nil
}

var _ gobs.IServiceStart = (*S4)(nil)

func (s *S4) Start(ctx context.Context) error {
	fmt.Println("S4 start")
	return nil
}

var _ gobs.IServiceStop = (*S4)(nil)

func (s *S4) Stop(ctx context.Context) error {
	fmt.Println("S4 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type S5 struct {
	s9  *S9
	s10 *S10
	s11 *S11
}

var _ gobs.IServiceInit = (*S5)(nil)

func (s *S5) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S5 init")
	return &gobs.ServiceLifeCycle{
		Deps: gobs.Dependencies{new(S9), new(S10), new(S11)},
	}, nil
}

var _ gobs.IServiceSetup = (*S5)(nil)

func (s *S5) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("S5 setup")
	if err := deps.Assign(&s.s9, &s.s10, &s.s11); err != nil {
		fmt.Println("Failed to assign dependencies", err)
		return err
	}
	return nil
}

var _ gobs.IServiceStart = (*S5)(nil)

func (s *S5) Start(ctx context.Context) error {
	fmt.Println("S5 start")
	return nil
}

var _ gobs.IServiceStop = (*S5)(nil)

func (s *S5) Stop(ctx context.Context) error {
	fmt.Println("S5 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"
	"time"

	"github.com/xarest/gobs"
	"github.com/xarest/gobs/common"
)

type S6 struct {
	s10 *S10
	s11 *S11
}

var _ gobs.IServiceInit = (*S6)(nil)

func (s *S6) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S6 init")
	return &gobs.ServiceLifeCycle{
		Deps: gobs.Dependencies{new(S10), new(S11)},
		AsyncMode: map[common.ServiceStatus]bool{
			common.StatusSetup: true,
			common.StatusStop:  true,
		},
	}, nil
}

var _ gobs.IServiceSetup = (*S6)(nil)

func (s *S6) Setup(ctx context.Context, deps gobs.Dependencies) error {
	time.Sleep(30 * time.Millisecond)
	fmt.Println("S6 setup")
	if err := deps.Assign(&s.s10, &s.s11); err != nil {
		fmt.Println("Failed to assign dependencies", err)
		return err
	}
	return nil
}

var _ gobs.IServiceStart = (*S6)(nil)

func (s *S6) Start(ctx context.Context) error {
	fmt.Println("S6 start")
	return nil
}

var _ gobs.IServiceStop = (*S6)(nil)

func (s *S6) Stop(ctx context.Context) error {
	time.Sleep(30 * time.Millisecond)
	fmt.Println("S6 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"
	"time"

	"github.com/xarest/gobs"
	"github.com/xarest/gobs/common"
)

type S7 struct {
	s12 *S12
}

var _ gobs.IServiceInit = (*S7)(nil)

func (s *S7) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S7 init")
	return &gobs.ServiceLifeCycle{
		Deps: gobs.Dependencies{new(S12)},
		AsyncMode: map[common.ServiceStatus]bool{
			common.StatusSetup: true,
			common.StatusStop:  true,
		},
	}, nil
}

var _ gobs.IServiceSetup = (*S7)(nil)

func (s *S7) Setup(ctx context.Context, deps gobs.Dependencies) error {
	time.Sleep(50 * time.Millisecond)
	fmt.Println("S7 setup")
	if err := deps.Assign(&s.s12); err != nil {
		fmt.Println("Failed to assign dependencies", err)
		return err
	}
	return nil
}

var _ gobs.IServiceStart = (*S7)(nil)

func (s *S7) Start(ctx context.Context) error {
	fmt.Println("S7 start")
	return nil
}

var _ gobs.IServiceStop = (*S7)(nil)

func (s *S7) Stop(ctx context.Context) error {
	time.Sleep(50 * time.Millisecond)
	fmt.Println("S7 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type S8 struct {
	s13 *S13
}

var _ gobs.IServiceInit = (*S8)(nil)

func (s *S8) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S8 init")
	return &gobs.ServiceLifeCycle{
		Deps: gobs.Dependencies{new(S13)},
	}, nil
}

var _ gobs.IServiceSetup = (*S8)(nil)

func (s *S8) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("S8 setup")
	if err := deps.Assign(&s.s13); err != nil {
		fmt.Println("Failed to assign dependencies", err)
		return err
	}
	return nil
}

var _ gobs.IServiceStart = (*S8)(nil)

func (s *S8) Start(ctx context.Context) error {
	fmt.Println("S8 start")
	return nil
}

var _ gobs.IServiceStop = (*S8)(nil)

func (s *S8) Stop(ctx context.Context) error {
	fmt.Println("S8 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"
	"time"

	"github.com/xarest/gobs"
	"github.com/xarest/gobs/common"
)

type S9 struct{}

var _ gobs.IServiceInit = (*S9)(nil)

func (s *S9) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S9 init")
	return &gobs.ServiceLifeCycle{
		AsyncMode: map[common.ServiceStatus]bool{
			common.StatusSetup: true,
			common.StatusStop:  true,
		},
	}, nil
}

var _ gobs.IServiceSetup = (*S9)(nil)

func (s *S9) Setup(ctx context.Context, deps gobs.Dependencies) error {
	time.Sleep(100 * time.Millisecond)
	fmt.Println("S9 setup")

	return nil
}

var _ gobs.IServiceStart = (*S9)(nil)

func (s *S9) Start(ctx context.Context) error {
	fmt.Println("S9 start")
	return nil
}

var _ gobs.IServiceStop = (*S9)(nil)

func (s *S9) Stop(ctx context.Context) error {
	time.Sleep(100 * time.Millisecond)
	fmt.Println("S9 stop")
	return nil
}
```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"
	"time"

	"github.com/xarest/gobs"
	"github.com/xarest/gobs/common"
)

type S10 struct{}

var _ gobs.IServiceInit = (*S10)(nil)

func (s *S10) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S10 init")
	return &gobs.ServiceLifeCycle{
		AsyncMode: map[common.ServiceStatus]bool{
			common.StatusSetup: true,
			common.StatusStop:  true,
		},
	}, nil
}

var _ gobs.IServiceSetup = (*S10)(nil)

func (s *S10) Setup(ctx context.Context, deps gobs.Dependencies) error {
	time.Sleep(60 * time.Millisecond)
	fmt.Println("S10 setup")

	return nil
}

var _ gobs.IServiceStart = (*S10)(nil)

func (s *S10) Start(ctx context.Context) error {
	fmt.Println("S10 start")
	return nil
}

var _ gobs.IServiceStop = (*S10)(nil)

func (s *S10) Stop(ctx context.Context) error {
	time.Sleep(60 * time.Millisecond)
	fmt.Println("S10 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type S11 struct{}

var _ gobs.IServiceInit = (*S11)(nil)

func (s *S11) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S11 init")
	return nil, nil
}

var _ gobs.IServiceSetup = (*S11)(nil)

func (s *S11) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("S11 setup")

	return nil
}

var _ gobs.IServiceStart = (*S11)(nil)

func (s *S11) Start(ctx context.Context) error {
	fmt.Println("S11 start")
	return nil
}

var _ gobs.IServiceStop = (*S11)(nil)

func (s *S11) Stop(ctx context.Context) error {
	fmt.Println("S11 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"

	"github.com/xarest/gobs"
)

type S12 struct{}

var _ gobs.IServiceInit = (*S12)(nil)

func (s *S12) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S12 init")
	return nil, nil
}

var _ gobs.IServiceSetup = (*S12)(nil)

func (s *S12) Setup(ctx context.Context, deps gobs.Dependencies) error {
	fmt.Println("S12 setup")

	return nil
}

var _ gobs.IServiceStart = (*S12)(nil)

func (s *S12) Start(ctx context.Context) error {
	fmt.Println("S12 start")
	return nil
}

var _ gobs.IServiceStop = (*S12)(nil)

func (s *S12) Stop(ctx context.Context) error {
	fmt.Println("S12 stop")
	return nil
}

```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
package services

import (
	"context"
	"fmt"
	"time"

	"github.com/xarest/gobs"
	"github.com/xarest/gobs/common"
)

type S13 struct{}

var _ gobs.IServiceInit = (*S13)(nil)

func (s *S13) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
	fmt.Println("S13 init")
	return &gobs.ServiceLifeCycle{
		AsyncMode: map[common.ServiceStatus]bool{
			common.StatusSetup: true,
			common.StatusStop:  true,
		},
	}, nil
}

var _ gobs.IServiceSetup = (*S13)(nil)

func (s *S13) Setup(ctx context.Context, deps gobs.Dependencies) error {
	time.Sleep(30 * time.Millisecond)
	fmt.Println("S13 setup")

	return nil
}

var _ gobs.IServiceStart = (*S13)(nil)

func (s *S13) Start(ctx context.Context) error {
	fmt.Println("S13 start")
	return nil
}

var _ gobs.IServiceStop = (*S13)(nil)

func (s *S13) Stop(ctx context.Context) error {
	fmt.Println("S13 stop")
	return nil
}

```
{{< /tab >}}
{{< /tabs >}}

## Gobs

```go {style=tokyonight-night,filename=main.go}
package main

import (
	"context"

	"github.com/traphamxuan/gobs"
)

func main() {
	ctx := context.Background()
	bs := gobs.NewBootstrap()
	bs.AddOrPanic(&S1{})
	bs.StartBoostrap(ctx)
}

```
# Output
```bash
S1 init
S2 init
S3 init
S4 init
S5 init
S6 init
S7 init
S8 init
S9 init
S10 init
S11 init
S12 init
S13 init

S11 setup
S12 setup
S13 setup
S8 setup
S7 setup
S10 setup
S6 setup
S3 setup
S9 setup
S4 setup
S5 setup
S2 setup
S1 setup

S11 start
S12 start
S13 start
S10 start
S9 start
S7 start
S8 start
S6 start
S4 start
S5 start
S3 start
S2 start
S1 start

S1 stop
S2 stop
S3 stop
S4 stop
S5 stop
S8 stop
S13 stop
S6 stop
S11 stop
S7 stop
S12 stop
S10 stop
S9 stop
```
