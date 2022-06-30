# ARMO cluster components
ARMO Vulnerability Scanning

![Version: 1.7.12](https://img.shields.io/badge/Version-1.7.12-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v1.7.12](https://img.shields.io/badge/AppVersion-v1.7.12-informational?style=flat-square)

## [Docs](https://hub.armo.cloud/docs/installation-of-armo-in-cluster)

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

if you ran kubescape cli tool and submitted, you cam get your Account ID from the local cache: 
```
kubescape config view | grep -i accountID
```
Otherwise, get the account ID from the [kubescape SaaS](https://hub.armo.cloud/docs/installation-of-armo-in-cluster#install-a-pre-registered-cluster)

Run the install command:
```
helm upgrade --install armo  armo/armo-cluster-components -n armo-system --create-namespace --set accountGuid=<my_account_guid> --set clusterName=`kubectl config current-context` 
```

> Add `--set clientID=<generated client id> --set secretKey=<generated secret key>` if you have [generated an auth key](https://hub.armo.cloud/docs/authentication)

> Add `--set armoKubescape.serviceMonitor.enabled=true` for installing the Prometheus service monitor, [read more about Prometheus integration](https://hub.armo.cloud/docs/prometheus-exporter)
 
## Chart support

### Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| armoCollector.enabled | bool | `true` | enable/disable the armoCollector |
| armoCollector.env[0] | object | `{"name":"PRINT_REPORT","value":"false"}` | print in verbose mode (print all reported data) |
| armoCollector.image.repository | string | `"quay.io/armosec/cluster-collector"` | [source code](https://github.com/armosec/k8s-armo-collector) (private repo) |
| armoCollector.volumes | object | `[]` | Additional volumes for the collector |
| armoCollector.volumeMounts | object | `[]` | Additional volumeMounts for the collector |
| armoKubescape.downloadArtifacts | bool | `true` | download policies every scan, we recommend it should remain true, you should change to 'false' when running in an air-gapped environment or when scanning with high frequency (when running with Prometheus) |
| armoKubescape.enableHostScan | bool | `true` | enable [host scanner feature](https://hub.armo.cloud/docs/host-sensor) |
| armoKubescape.enabled | bool | `true` | enable/disable kubescape scanning |
| armoKubescape.image.repository | string | `"quay.io/armosec/kubescape"` | [source code](https://github.com/armosec/kubescape/tree/master/httphandler) (public repo) |
| armoKubescape.serviceMonitor.enabled | bool | `false` | enable/disable service monitor for prometheus (operator) integration |
| armoKubescape.skipUpdateCheck | bool | `false` | skip check for a newer version  |
| armoKubescape.submit | bool | `true` | submit results to ARMO SaaS: https://portal.armo.cloud/ |
| armoKubescape.volumes | object | `[]` | Additional volumes for Kubescape |
| armoKubescape.volumeMounts | object | `[]` | Additional volumeMounts for Kubescape |
| armoKubescapeScanScheduler.enabled | bool | `true` | enable/disable a kubescape scheduled scan using a CronJob |
| armoKubescapeScanScheduler.image.repository | string | `"quay.io/armosec/http_request"` | [source code](https://github.com/armosec/http-request) (public repo) |
| armoKubescapeScanScheduler.scanSchedule | string | `"0 0 * * *"` | scan schedule frequency |
| armoKubescapeScanScheduler.volumes | object | `[]` | Additional volumes for scan scheduler |
| armoKubescapeScanScheduler.volumeMounts | object | `[]` | Additional volumeMounts for scan scheduler |
| armoNotificationService.enabled | bool | `true` | enable/disable passing notifications from ARMO SaaS to the armo-web-socket microservice. The notifications are the onDemand scanning and the scanning schedule settings |
| armoNotificationService.image.repository | string | `"quay.io/armosec/notification-server"` | [source code](https://github.com/armosec/capostman) (private repo) |
| armoNotificationService.volumes | object | `[]` | Additional volumes for the notification service |
| armoNotificationService.volumeMounts | object | `[]` | Additional volumeMounts for the notification service |
| armoScanScheduler.enabled | bool | `true` | enable/disable image vulnerability a schedule scan using a CronJob |
| armoScanScheduler.image.repository | string | `"curlimages/curl"` | image: curlimages/curl |
| armoScanScheduler.scanSchedule | string | `"0 0 * * *"` | scan schedule frequency |
| armoKubescapeScanScheduler.volumes | object | `[]` | Additional volumes for scan scheduler |
| armoKubescapeScanScheduler.volumeMounts | object | `[]` | Additional volumeMounts for scan scheduler |
| armoVulnScanner.enabled | bool | `true` | enable/disable image vulnerability scanning |
| armoVulnScanner.image.repository | string | `"quay.io/armosec/images-vulnerabilities-scan"` | [source code](https://github.com/armosec/ca-vuln-scan) (private repo) |
| armoVulnScanner.volumes | object | `[]` | Additional volumes for the image vulnerability scanning |
| armoVulnScanner.volumeMounts | object | `[]` | Additional volumeMounts for the image vulnerability scanning |
| armoWebsocket.enabled | bool | `true` | enable/disable kubescape and image vulnerability scanning |
| armoWebsocket.image.repository | string | `"quay.io/armosec/action-trigger"` | [source code](https://github.com/armosec/k8s-ca-websocket) (private repo) |
| armoWebsocket.volumes | object | `[]` | Additional volumes for the web socket |
| armoWebsocket.volumeMounts | object | `[]` | Additional volumeMounts for the web socket |
| armoKubescapeHostScanner.volumes | object | `[]` | Additional volumes for the host scanner |
| armoKubescapeHostScanner.volumeMounts | object | `[]` | Additional volumeMounts for the host scanner |
| aws_iam_role_arn | string | `nil` | AWS IAM arn role |
| clientID | string | `""` | client ID, [read more](https://hub.armo.cloud/docs/authentication) |
| cloudRegion | string | `nil` | cloud region |
| cloud_provider_engine | string | `nil` | cloud provider engine |
| gkeProject | string | `nil` | GKE project |
| gke_service_account | string | `nil` | GKE service account |
| secretKey | string | `""` | secret key, [read more](https://hub.armo.cloud/docs/authentication) |
| triggerNewImageScan | string | `"disable"` | enable/disable trigger image scan for new images |
| volumes | object | `[]` | Additional volumes for all containers |
| volumeMounts | object | `[]` | Additional volumeMounts for all containers |


