+++
title = 'Setup'
date = 2024-07-13T02:34:23+07:00
draft = true
weight = 222
+++

`Setup` is the step when the components defined at [Initialization](../init) step run the very-first processes before being ready for being used by the others.
> Ex: setup connection to database, redis, message queues, query configurations from secret manager, setup tracing and monitoring to the log services, etc.

There are some requirements that this step must follow to make sure it will finished correctly:

**1. Components `setup` may require other components**. They have to be managed and distributed to correct places so that no duplication happen.

**2. The component only run when their dependencies are ready**. All components must be aligned to correct order of `setup`.
> Ex: database connection only starts when the configurations setup successfully.

**3. The setup process must stop immediately if a component failed**. When a component failed, the `setup` process must return error instead of trying to process the other to prevent the setup process from unexpected results.
> Ex: database connection is next to the configuration setup. If the configuration setup failed, the setup process would return the error immediately instead of trying to process database connection setup with invalid configurations.

## Using Gobs to manage the setup process

### Setup()
Calling `Setup()` of gobs will start running all `Setup()` in service instances. This method may take time to complete and requires goroutine. Thus the best practice is putting this process in the background so that the main context can handle following external or internal signals registered in [Interrupt](../interrupt/)
```go {style=tokyonight-night}
go func(ctx context.Context) {
  if err := bs.Setup(ctx); err != nil {
    return err
  }
  // Handle error here
}(ctx)
// Waiting for interrupt signals here
```
