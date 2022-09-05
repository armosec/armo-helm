# ARMO cluster components
ARMO Vulnerability Scanning

![Version: 1.7.18](https://img.shields.io/badge/Version-1.7.18-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v1.7.18](https://img.shields.io/badge/AppVersion-v1.7.18-informational?style=flat-square)

## [Docs](https://hub.armosec.io/docs/installation-of-armo-in-cluster)

## Installing ARMO cluster components in a Kubernetes cluster Using Helm:

1. Add the Vulnerability Scanning Helm Repo
```
helm repo add armo https://armosec.github.io/armo-helm/
```

2. Update helm repo
```
helm repo update
```

3. Install the Helm Chart, use your account ID and give your cluster a name 

if you ran kubescape cli tool and submitted, you can get your Account ID from the local cache: 
```
kubescape config view | grep -i accountID
```
Otherwise, get the account ID from the [kubescape SaaS](https://hub.armosec.io/docs/installation-of-armo-in-cluster#install-a-pre-registered-cluster)

Run the install command:
```
helm upgrade --install armo  armo/armo-cluster-components -n armo-system --create-namespace --set account=<my_account_ID> --set clusterName=`kubectl config current-context` 
```

> Add `--set clientID=<generated client id> --set secretKey=<generated secret key>` if you have [generated an auth key](https://hub.armosec.io/docs/authentication)

> Add `--set kubescape.serviceMonitor.enabled=true` for installing the Prometheus service monitor, [read more about Prometheus integration](https://hub.armosec.io/docs/prometheus-exporter)
 
## Chart support

### Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| kollector.affinity | object | `{}` | Assign custom [affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) rules to the StatefulSet |
| kollector.enabled | bool | `true` | enable/disable the kollector |
| kollector.env[0] | object | `{"name":"PRINT_REPORT","value":"false"}` | print in verbose mode (print all reported data) |
| kollector.image.repository | string | `"quay.io/kubescape/kollector"` | [source code](https://github.com/kubescape/kollector) |
| kollector.nodeSelector | object | `{}` | [Node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) |
| kollector.volumes | object | `[]` | Additional volumes for the collector |
| kollector.volumeMounts | object | `[]` | Additional volumeMounts for the collector |
| kubescape.affinity | object | `{}` | Assign custom [affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) rules to the deployment |
| kubescape.downloadArtifacts | bool | `true` | download policies every scan, we recommend it should remain true, you should change to 'false' when running in an air-gapped environment or when scanning with high frequency (when running with Prometheus) |
| kubescape.enableHostScan | bool | `true` | enable [host scanner feature](https://hub.armosec.io/docs/host-sensor) |
| kubescape.enabled | bool | `true` | enable/disable kubescape scanning |
| kubescape.image.repository | string | `"quay.io/armosec/kubescape"` | [source code](https://github.com/armosec/kubescape/tree/master/httphandler) (public repo) |
| kubescape.nodeSelector | object | `{}` | [Node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) |
| kubescape.serviceMonitor.enabled | bool | `false` | enable/disable service monitor for prometheus (operator) integration |
| kubescape.skipUpdateCheck | bool | `false` | skip check for a newer version  |
| kubescape.submit | bool | `true` | submit results to ARMO SaaS: https://cloud.armosec.io/ |
| kubescape.volumes | object | `[]` | Additional volumes for Kubescape |
| kubescape.volumeMounts | object | `[]` | Additional volumeMounts for Kubescape |
| kubescapeScheduler.enabled | bool | `true` | enable/disable a kubescape scheduled scan using a CronJob |
| kubescapeScheduler.image.repository | string | `"quay.io/armosec/http_request"` | [source code](https://github.com/armosec/http-request) (public repo) |
| kubescapeScheduler.scanSchedule | string | `"0 0 * * *"` | scan schedule frequency |
| kubescapeScheduler.volumes | object | `[]` | Additional volumes for scan scheduler |
| kubescapeScheduler.volumeMounts | object | `[]` | Additional volumeMounts for scan scheduler |
| gateway.affinity | object | `{}` | Assign custom [affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) rules to the deployment |
| gateway.enabled | bool | `true` | enable/disable passing notifications from ARMO SaaS to the armo-web-socket microservice. The notifications are the onDemand scanning and the scanning schedule settings |
| gateway.image.repository | string | `"quay.io/kubescape/gateway"` | [source code](https://github.com/kubescape/gateway) |
| gateway.nodeSelector | object | `{}` | [Node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) |
| gateway.volumes | object | `[]` | Additional volumes for the notification service |
| gateway.volumeMounts | object | `[]` | Additional volumeMounts for the notification service |
| kubevuln.affinity | object | `{}` | Assign custom [affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) rules to the deployment |
| kubevuln.enabled | bool | `true` | enable/disable image vulnerability scanning |
| kubevuln.image.repository | string | `"quay.io/kubescape/kubevuln"` | [source code](https://github.com/kubescape/kubevuln) |
| kubevuln.nodeSelector | object | `{}` | [Node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) |
| kubevuln.volumes | object | `[]` | Additional volumes for the image vulnerability scanning |
| kubevuln.volumeMounts | object | `[]` | Additional volumeMounts for the image vulnerability scanning |
| kubevulnScheduler.enabled | bool | `true` | enable/disable a image vulnerability scheduled scan using a CronJob |
| kubevulnScheduler.image.repository | string | `"quay.io/armosec/http_request"` | [source code](https://github.com/armosec/http-request) (public repo) |
| kubevulnScheduler.scanSchedule | string | `"0 0 * * *"` | scan schedule frequency |
| kubevulnScheduler.volumes | object | `[]` | Additional volumes for scan scheduler |
| kubevulnScheduler.volumeMounts | object | `[]` | Additional volumeMounts for scan scheduler |
| operator.affinity | object | `{}` | Assign custom [affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) rules to the deployment |
| operator.enabled | bool | `true` | enable/disable kubescape and image vulnerability scanning |
| operator.image.repository | string | `"quay.io/kubescape/operator"` | [source code](https://github.com/kubescape/operator) |
| operator.nodeSelector | object | `{}` | [Node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) |
| operator.volumes | object | `[]` | Additional volumes for the web socket |
| operator.volumeMounts | object | `[]` | Additional volumeMounts for the web socket |
| kubescapeHostScanner.volumes | object | `[]` | Additional volumes for the host scanner |
| kubescapeHostScanner.volumeMounts | object | `[]` | Additional volumeMounts for the host scanner |
| awsIamRoleArn | string | `nil` | AWS IAM arn role |
| clientID | string | `""` | client ID, [read more](https://hub.armosec.io/docs/authentication) |
| addRevisionLabel | bool | `true` | Add revision label to the components. This will insure the components will restart when updating the helm |
| cloudRegion | string | `nil` | cloud region |
| cloudProviderEngine | string | `nil` | cloud provider engine |
| gkeProject | string | `nil` | GKE project |
| gkeServiceAccount | string | `nil` | GKE service account |
| secretKey | string | `""` | secret key, [read more](https://hub.armosec.io/docs/authentication) |
| triggerNewImageScan | bool | `false` | enable/disable trigger image scan for new images |
| volumes | object | `[]` | Additional volumes for all containers |
| volumeMounts | object | `[]` | Additional volumeMounts for all containers |


# In-cluster components overview

An overview of each in-cluster component which is part of the Kubescpae platform helm chart.
Follow the repository link for in-depth information on a specific component.

---

## High-level Architecture Diagram

```mermaid
graph TB

  client([client]) .-> dashboard
  masterGw  .- gw

  subgraph Cluster
    gw(Gateway)
    operator(Operator)
    k8sApi(Kubernetes API);
    kubevuln(Kubevuln)
    ks(Kubescape)
    gw --- operator
    operator -->|scan cluster| ks
    operator -->|scan images| kubevuln
    operator --> k8sApi
    ks --> k8sApi
  end;
  
subgraph Backend
    er(EventReceiver)
    dashboard(Dashboard) --> masterGw("Master Gateway") 
    ks --> er
    kubevuln --> er
  end;
  
  classDef k8s fill:#326ce5,stroke:#fff,stroke-width:1px,color:#fff;
  classDef plain fill:#ddd,stroke:#fff,stroke-width:1px,color:#000;
  class k8sApi k8s
  class ks,operator,gw,masterGw,kollector,kubevuln,er,dashboard plain
```

---

## [Gateway](https://github.com/kubescape/gateway)

* __Resource Kind:__ `Deployment`
* __Communication:__ REST API, Websocket
* __Responsibility:__ Broadcasts a message received to its registered clients. When a client registers itself in a Gateway it must provide a set of attributes, which will serve as identification, for message routing purposes.

  In our architecture, the Gateway acts both as a server and a client, depending on its running configuration:
  * Master Gateway: Refers to the instance running in the backend. Broadcasts messages to all of its registered Gateways.
  * In-cluster Gateway: Refers to the instance running in the cluster. Registered to the Master Gateway using a websocket; Broadcasts messages to the different in-cluster components, this enables executing actions in runtime.

  A Master Gateway communicates with multiple in-cluster Gateways, hence it is able to communicate with multiple clusters.

<details><summary>Component Diagram</summary>

```mermaid
graph TB
  subgraph Backend
   dashboard(Dashboard)
    masterGw("Gateway (Master)") 
  end
   subgraph Cluster N
    gw3("Gateway (In-cluster)")
    operator3(Operator)
  end;
  subgraph Cluster 2
    gw2("Gateway (In-cluster)")
    operator2(Operator)
  end;
   
subgraph Cluster 1
    gw1("Gateway (In-cluster)")
    operator1(Operator)
  end;
  dashboard --> masterGw
   masterGw .- gw2
   masterGw .- gw3
       gw1 .- operator1
    gw2 .- operator2
    gw3 .- operator3
   masterGw .- gw1

    
  classDef k8s fill:#326ce5,stroke:#fff,stroke-width:1px,color:#fff;
  classDef plain fill:#ddd,stroke:#fff,stroke-width:1px,color:#000;
  class k8sApi k8s
  class ks,operator1,dashboard,operator2,operator3 plain
```

</details>

---

## [Operator](https://github.com/kubescape/operator)

* __Resource Kind:__ `Deployment`
* __Communication:__ REST API, Websocket
* __Responsibility:__ The Operator component is at the heart of the solution as it is the triggering engine for the different actions in the cluster; It responds to REST API requests and messages received over websocket connection, and triggers the relevant action in the cluster. Such actions could be triggering a configuration scan, image vulnerability scan, defining a recurring scan (by creating CronJobs), etc.

<details><summary>Component Diagram</summary>

```mermaid
graph TB
  subgraph Cluster
    gw(Gateway)
    operator(Operator)
    k8sApi(Kubernetes API);
    kubevuln(Kubevuln)
    ks(Kubescape)
    urlCm{{ConfigMap<br>URLs}}
    recurringTempCm{{ConfigMap<br>Recur. Scan Template}}
    recurringScanCj{{CronJob<br>Recurring Scan}}
  end;
   masterGw(Master Gateway) .- gw
    gw ---> operator
    recurringScanCj ---> operator
    recurringScanCj --> recurringScanCj
    operator -->|scan cluster| ks
    operator -->|scan images| kubevuln
    operator --> k8sApi
    operator --- urlCm
    operator --- recurringTempCm
  
  classDef k8s fill:#326ce5,stroke:#fff,stroke-width:1px,color:#fff;
  classDef plain fill:#ddd,stroke:#fff,stroke-width:1px,color:#000;
  class k8sApi k8s
  class ks,gw,masterGw,kollector,urlCm,recurringScanCj,recurringTempCm,kubevuln,er,dashboard plain
```

</details>

---

## [Kubevuln](https://github.com/kubescape/kubevuln/)

* __Resource Kind:__ `Deployment`
* __Communication:__ REST API
* __Responsibility:__ Scans container images for vulnerabilities, using [Grype](https://github.com/anchore/grype) as its engine.

<details><summary>Component Diagram</summary>

```mermaid
graph TB

subgraph Cluster
    kubevuln(Kubevuln)  
    k8sApi(Kubernetes API)
    operator(Operator)
    gateway(Gateway)
    urlCm{{ConfigMap<br>URLs}}
    recurringScanCj{{CronJob<br>Recurring Scan}}
    recurringScanCm{{ConfigMap<br>Recurring Scan}}
    recurringTempCm{{ConfigMap<br>Recurring Scan Template}}

end

masterGateway  .- gateway
gateway .-|Scan Notification| operator 
operator -->|Collect NS, Images|k8sApi
operator -->|Start Scan| kubevuln
operator --- urlCm
urlCm --- kubevuln 
recurringTempCm --- operator
recurringScanCj -->|Periodic Run| recurringScanCj
recurringScanCj -->|Scan Notification| operator
recurringScanCm --- recurringScanCj

subgraph Backend
    er(EventReceiver)
    masterGateway("Master Gateway") 
    kubevuln -->|Scan Results| er
end;

classDef k8s fill:#326ce5,stroke:#fff,stroke-width:1px,color:#fff;
classDef plain fill:#ddd,stroke:#fff,stroke-width:1px,color:#000

class k8sApi k8s
class urlCm,recurringScanCm,operator,er,gateway,masterGateway,recurringScanCj,recurringTempCm plain


```
</details>

---

## [Kubescape](https://github.com/armosec/kubescape/tree/master/httphandler)

* __Resource Kind:__ `Deployment`
* __Communication:__ REST API
* __Responsibility:__ Runs [Kubescape](https://github.com/armosec/kubescape) for detecting misconfigurations in the cluster; This is microservice uses the same engine as the Kubescape CLI tool.

<details><summary>Component Diagram</summary>

```mermaid
graph TB

subgraph Cluster
    ks(Kubescape)  
    k8sApi(Kubernetes API)
    operator(Operator)
    gateway(Gateway)
    ksCm{{ConfigMap<br>Kubescape}}
    recurringScanCj{{CronJob<br>Recurring Scan}}
    recurringScanCm{{ConfigMap<br>Recurring Scan}}
    recurringTempCm{{ConfigMap<br>Recurring Scan Template}}
end

masterGateway  .- gateway
gateway .-|Scan Notification| operator 
operator -->|Start Scan| ks
ks-->|Collect Cluster Info|k8sApi
ksCm --- ks 
recurringTempCm --- operator
recurringScanCj -->|Periodic Run| recurringScanCj
recurringScanCj -->|Scan Notification| operator
recurringScanCm --- recurringScanCj
subgraph Backend
    er(EventReceiver)
    masterGateway("Master Gateway") 
    ks -->|Scan Results| er
end;

classDef k8s fill:#326ce5,stroke:#fff,stroke-width:1px,color:#fff;
classDef plain fill:#ddd,stroke:#fff,stroke-width:1px,color:#000

class k8sApi k8s
class ksCm,recurringScanCm,operator,er,gateway,masterGateway,recurringScanCj,recurringTempCm plain


```

</details>

---

## [Kollector](https://github.com/kubescape/kollector)

* __Resource Kind:__ `StatefulSet`
* __Responsibility:__ Communicates with the Kubernetes API server to collect cluster information and watches for changes in the cluster. Information is reported to the backend via the EventReceiver and the Gateway.

<details><summary>Component Diagram</summary>

```mermaid
graph TD
subgraph Backend
    er(EventReceiver)
    masterGw("Master Gateway") 
end;

subgraph Cluster
    kollector(Kollector) 
    k8sApi(Kubernetes API);
    gw(Gateway)
end;

kollector .->|Scan new image| gw
masterGw  .- gw
kollector --> er
kollector --> k8sApi

classDef k8s fill:#326ce5,stroke:#fff,stroke-width:1px,color:#fff;
classDef plain fill:#ddd,stroke:#fff,stroke-width:1px,color:#000;
class k8sApi k8s
class er,gw,masterGw plain
```

</details>

---

## [URLs ConfigMap](https://github.com/armosec/armo-helm/blob/master/charts/armo-components/templates/armo-configmap.yaml)

Holds a list of communication URLs. Used by the following components:

* Operator
* Kubevuln
* Gateway

<details><summary>Config Example (YAML)</summary>

```yaml
gatewayWebsocketURL: 127.0.0.1:8001                             # component: in-cluster gateway
gatewayRestURL: 127.0.0.1:8002                                  # component: in-cluster gateway
kubevulnURL: 127.0.0.1:8081                                     # component: kubevuln
kubescapeURL: 127.0.0.1:8080                                    # component: kubescape
eventReceiverRestURL: https://report.cloud.com                  # component: eventreceiver
eventReceiverWebsocketURL: wss://report.cloud.com               # component: eventreceiver
rootGatewayURL: wss://masterns.cloud.com/v1/waitfornotification # component: master gateway
accountID: 1111-aaaaa-4444-555
clusterName: minikube
```
</details>

---

## Kubernetes API

Some in-cluster components communicate with the Kubernetes API server for different purposes:

* Kollector

  Watches for changes in namespace, workloads, nodes. Reports information to the EventReceiver. Identifies image-related changes and triggers an image scanning on the new images accordingly (scanning new images functionality is optional).

* Operator

  Creates/updates/deletes resources for recurring scan purposes (CronJobs, ConfigMaps). Collects required information (NS, image names/tags) for Kubevuln's image scanning.

* Kubescape

  Collects namespaces, workloads, RBAC etc. required for cluster scans.

---

## Backend components

The backend components are running in [Kubescape's SaaS offering](https://cloud.armosec.io/).

### Dashboard

* REST API service

### EventReceiver

* __Responsibility:__ Receive and process Kubescape & Kubevuln scan results.
* __Communication:__ REST API

---

## Logging and troubleshooting

Each component writes logs to the standard output.

Every action has a generated `jobId` which is written to the log.

An action which creates sub-action(s), will be created with a different `jobId` but with a `parentId` which will correlate to the parent action's `jobId`.

---

## Recurring scans

3 types of recurring scans are supported:

  1. Cluster configuration scanning (Kubescape)
  2. Vulnerability scanning for container images (Kubevuln)
  3. Container registry scanning (Kubevuln)

When creating a recurring scan, the Operator component will create a `ConfigMap` and a `CronJob` from a recurring template ConfigMap. Each scan type comes with a template.

The CronJob itself does not run the scan directly. When a CronJob is ready to run, it will send a REST API request to the Operator component, which will then trigger the relevant scan (similarly to a request coming from the Gateway).

The scan results are then sent by each relevant component to the EventReceiver.

### Main Flows Diagrams

<details><summary>Recurring Scan Creation</summary>


```mermaid
sequenceDiagram
    actor user
    participant dashboard as Backend<br><br>Dashboard
    participant masterGw as Backend<br><br>Master Gateway
    participant clusterGw as Cluster<br><br>In-Cluster Gateway
    participant operator as Cluster<br><br>Operator
    participant k8sApi as Cluster<br><br>Kubernetes API
    
    user->>dashboard: 1. create scan schedule
    dashboard->>masterGw: 2. build schedule notification
    masterGw->>clusterGw: 3. broadcast notification
    clusterGw->>operator: 4. create recurring scan
    operator->>k8sApi: 5. get namespaces, workloads
    k8sApi-->>operator: 
    operator->>k8sApi: 6. Create cronjob & ConfigMap
```
</details>

<details><summary>Recurring Image Scan</summary>

```mermaid
sequenceDiagram
   participant cronJob as Cluster<br><br>CronJob
   participant operator as Cluster<br><br>Operator
   participant k8sApi as Cluster<br><br>Kubernetes API
   participant kubeVuln as Cluster<br><br>Kubevuln
   participant er as Backend<br><br>EventReceiver
   loop
      cronJob->>operator: 1. run image scan
   end
   operator->>k8sApi: 2. list NS, container images
   k8sApi-->>operator: 
   operator->>kubeVuln: 3. scan images
   kubeVuln ->> er: 4. send scan results
```

</details>

<details><summary>Recurring Kubescape Scan</summary>

```mermaid
sequenceDiagram
  participant cronJob as Cluster<br><br>CronJob
  participant operator as Cluster<br><br>Operator
  participant ks as Cluster<br><br>Kubescape
  participant k8sApi as Cluster<br><br>Kubernetes API
  participant er as Backend<br><br>EventReceiver
  loop
      cronJob->>operator: 1. run configuration scan
  end
  operator->>ks: 2. kubescape scan 
  ks->>k8sApi: 3. list NS, workloads, RBAC 
  k8sApi->>ks: 
  ks ->> er: 4. send scan results
```

</details>