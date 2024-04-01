
## For backstage cross namespace communication : <br> 
---
First create an api and component in differnt namespace. 
And when get the event type it first look for in the current namespace,
if not found it looks on other namespaces. if found register the component else ignore it.

## Prerequisite
---
1. node.js
2. yarn
3. git

## procedure 
---
1. create a directory
2. setup backsatge in it with `npx @backstage/create-app@latest`
3. create manifest directory to store the resources
4. create API and Component keeping them in differnt namespace
5. push all these on github
6. start backstage and register the components and API.

## Quickstart demo 
---
[youtube link](https://youtu.be/HwZybuF0kjY)


## Diagram 
---
```mermaid
flowchart TD
    GetEventType[Get EventType]
    GetConsumer[Get EventType consumer Backstage id]
    QueryConsumerInCurrentNamespace(Query consumer in current <br> Backstage namespace)
    CheckComponentInCurrentNamespaceExists{Component in current <br> Backstage namespace exists}
    RegisterConsumer1[Register component as the consumer]
    QueryConsumerInAllNamespaces[Query consumer in all <br> Backstage namespaces]
    CheckNamespaceAnnotation{Kubernetes namespace annotation <br> matches the Kubernetes namespace?}
    RegisterConsumer2[Register component as the consumer]

    GetEventType --> GetConsumer
    GetConsumer --> QueryConsumerInCurrentNamespace
    QueryConsumerInCurrentNamespace --> CheckComponentInCurrentNamespaceExists
    CheckComponentInCurrentNamespaceExists --> |Yes| RegisterConsumer1
    CheckComponentInCurrentNamespaceExists --> |No| QueryConsumerInAllNamespaces
    QueryConsumerInAllNamespaces --> CheckNamespaceAnnotation
    CheckNamespaceAnnotation --> |Yes| RegisterConsumer2
    CheckNamespaceAnnotation --> |No| Ignore

    GetConsumer -.- Note1["At this stage, we have the Kubernetes namespace and the <br> Backstage component ID of the consumer. <br> We don't have the Backstage namespace though."]
    Note1:::note
    classDef note fill:yellow
```
