+++
title = 'Architecture'
date = 2024-07-13T02:37:23+07:00
draft = true
weight = 301
slug = 'architecture'
+++
This is all components in an general API webserver project. It includes two main entry points: API routers and API workers

![](program_structure.svg "Structure of a complete api server")

### Explain components

- **api**: Serve almost features as a gateway for clent acessing into the services and resources of the system.
- **worker**: Usually built-in with the api server to support the main API server for some tasks which should be delayed in background. This kind of worker is simple and the API server calls it directly without any message queue dispatcher. For larger scale, background tasks can be processed in distributed worker instances. The API server requires message queue to send command to those workers.
- **libs**: Wrapper for shared client instances to the 3rd-party libraries. Some of them are used to connect to the 3rd party services and they should be mocked for testing.
- **_utils_**: pure functions for particular stateless features
- _model_: The definitions of parameters for input/output into/from the repositories
- _request/response_: The definitions of parameters for input/output into/from the services with controllers and the controllers with the clients

### File structure

{{< filetree/container >}}
  {{< filetree/folder name="api" state="closed">}}
    {{< filetree/folder name="middleware" state="closed">}}
        {{< filetree/file name="authen.go" >}}
        {{< filetree/file name="author.go" >}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="validator" state="closed">}}
        {{< filetree/file name="password.go" >}}
        {{< filetree/file name="otp.go" >}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="controller" state="closed">}}
        {{< filetree/file name="auth.go" >}}
        {{< filetree/file name="user.go" >}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="service" state="closed">}}
        {{< filetree/file name="auth.go" >}}
        {{< filetree/file name="role_permission.go" >}}
        {{< filetree/file name="user.go" >}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="repository" state="closed">}}
        {{< filetree/file name="auth.go" >}}
        {{< filetree/file name="role_permission.go" >}}
        {{< filetree/file name="user.go" >}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="dto" state="closed">}}
      {{< filetree/folder name="request" state="closed">}}
        {{< filetree/file name="account.go" >}}
        {{< filetree/file name="permission.go" >}}
        {{< filetree/file name="role.go" >}}
        {{< filetree/file name="user.go" >}}
      {{< /filetree/folder >}}
      {{< filetree/folder name="response" state="closed">}}
        {{< filetree/file name="account.go" >}}
        {{< filetree/file name="permission.go" >}}
        {{< filetree/file name="role.go" >}}
        {{< filetree/file name="user.go" >}}
      {{< /filetree/folder >}}
      {{< filetree/folder name="model" state="closed">}}
        {{< filetree/file name="account.go" >}}
        {{< filetree/file name="user.go" >}}
      {{< /filetree/folder >}}
      {{< filetree/folder name="schema" state="closed">}}
        {{< filetree/file name="permission.go" >}}
        {{< filetree/file name="role.go" >}}
        {{< filetree/file name="role_permission.go" >}}
        {{< filetree/file name="task.go" >}}
        {{< filetree/file name="user.go" >}}
        {{< filetree/file name="user_role.go" >}}
      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
    {{< filetree/file name="router.go" >}}
  {{< /filetree/folder >}}
  {{< filetree/folder name="worker" state="closed">}}
    {{< filetree/file name="scheduler.go" >}}
    {{< filetree/file name="client.go" >}}
  {{< /filetree/folder >}}
  {{< filetree/folder name="utils" state="closed">}}
    {{< filetree/file name="context.go" >}}
    {{< filetree/file name="generator.go" >}}
    {{< filetree/file name="convert.go" >}}
  {{< /filetree/folder >}}
  {{< filetree/folder name="lib" state="closed">}}
    {{< filetree/folder name="db" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="config" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="logger" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="storage" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="cache" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="queue" state="closed">}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
  {{< filetree/folder name="test" state="closed">}}
    {{< filetree/folder name="e2e" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="helper" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="mock" state="closed">}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
  {{< filetree/folder name="deployment" state="closed">}}
    {{< filetree/folder name="migration" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="scripts" state="closed">}}
    {{< /filetree/folder >}}
    {{< filetree/file name="Dockerfile" >}}
    {{< filetree/file name="docker-compose.yml" >}}
  {{< /filetree/folder >}}
    {{< filetree/folder name="docs" state="closed" >}}
    {{< filetree/file name="_index.md" >}}
    {{< filetree/file name="introduction.md" >}}
  {{< /filetree/folder >}}
  {{< filetree/file name="main.go" >}}
  {{< filetree/file name="go.mod" >}}
  {{< filetree/file name="go.sum" >}}
{{< /filetree/container >}}

<!-- 
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
{{< /cards >}} -->
