+++
title = 'ServiceLifeCycle'
date = 2024-07-13T02:34:02+07:00
draft = false
weight = 1
+++

```go {style=tokyonight-night}
type ServiceLifeCycle struct {
	Deps        Dependencies
	ExtraDeps   []CustomService
	AsyncMode   map[common.ServiceStatus]bool
	AfterInit   func(ctx context.Context, deps Dependencies) error
	OnInterrupt func(ctx conntext.Context, reasons []int)
}
```
1. `Deps Dependencies`: List of services that the service is depended on

Example:
```go {style=tokyonight-night,filename=api.go}
func (a *API) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return &gobs.ServiceLifeCycle{
    Deps: []interface{
      &log.Log{},
      &conf.Config{},
    }
  }, nil
}
```
2. `ExtraDeps []CustomService`: List of services inform of extra information. It is used to make the service depend on the other services which are not the same with the ones existed in the gobs.

Example: API requires 2 independent instances of Log and Config without reuse the instances which are commonly shared in the gobs
```go {style=tokyonight-night,filename=api.go}
import gCommon "github.com/traphamxuan/gobs/common"

func (a *API) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return &gobs.ServiceLifeCycle{
    ExtraDeps: []gobs.CustomService{
      {
        Name:    "Log-2",
        Instance: &log.Log{}
      },
      {
        Name:    "Cfg-2",
        Instance: &cfg,
        Status:   gCommon.StatusSetup,
      },
    }
  }, nil
}
```
3. `AsyncMode`: Define stages of a service work in async mode. It is useful to not pending other services when this service is waiting for another processes.

Example: API has `Start` method which should run in async mode. This configuration will help other instannces who does not depend on API can run without waiting for API to finish the `Start` stage.
```go {style=tokyonight-night,filename=api.go}
import gCommon "github.com/traphamxuan/gobs/common"

func (a *API) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return &gobs.ServiceLifeCycle{
		AsyncMode: map[gCommon.ServiceStatus]bool{
			gCommon.StatusStart: true,
		},
  }, nil
}
```
4. `AfterInit`: Callback after `Init` of service run successfully, when all of its dependencies are ready to be injected. Usually used for filling internal fields of Service struct

Example: API depends on log & config instances. When both of them finish `init` step, `AfterInit` of API is called to load those instances into API instance.
```go {style=tokyonight-night,filename=api.go}
func (a *API) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return &gobs.ServiceLifeCycle{
    Deps: []interface{
      &log.Log{},
      &conf.Config{},
    },
    AfterInit: func(ctx context.Context, deps Dependencies) error {
      a.log = deps[0]
      a.cfg = deps[1]
    }
  }, nil
}
```

5. `OnInterrupt`: Callback when gobs is interrupted at `start` state and it is going to be stopped. This callback is critical thus logic inside must be execute as quick as possible. Don't try any blocking execution inside this method

Example: Log will show warning when a signal interrupt gobs
```go {style=tokyonight-night,filename=log.go}
func (l *Log) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return &gobs.ServiceLifeCycle{
    OnInterrupt: func(ctx context.Context, reasons []int) {
      l.Warn("The appication has been interrupted")
    }
  }, nil
}
```