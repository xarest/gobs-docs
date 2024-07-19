+++
title = 'Create and run gobs'
date = 2024-07-13T02:31:27+07:00
draft = true
weight = 1
+++

#### Create gobs instance

```golang
bs := gobs.NewBootstrap()
```

#### Add services which need to be started to gobs
```golang
bs.AddOrPanic(&api.API{})
```

#### Create trigger to stop gobs
```golang
quit := make(chan os.Signal, 2)
signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
defer close(quit)
trigger := gUtils.WrapChannel[os.Signal, struct{}](appCtx, quit)
```

#### Run gobs
```golang
bs.StartBootstrap(appCtx, trigger)
```
