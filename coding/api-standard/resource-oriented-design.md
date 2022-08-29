# Resource oriented design

> Based on: https://google.aip.dev/121

Resource-oriented design is a pattern for specifying [RPC](https://en.wikipedia.org/wiki/Remote\_procedure\_call) APIs, based on several high-level design principles (most of which are common to recent public HTTP APIs):

* The fundamental building blocks of an API are individually-named _resources_ (nouns) and the relationships and hierarchy that exist between them.
* A small number of standard _methods_ (verbs) provide the semantics for most common operations. However, custom methods are available in situations where the standard methods do not fit.
* Stateless protocol: Each interaction between the client and the server is independent, and both the client and server have clear roles.

Readers might notice similarities between these principles and some principles of [REST](https://en.wikipedia.org/wiki/Representational\_state\_transfer); resource-oriented design borrows many principles from REST, while also defining its own patterns where appropriate.

## Guidance

When designing an API, consider the following (roughly in logical order):

* The resources (nouns) the API will provide
* The relationships and hierarchies between those resources
* The schema of each resource
* The methods (verbs) each resource provides, relying as much as possible on the standard verbs.

### Resources

A resource-oriented API **should** generally be modeled as a resource hierarchy, where each node is either a simple resource or a collection of resources.

A _collection_ contains resources of _the same type_. For example, a publisher has the collection of books that it publishes. A resource usually has fields, and resources may have any number of sub-resources (usually collections).

**Note:** While there is some conceptual alignment between storage systems and APIs, a service with a resource-oriented API is not necessarily a database, and has enormous flexibility in how it interprets resources and methods. API designers **should not** expect that their API will be reflective of their database schema. In fact, having an API that is identical to the underlying database schema is actually an anti-pattern, as it tightly couples the surface to the underlying system.

### Methods

Resource-oriented APIs emphasize resources (data model) over the methods performed on those resources (functionality). A typical resource-oriented API exposes a large number of resources with a small number of methods on each resource. The methods can be either the standard methods ([Get](../../docs/api-standard/0131.md), [List](../../docs/api-standard/0132.md), [Create](../../docs/api-standard/0133.md), [Update](../../docs/api-standard/0134.md), [Delete](../../docs/api-standard/0135.md)), or [custom methods](../../docs/api-standard/0136.md).

**Note:** A custom method in resource-oriented design does _not_ entail defining a new or custom HTTP verb. Custom methods use traditional HTTP verbs (usually `POST`) and define the custom verb in the URI.

APIs **should** prefer standard methods over custom methods; the purpose of custom methods is to define functionality that does not cleanly map to any of the standard methods. Custom methods offer the same design freedom as traditional RPC APIs, which can be used to implement common programming patterns, such as database transactions, import and export, or data analysis.

### Stateless protocol

As with most public APIs available today, resource-oriented APIs **must** operate over a stateless protocol: The fundamental behavior of any individual request is independent of other requests made by the caller.

In an API with a stateless protocol, the server has the responsibility for persisting data, which may be shared between multiple clients, while clients have sole responsibility and authority for maintaining the application state.
