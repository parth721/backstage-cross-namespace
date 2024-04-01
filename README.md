
## For backstage cross namespace communication : <br> 
First create an api and component in differnt namespace. 
And when get the event type it first look for in the current namespace,
if not found it looks on other namespaces. if found register the component else ignore it.

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
