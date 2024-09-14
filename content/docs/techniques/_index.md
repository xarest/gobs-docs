+++
title = 'Techniques'
date = 2024-07-13T00:33:02+07:00
draft = true
weight = 300
+++

Originally, GoBs is created to resolve the complexity of initialization and sharing service instances in Dependecies Injection - DI pattern of the API server. Although GoBS supports to resolve the complexity, the clean & clear structure eliminating the issues is referred rather than perfect solutions for particular problems. This section introduces an architecture supporting general features that a API webserver should have.

![](components_dependencies.svg "Flow of dependencies between components in a DI project")
