# Script for the demo

## 1 - Create a Namespace

### Goal

```mermaid
flowchart LR

subgraph Cluster

    subgraph develop[develop]

    end
end


classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;


class Cluster Physical
class develop Logical
```

### Resource files


[namespace.yaml](.k8s/develop/namespace.yaml)

### Commands

```shell
kubectl get ns

kubectl apply -f .k8s/develop/namespace.yaml 

kubectl get ns

kubectl get pods --namespace develop
```

## 2 - Create the Api via Pod

### Goal

```mermaid
flowchart LR

subgraph Cluster

    subgraph develop[develop]
        subgraph Api
            Container1[HTTP Server]
        end
    end
end


classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;

classDef Pod fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


classDef Container fill:#bbb,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


class Cluster Physical
class develop Logical
class Api Pod
class Container1 Container
```

### Resources

[api.yaml](.k8s/develop/api.yaml)

### Commands

```shell
kubectl apply -f .k8s/develop/api.yaml 

kubectl get pods -n develop

kubectl describe po api -n develop
```

## Expose Pod 1 via Service 1

### Goal


```mermaid
flowchart LR

Browser

subgraph Cluster

    subgraph develop[develop]
        subgraph Api
            Container1[HTTP Server]
        end

        ApiGtwSvc(Api)
    end
end

ApiGtwSvc --- Api
Browser --> ApiGtwSvc


classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;

classDef Pod fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

classDef Container fill:#bbb,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


classDef PublicService fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

class Cluster Physical
class develop Logical
class Api Pod
class Container1 Container
class ApiGtwSvc PublicService
```

### Resources

[api-svc.yaml](.k8s/develop/api-svc.yaml)

### Commands

```shell
kubectl apply -f .k8s/develop/api-svc.yaml

kubectl get all -n develop
```

Open: http://localhost:30001


## Add backend via Deployment

### Goal

```mermaid
flowchart LR

Browser

subgraph Cluster

    subgraph develop[develop]
        subgraph Api
            Container1[HTTP Server]
        end

        subgraph Backend
            subgraph Backend1["Backend(1)"]
                Container1_1[HTTP Server]
            end
            subgraph Backend2["Backend(1)"]
                Container1_2[HTTP Server]
            end
            subgraph Backend3["Backend(1)"]
                Container1_3[HTTP Server]
            end
        end

        ApiGtwSvc(Service)
    end
end

ApiGtwSvc --- Api
Browser --> ApiGtwSvc


classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;

classDef Pod fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

classDef Container fill:#bbb,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

classDef PublicService fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


class Cluster Physical
class develop Logical
class Backend Logical
class Api Pod
class Backend1 Pod
class Backend2 Pod
class Backend3 Pod
class Container1 Container
class Container1_1 Container
class Container1_2 Container
class Container1_3 Container
class ApiGtwSvc PublicService
```


Show [backend.yaml](.k8s/develop/backend.yaml)

```shell

kubectl apply -f .k8s/develop/backend.yaml

kubectl get all -n develop
```

### 5 - Connect Api to Backend


### Goal

```mermaid
flowchart LR

Browser

subgraph Cluster

    subgraph develop[develop]
        
        subgraph Api
            Container1[HTTP Server]
        end

        subgraph Backend
            subgraph Backend1["Backend(1)"]
                Container1_1[HTTP Server]
            end
            subgraph Backend2["Backend(1)"]
                Container1_2[HTTP Server]
            end
            subgraph Backend3["Backend(1)"]
                Container1_3[HTTP Server]
            end
        end

        ApiGtwSvc(Api)
        BackendSvc(Backend)
    end
end

ApiGtwSvc --- Api
Browser --> ApiGtwSvc

BackendSvc --- Backend1
BackendSvc --- Backend2
BackendSvc --- Backend3
Api ----> BackendSvc

classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;

classDef Pod fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

classDef Container fill:#bbb,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

classDef PublicService fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


classDef PrivateService fill:#ccc,stroke:#333,stroke-width:3px,color:#333,font-size:11px;


class Cluster Physical
class develop Logical
class Backend Logical
class Api Pod
class Backend1 Pod
class Backend2 Pod
class Backend3 Pod
class Container1 Container
class Container1_1 Container
class Container1_2 Container
class Container1_3 Container
class ApiGtwSvc PublicService
class BackendSvc PrivateService
```

### Resources

[backend service](.k8s/develop/backend-svc.yaml)

```shell

kubectl apply -f .k8s/develop/backend-svc.yaml

kubectl get all -n develop
```

Open: http://localhost:30001
