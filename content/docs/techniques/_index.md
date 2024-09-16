+++
title = 'Techniques'
date = 2024-07-13T00:33:02+07:00
draft = true
weight = 300
+++

Originally, GoBs is created to resolve the complexity of initialization and sharing service instances in Dependecies Injection - DI pattern of the API server. Although GoBS supports to resolve the complexity, the clean & clear structure eliminating the issues is referred rather than perfect solutions for particular problems. This section introduces an architecture supporting general features that a API webserfver should have.

![](components_dependencies.svg "Flow of dependencies between components in a DI project")

This section will describe a sample project which includes 

![](program_structure.svg "Structure of a complete api server")

### Explain components

- **api**: Serve almost features as a gateway for clent acessing into the services and resources of the system.
- **worker**: Usually built-in with the api server to support the main API server for some tasks which should be delayed in background. This kind of worker is simple and the API server calls it directly without any message queue dispatcher. For larger scale, background tasks can be processed in distributed worker instances. The API server requires message queue to send command to those workers.
- **libs**: Wrapper for shared client instances to the 3rd-party libraries. Some of them are used to connect to the 3rd party services and they should be mocked for testing.
- **_utils_**: pure functions for particular stateless features
- _model_: The definitions of parameters for input/output into/from the repositories
- _request/response_: The definitions of parameters for input/output into/from the services with controllers and the controllers with the clients
