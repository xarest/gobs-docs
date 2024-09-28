+++
title = 'Setup'
date = 2024-07-13T02:34:23+07:00
draft = true
weight = 222
+++

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
