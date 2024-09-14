+++
title = 'Architecture'
date = 2024-07-13T02:37:23+07:00
draft = true
weight = 301
slug = 'architecture'
+++

![](program_structure.svg "Structure of a complete api server")


{{< cards >}}
  {{< card link="architecture" title="GoBS DI Architecture" icons="https://source.unsplash.com/featured/800x600?landscape" subtitle="API in DI project structure" >}}
  {{< card 
    link="configuration"
    title="Configuration"
    icons="https://img.icons8.com/ios/100/variable.png" subtitle="Manage configurations in API server with secret and configuration services"
  >}}
  {{< card 
    link="log"
    title="Log"
    icons="/images/log.png"
    subtitle="Initialize log with monitoring services"
  >}}
  {{< card 
    link="database"
    title="Database"
    icons="/images/log.png"
    subtitle="Manage database connections and query sessions"
  >}}
  {{< card 
    link="cache"
    title="Cache"
    icons="/images/log.png"
    subtitle="Cache service for API server. In-memory, redis, etc"
  >}}
  {{< card 
    link="router"
    title="Routing framework"
    icons="/images/log.png"
    subtitle="Common routing frameworks in Golang. Echo, Gin, Fiber, etc"
  >}}
  {{< card 
    link="middleware"
    title="Middlewares"
    icons="/images/log.png"
    subtitle="Common mddlewares supporting routing for API server. Authentication, Authorization, Rate Limit, etc"
  >}}
  {{< card 
    link="validator"
    title="Validators"
    icons="/images/log.png"
    subtitle="Implement binding and validating requests for API endpoints"
  >}}
  {{< card 
    link="handler"
    title="Handlers/Controllers"
    icons="/images/log.png"
    subtitle="Manage resources for request accesses using gobs"
  >}}
  {{< card 
    link="services"
    title="Services"
    icons="/images/log.png"
    subtitle="The core business logics aka functional requiremennts that make your servers distinguish between the others"
  >}}
  {{< card 
    link="worker"
    title="Background processes"
    icons="/images/log.png"
    subtitle="Manage specific tasks in background for API servers"
  >}}
  {{< card 
    link="gprc"
    title="GRPC Client"
    icons="/images/log.png"
    subtitle="Manage GRPC connections using gobs"
  >}}
  {{< card 
    link="websocket"
    title="Websocket"
    icons="/images/log.png"
    subtitle="Manage connections and process websocket requests"
  >}}
  {{< card 
    link="queue"
    title="Background services"
    icons="/images/log.png"
    subtitle="Manage connectionns with backgroud services via message queue or event bus"
  >}}
{{< /cards >}}
