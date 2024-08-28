+++
title = 'Create and run gobs'
date = 2024-07-13T02:31:27+07:00
draft = false
weight = 101
slug = 'gobs-instance'
+++

#### Create gobs instance

```golang
bs := gobs.NewBootstrap()
```
This statement create an instance of gobs which will manage all other instances added to it. If your application have more than one independent tasks, multiple gobs instannces may be considered.

#### Add services which need to be started to gobs
```golang
bs.AddOrPanic(&api.API{})
```
As its name, this statement create an instance of `API` then put it into the gobs. Usually, this method quite simple and won't be able to crash. However, we would throw panic if it got a nil instance.

#### Create trigger to stop gobs
```golang
_, canncelTrigger := bs.AddTrigger(gUtils.DefautOSSignal(syscall.SIGINT, syscall.SIGTERM))
defer cancelTrigger()
```
`AddTrigger` will register a method to stop the gobs when it is running. In this case, it add `syscall.SIGINT` and `syscall.SIGTERM` into gobs so that when the os trigger an interrupt, ctrl+C from the terminal for instance, gobs will interrupt its running processes and end with stop state.

#### Run gobs
```golang
bs.Start(appCtx)
```
This method will run all `Start(ctx)` method of all instances added into gobs and their dependencies also. All are in order of start. The last one hold pending state as a server will pending the `bs.Start(ctx)` too. This method only returns when all instances have been stopped properly.