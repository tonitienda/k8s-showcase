# Script for the demo

## 1 - Create a Namespace

### Goal

```mermaid
flowchart LR

subgraph Cluster

    subgraph NS1[ns1]

    end
end


classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;


class Cluster Physical
class NS1 Logical
```

### Resource files


[namespace.yaml](.k8s/1_namespace1.yaml)

### Commands

```shell
kubectl get ns

kubectl apply -f .k8s/1_namespace1.yaml 

kubectl get ns

kubectl get pods --namespace ns1
```

## 2 - Create the ApiGtw via Pod

### Goal

```mermaid
flowchart LR

subgraph Cluster

    subgraph NS1[ns1]
        subgraph ApiGtw
            Container1[Container]
        end
    end
end


classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;

classDef Pod fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


classDef Container fill:#bbb,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


class Cluster Physical
class NS1 Logical
class ApiGtw Pod
class Container1 Container
```

### Resources

[ApiGtw.yaml](.k8s/2_api-gtw.yaml)

### Commands

```shell
kubectl apply -f .k8s/api-gtw.yaml 

kubectl get pods -n ns1

kubectl describe po api-gtw -n ns1
```

## Expose Pod 1 via Service 1

### Goal


```mermaid
flowchart LR

Browser

ApiGtwEndPoint((" "))

subgraph Cluster

    subgraph NS1[ns1]
        subgraph ApiGtw
            Container1[Container]
        end

        ApiGtwSvc(ApiGtw)
    end
end

ApiGtwSvc --- ApiGtw
ApiGtwSvc --- ApiGtwEndPoint
Browser --> ApiGtwEndPoint


classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;

classDef Pod fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

classDef Container fill:#bbb,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


classDef Service fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


class Cluster Physical
class NS1 Logical
class ApiGtw Pod
class Container1 Container
class ApiGtwSvc Service
```

### Resources

[api-gtw-svc.yaml](.k8s/3-api-gtw-svc.yaml)

### Commands

```shell
kubectl apply -f .k8s/3_api-gtw-svc.yaml

kubectl get all -n ns1
```

Open: http://localhost:30001


## Add backend via Deployment

### Goal

```mermaid
flowchart LR

Browser

ApiGtwEndPoint((" "))

subgraph Cluster

    subgraph NS1[ns1]
        subgraph ApiGtw
            Container1[Container]
        end

        subgraph Backend
            subgraph Backend1["Backend(1)"]
                Container1_1[Container]
            end
            subgraph Backend2["Backend(1)"]
                Container1_2[Container]
            end
            subgraph Backend3["Backend(1)"]
                Container1_3[Container]
            end
        end

        ApiGtwSvc(Service)
    end
end

ApiGtwSvc --- ApiGtw
ApiGtwSvc --- ApiGtwEndPoint
Browser --> ApiGtwEndPoint


classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;

classDef Pod fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

classDef Container fill:#bbb,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


classDef Service fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


class Cluster Physical
class NS1 Logical
class Backend Logical
class ApiGtw Pod
class Backend1 Pod
class Backend2 Pod
class Backend3 Pod
class Container1 Container
class Container1_1 Container
class Container1_2 Container
class Container1_3 Container
class ApiGtwSvc Service
```


Show [backend-deployment.yaml](.k8s/4_backend-deployment.yaml)

```shell

kubectl apply -f .k8s/deployment1.yaml

kubectl get all -ns ns1
```

### 5 - Connect ApiGtw to Backend


### Goal

```mermaid
flowchart LR

Browser

ApiGtwEndPoint((" "))

subgraph Cluster

    subgraph NS1[ns1]

        BackendEndPoint((" "))
        
        subgraph ApiGtw
            Container1[Container]
        end

        subgraph Backend
            subgraph Backend1["Backend(1)"]
                Container1_1[Container]
            end
            subgraph Backend2["Backend(1)"]
                Container1_2[Container]
            end
            subgraph Backend3["Backend(1)"]
                Container1_3[Container]
            end
        end

        ApiGtwSvc(ApiGtw)
        BackendSvc(Backend)
    end
end

ApiGtwSvc --- ApiGtw
ApiGtwSvc --- ApiGtwEndPoint
Browser --> ApiGtwEndPoint

BackendSvc --- Backend1
BackendSvc --- Backend2
BackendSvc --- Backend3
BackendSvc --- BackendEndPoint
ApiGtw ----> BackendEndPoint

classDef Physical fill:#eee,stroke:#333,stroke-width:2px,color:#333,font-size:11px;

classDef Logical fill:#ddd,stroke:#333,stroke-width:1px,color:#333,font-size:11px,stroke-dasharray: 5, 5;

classDef Pod fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;

classDef Container fill:#bbb,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


classDef Service fill:#ccc,stroke:#333,stroke-width:1px,color:#333,font-size:11px;


class Cluster Physical
class NS1 Logical
class Backend Logical
class ApiGtw Pod
class Backend1 Pod
class Backend2 Pod
class Backend3 Pod
class Container1 Container
class Container1_1 Container
class Container1_2 Container
class Container1_3 Container
class ApiGtwSvc Service
class BackendSvc Service
```

### Resources

[backend service](.k8s/5_backend-svc.yaml)

```shell

kubectl apply -f .k8s/5_backend-svc.yaml

kubectl get all -ns ns1
```

Open: http://localhost:30001
