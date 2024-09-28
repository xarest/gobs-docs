+++
title = 'Bootstrap'
date = 2024-09-28T02:34:02+07:00
draft = false
weight = 220
+++

This section will present the flow of starting up of an application writing in Golang. The good application should be able to:
- Handle the main logic features in long running processes
- Response to the external signals (user or system interrupt)
- Response to the internal signals (error or corrupt from internal components)

In reality, thoes non-functional requirements is set in the main function (`main.go`). Fortunately, Go with goroutine support us to adapt them. Let's get into the flow of starting an application in Golang _using Gobs_.

{{< cards >}}
  {{< card link="./init/" title="Init" icon="rocket" subtitle="Initialize an instance to handle the bootstrap flow" >}}
  {{< card link="./setup/" title="Setup" icon="guard" subtitle="Set environment for the components setup">}}
  {{< card link="./start/" title="Start" icon="clear" subtitle="Handle CLI mode or Server mode after starting process">}}
  {{< card link="./interrupt/" title="Interrupt" icon="clear" subtitle="Response to the external and internal signals">}}
  {{< card link="./stop/" title="Stop" icon="clear" subtitle="Holding for final works before exiting">}}
{{< /cards >}}
