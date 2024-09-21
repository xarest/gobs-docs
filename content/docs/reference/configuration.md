+++
title = 'Configuration'
date = 2024-07-13T02:35:16+07:00
draft = false
weight = 3
+++
Gobs can be configured at begining with some information:
- Startup modes (parallel or simultaneous)
- Runtime modes (cli or server)
- Log
- Time for shutting down


```go {style=tokyonight-night}
type Config struct {
	NumOfConcurrencies  int
	Logger              logger.GLog
	EnableLogDetail     bool
	IsServerMode        bool
	MaxDelayForShutdown time.Duration
}
```

#### NumOfConcurrencies - Startup modes
Define the number of threads using for services startup processes.
- `0`: gobs will run in simultaneous mode. All services will be arranged to be run one by one.
- `-1`: gobs will allocates goroutine for async services without any limitations.
- `n`: gobs will allocates goroutine for async services under the control which is not more than N services running at the same time.
{{< callout type="warning" >}}
  All services which is not set as async mode in `Init()` step will run simultaneously even if `NumOfConcurrencies = -1`
{{< /callout >}}

#### RunMode - Runtime mode
Define the way that gobs handles the application after all services started successfully
- `CLI`: Gobs triggers `Stop()` automatically after all services finished starting processes.
- `Server`: Gobs hold in pending state after all services finished starting processes. Gobs only triggers `Stop()` by internal or external signals which fire the `Interrupt()`

#### Log
Log interface used to log information inside Gobs. There are 2 modes of logging:
- `Logger != nil` Log the status of services when gobs run them
- `EnableLogDetail = true` Log the detail for each processes inside. Used for debugging if gobs had any errors.

#### MaxDelayForShutdown
Gobs exits immeditately after the deadline in `MaxDelayForShutdown`. The deadline is counted from the time gobs receive got the signals or finished all `Start()`.
- `0` means unlimited time for shutting down.
{{< callout type="warning" >}}
  Don't try to make anything costly in the `Interrupt()` to avoid run out of time when services are trying to run `Stop()`.
{{< /callout >}}
