+++
title = 'Golang Bootstrap'
date = 2024-07-13T00:33:02+07:00
description = "Open source projects powered by Xarest"
draft = false
toc = false
wide = 100
+++

##  Manage complex project structure with GoBs

> All service instances only work when their dependencies are ready.
> All service instances onnly stop when the ones depends on them are finished.

{{< cards >}}
  {{< card link="#fast-booting-sequence" title="Fast" icon="rocket" subtitle="Gobs optimizes booting flows via multi-threading. All services will start immediately when their dependencies are ready" >}}
  {{< card link="#warranty-application-life-cycles" title="Safe" icon="guard" subtitle="Gobs resolves dependencies between services by producer-consumer pattern. All services will be ordered so that system is always finite states">}}
  {{< card link="#flexible-adaptor" title="Light-weight" icon="clear" subtitle="Only standard libraries of Golang with clear architecture are used. Each services can implement without changing original logic">}}
  {{< card link="#light-weight-libraries" title="Flexible" icon="flexible" subtitle="Compatible with almost routing framework such as gin, echo, fiber" >}}
{{< /cards >}}


## What is Gobs?

Gobs is the framework optimizing boot sequence of large applications written in Golang. With Gobs, developers can scale their application logics easily without caring about the dependencies and life-cylces of the services at booting stage.

## Why Gobs?

Golang is commonly used for micro-services. Each of them is a separated module working independently. This approach creates a barrier preventing Golang from the big applications with complicated features. Go is easy to maintain as small module. When the application grows bigger application based on Go will get some obstacles:

#### Long-scripts for initialization
When the application has been integrated with numerous features, this snippet is commonly going be lengthy.
```go {style=tokyonight-night}
	logger := logger.NewLogger(env)
	if err != nil {
		return err
	}
	o := orm.NewOrm(ctx, dbInfo, s.Logger, s.SStore)
	if err != nil {
		return err
	}
	engine := gin.Default()
	mw := middleware.NewMiddleware(o, rd, logger, sim)
	buildHandler := builder.NewBuilder(base, o)
	gameHandler := game.NewGame(base, sim)
	profileHandler := profile.NewProfile(base)
```
They require various style to handle the initialization steps which makes the code messy. Additionaly, handling and transfering the instances which have been created to the other initializations is another challenge that prevents the application from growing bigger.

#### Time connsuming on bootstrap the application
In the snippet in previous [section](#long-scripts-for-initialization), although Go supports greatly concurrent processes, the initial steps is simply a lengthy script. Some intial steps may not relate to each others but they have to wait. If Go is the main part of a big application, the bootstrap requires numerous services will take lots of time.

#### Defined states when the app is interrupted by external events
With the manual management for all instances created in previous [section](#long-scripts-for-initialization), all instances do not have proper processing for every events happen to the application. For example, router framework (gin, echo) can have safe shutting down to help a request finish before closing the server. Meanwhile, there are no safe place for sqs, s3, redis, db to close the connection and tell those services to release the resources taken by Go application at the begining.
Event worse, defining a task to re-load redis before going live is another challenge for bootstrap time when the application had been shutdown accidentally before

## How Gobs solve the problem?

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
