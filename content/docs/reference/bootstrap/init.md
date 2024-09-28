+++
title = 'Init'
date = 2024-07-13T02:34:23+07:00
draft = false
weight = 221
+++

Initialization of Go application is the first step to define what components that the application will have. For example, most of applications must have log and configuration loader.

Note that this step is just making the definitions for those components in order to set up a boot up plan. No blocking action happens. The external signals to stop the application won't be available at this step.

## Using Gobs to manage the initialization

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
