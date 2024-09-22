+++
title = 'Bootstrap'
date = 2024-07-13T02:34:23+07:00
draft = true
weight = 2
+++

Gobs manages resources of the applicaiton by an instance from `gobs.NewBootstrap()`. A typical application generally requires only a single instance of gobs. If you application is complex, various instances can be used to manage multiple processes at the same time.

The initialization may accept some [configurations](../configuration/) to define the way gobs will run.

```go {style=tokyonight-night,filename=single.go}
bs := gobs.NewBootstrap()
```

A gobs instance has life-cycle similar with a service including `Init(), Setup(), Start(), Stop()`. The diffenencies lie in the way we handle it. The service life-cycles depend on the envents managed by gobs. Meanwhile, gobs instance is called directly in main context to change these stages.

### Init()
Initial step is used to set up the schedule plan to bring-up all service instances. The init step requires 2 parts: Adding services to gobs instance and build schedule plan for services and all of their dependencies.
```go {style=tokyonight-night,filename=single.go}
// Add services to gobs
bs.AddOrPanic(&Log{})
bs.AddOrPanic(&API{})

// Build bootstrap plan
if err := bs.Init(); err != nil {
  panic(err)
}
```

#### Adding services into gobs instance
There are 2 things that we should take care when adding a service into gobs. The key identifier and the status
```go {style=tokyonight-night}
Add(s IService, status common.ServiceStatus, key string) error
```
##### status
Gobs manages all service instances following fixed status values as mentioned at [services](../service/). 
Mostly, the service instance status should start with `StatusUninitialized`.
```go {style=tokyonight-night}
bs.Add(&Log{}, common.StatusUninitialized, "logkey")
bs.AddMany(&Log{}, &API{})
```
If we want to add an instance which has already been setup or start somewhere else, the correct status should be added
```go {style=tokyonight-night}
log := NewLog()
bs.Add(&log, common.StatusStart, "logkey")
```

##### Identifier - key
Each service instance in gobs is defined by a unique key. Gobs will check the key when new instances added. If it's existed, the old instance will be overwritten by the new ones.

To ease the way managing keys for all service instances, gobs has a default convention: If an instance is added without defining the name, its location will be its key.
```go {style=tokyonight-night}
import "myapp/log"

bs.AddDefault(&log.Log{}) // The key is myapp/log.Log
bs.AddOrPanic(&log.Log{}) // The key is myapp/log.Log
```
This is the default name that gobs uses to identify the instances it found in dependencies in `Init()` of the [ServiceLifeCycle](../service/01_service-life-cycle/).

#### Build bringup schedule
When `bs.Init()` called, gobs will invoke `Init()` of all instances added into gobs. If the `Init()` in the instance return dependencies, gobs will keep going until all instances are in order.

### Setup()
Calling `Setup()` of gobs will start running all `Setup()` in service instances. This method may take time to complete and requires goroutine. Thus the best practice is putting this process in the background so that the main context can handle following external or internal signals
```go {style=tokyonight-night}
go func(ctx context.Context) {
  if err := bs.Setup(ctx); err != nil {
    return err
  }
  // Handle error here
}(ctx)
// Waiting for interrupt signals here
```
### Start()
Calling `Start()` of gobs will start running all `Start()` and `StartServer()` in service instances. This method may take time to complete 
### Interrupt()
### Stop()
### StartBootstrap()
