+++
title = 'Build Gobs service'
date = 2024-07-13T02:31:27+07:00
draft = false
weight = 102
slug = 'gobs-service'
+++

A service instance managed by gobs has 4 stages of its lifecycle: `init`, `setup`, `start`, `stop`. Users can optionally custom those step for particular services. Let's go to functionalities of each stages

### Init service

Initialization stage is used for setting up dependencies of a service.

{{< tabs items="Quick,Common,Advanced" >}}
{{< tab >}}
```go {style=tokyonight-night}
type API struct {
  log *log.Log
  cfg *conf.Config
}
var _ gobs.InitService = (*API)(nil)

// set dependencies of API are log.Log and conf.Config
func (a *API) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return gobs.GenerateSetupConfig(&a.log, &a.cfg)
}
```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
type API struct {
  log *log.Log
  cfg *conf.Config
}
var _ gobs.InitService = (*API)(nil)

// set dependencies of API are log.Log and conf.Config
func (a *API) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return &gobs.ServiceLifeCycle{
    Deps: []interface{
      &log.Log{},
      &conf.Config{},
    },
    OnSetup: func(ctx context.Context, deps Dependencies) error {
      return deps.Assign(&a.log, &a.cfg)
    }
  }, nil
}
```
{{< /tab >}}
{{< tab >}}
```go {style=tokyonight-night}
type API struct {
  log *log.Log
  cfg *conf.Config
}
var _ gobs.InitService = (*API)(nil)

// set dependencies of API are log.Log and conf.Config
// and the conf.Config is different from common shared conf.Config
func (a *API) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return &gobs.ServiceLifeCycle{
    Deps: []interface{
      &log.Log{},
    },
    ExtraDeps: []gobs.CustomService{
      Name: "Configuration 2",
      Service: &conf.Config{},
    },
    OnSetup: func(ctx context.Context, deps Dependencies) error {
      a.log = deps[0]
      a.cfg = deps[1]
    }
  }, nil
}
```
{{< /tab >}}
{{< /tabs >}}

With the configuration above, API service depends on 2 services: `Log` and `Config`. At `init` stage, gobs will try to find instances of `Log` and `Config` in gobs sharing pool to return as `deps` parameter on `OnSetup` callback.

All next stages of `API` service will follow the dependencies set up at `init` stage.

### Setup service

Setup stage is used for configuring of a service before starting.

```go {style=tokyonight-night}
type API struct {
  log *log.Log
  cfg *conf.Config
}
var _ gobs.SetupService = (*API)(nil)

func (a *API) Setup(ctx context.Context) error {
  a.log.Info("API is setup")
  return a.cfg.Config()
}
```

When API implements method `Setup` like `gobs.SetupService`, gobs will manage to run `a.Setup` after `log.Log` and `conf.Config` finish their `Setup`.

{{< callout type="info" >}}
  If `log.Log` or `conf.Config` failed in running `Setup`, the setup process will stop immediately and `Setup` of API won't be invoked
{{< /callout >}}


### Start service

Start stage is used for running main process of a service.

```go {style=tokyonight-night}
type API struct {
  log *log.Log
  cfg *conf.Config
}
var _ gobs.StartService = (*API)(nil)

func (a *API) Start(ctx context.Context) error {
  a.log.Info("API is running")
  return a.Run()
}
```

When API implements method `Start` like `gobs.StartService`, gobs will manage to run `a.Start` after `log.Log` and `conf.Config` finish their `Start`

{{< callout type="warning" >}}
  API will never start if `log` or `cfg` is pending without finishing `Start` method
{{< /callout >}}
{{< callout type="info" >}}
  If `log.Log` or `conf.Config` failed in running `Setup`, the setup process will stop immediately and `Setup` of API won't be invoked
{{< /callout >}}

### Stop service

Start stage is used to do the last execution and release resources before existing the application.

```go {style=tokyonight-night}
type API struct {
  log *log.Log
  cfg *conf.Config
}
var _ gobs.StopService = (*API)(nil)

func (a *API) Stop(ctx context.Context) {
  a.log.Info("API is stopped")
  a.cfg.Release()
}
```

`Stop` flow is in reverse order to `Setup` and `Start` flow. `Setup` method of `conf.Config` and `log.Log` will be called after `Stop` of `API` finish its works. gobs only invoke `Stop` of the services which had finished their `Setup` stage.

{{< callout type="info" >}}
  `Stop` process will run automatically if `Setup` or `Start` fail
{{< /callout >}}

### Complete
```go {style=tokyonight-night}
type API struct {
  log *log.Log
  cfg *conf.Config
}

var _ gobs.InitService = (*API)(nil)
func (a *API) Init(ctx context.Context) (*gobs.ServiceLifeCycle, error) {
  return gobs.GenerateSetupConfig(&a.log, &a.cfg)
}

var _ gobs.SetupService = (*API)(nil)
func (a *API) Setup(ctx context.Context) error {
  a.log.Info("API is setup")
  return a.cfg.Config()
}

var _ gobs.StartService = (*API)(nil)
func (a *API) Start(ctx context.Context) error {
  a.log.Info("API is running")
  return a.Run()
}

var _ gobs.StopService = (*API)(nil)
func (a *API) Stop(ctx context.Context) {
  a.log.Info("API is stopped")
  a.cfg.Release()
}
```

## Bonus
If your service is simple you can ignore `Init` method and setup your instance as common pattern below
```go {style=tokyonight-night,filename=api.go}
type API struct {
  log *log.Log
  cfg *conf.Config
}

func NewAPI(log *log.Log, cfg *conf.Config) *API {
  return &API{
    log: log,
    cfg: cfg,
  }
}

var _ gobs.SetupService = (*API)(nil)
func (a *API) Setup(ctx context.Context) error {
  a.log.Info("API is setup")
  return a.cfg.Config()
}

var _ gobs.StartService = (*API)(nil)
func (a *API) Start(ctx context.Context) error {
  a.log.Info("API is running")
  return a.Run()
}

var _ gobs.StopService = (*API)(nil)
func (a *API) Stop(ctx context.Context) {
  a.log.Info("API is stopped")
  a.cfg.Release()
}
```

Then the creation of gobs instance will be
```go {style=tokyonight-night,filename=main.go}
func main() {
  ctx := context.Background()
  bs := gobs.NewBootstrap()
  cfg := NewConfig()
  log := NewLog()
  api := NewAPI(log, cfg)
  bs.AddOrPanic(cfg)
  bs.AddOrPanic(log)
  bs.AddOrPanic(api)
  bs.Start(ctx)
}
```
