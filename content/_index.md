+++
title = 'Golang Bootstrap'
date = 2024-07-13T00:33:02+07:00
description = "Open source projects powered by Xarest"
draft = true
toc = false
wide = 100
+++

##  Manage complex project structure with GoBs

{{< cards >}}
  {{< card link="#fast-booting-sequence" title="Fast" icon="rocket" subtitle="Gobs optimizes booting flows via multi-threading. All services will start immediately when their dependencies are ready" >}}
  {{< card link="#warranty-application-life-cycles" title="Safe" icon="guard" subtitle="Gobs resolves dependencies between services by producer-consumer pattern. All services will be ordered so that system is always finite states">}}
  {{< card link="#flexible-adaptor" title="Light-weight" icon="clear" subtitle="Only standard libraries of Golang with clear architecture are used. Each services can implement without changing original logic">}}
  {{< card link="#light-weight-libraries" title="Flexible" icon="flexible" subtitle="Compatible with almost routing framework such as gin, echo, fiber" >}}
{{< /cards >}}


## What is Gobs?

Gobs is the framework optimizing boot sequence of large applications written in Golang. With Gobs, developers can scale their application logics easily wihtout caring about the dependencies and life-cylces of the services at booting stage.

## Why Gobs?

Gobs provides:

#### **Fast booting sequence**

All services registered with Gobs will be managed in parallel executions. The services will be executed immediately when their dependencies are ready without pending for any other instances.

#### **Warranty application life-cycles**

<!-- With the complex dependencies network of large applications, defining the states of a service when one of them in system failed may be painful. In worst cases, a service may cause unexpected actions when the system has ben failing.  -->
All instances of service will be managed in 4 states: `init`, `setup`, `start`, `stop`. Gobs makes a plan for all services in ordered. Thus no service will be executed in the undefined states or making any unexpected actions when its dependencies failed.

#### **Flexible adaptor**

4 life-cycle states of a services are optional. It can be implemented without changing current process of the original business logic. Gobs is tested properly with common framework such as gin, echo, fiber.
Gobs provides some best practices in project structure for easily managing and scaling various common services.

#### **Light-weight libraries**

Apart from standard libraries of Golang, no 3rd party library has been implemented. Gobs is built and managed with enthusiast of Gophers.
