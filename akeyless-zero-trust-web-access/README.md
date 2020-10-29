# Akeyless-Zero-Trust-Web-Access
 

## TL;DR

```bash
$ git clone git@github.com:akeylesslabs/akeyless-services-helm.git
$ cd akeyless-zero-trust-web-access
$ helm install my-release .
```

## Preparation

### Network
When using Embedded browser session behind load balancer such as ELB, the session can be closed due to **idle connection timeout**, so its advise to increase it
to a reasonable high value, or event unlimited.

e.g when running on AWS with ELB:
https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/config-idle-timeout.html?icmpid=docs_elb_console


### Preqrequisites

#### Horizonal Auto-Scaling
Horizontal auto-scaling is based on the HorizonalPodAutoscaler object.  
For it to work properly, Kubernetes metrics server must be installed in the cluster - https://github.com/kubernetes-sigs/metrics-server


## Installing the Chart

The `values.yaml` file holds default values, replace the values with the ones from your environment where needed.  

To install the chart, clone the repo and run:
```bash
helm install my-release .
``` 

## Parameters

The following table lists the configurable parameters of the Zero-Trust-Web-Access chart and their default values.

### Deployment parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `image.dockerRepositoryCreds`             | Akeyless docker repository credentials - required                                                                    | `nil`                                                        |
| `dispatcher.image.repository`             | Zero-Trust-Web-Access Dispatcher image name                                                                          | `akeyless/zero-trust-web-dispatcher`                         |
| `dispatcher.image.tag`                    | Zero-Trust-Web-Access Dispatcher image tag                                                                           | `latest`                                                     |      
| `dispatcher.image.pullPolicy`             | Zero-Trust-Web-Access Dispatcher image pull policy                                                                   | `Always`                                                     |  
| `dispatcher.containerName`                | Zero-Trust-Web-Access Dispatcher container name                                                                      | `web-dispatcher`                                             |    
| `dispatcher.replicaCount`                 | Number of Zero-Trust-Web-Access Dispatcher nodes                                                                     | `1`                                                          |
| `dispatcher.livenessProbe`                | Liveness probe configuration for Zero-Trust-Web-Access Dispatcher                                                    | Check `values.yaml` file                                     |                   
| `dispatcher.readinessProbe`               | Readiness probe configuration for Zero-Trust-Web-Access Dispatcher                                                   | Check `values.yaml` file                                     |         
| `dispatcher.resources.limits`             | The resources limits for Zero-Trust-Web-Access Dispatcher containers                                                 | `{}`                                                         |
| `dispatcher.resources.requests`           | The requested resources for Zero-Trust-Web-Access Dispatcher containers                                              | `{}`                                                         |
| `webWorker.image.repository`              | Zero-Trust-Web-Access Web Worker image name                                                                          | `akeyless/zero-trust-web-worker`                             |
| `webWorker.image.tag`                     | Zero-Trust-Web-Access Web Worker image tag                                                                           | `latest`                                                     |      
| `webWorker.image.pullPolicy`              | Zero-Trust-Web-Access Web Worker image pull policy                                                                   | `Always`                                                     |
| `webWorker.containerName`                 | Zero-Trust-Web-Access Web Worker container name                                                                      | `web-worker`                                                 |
| `webWorker.replicaCount`                  | Number of Zero-Trust-Web-Access Web Worker nodes                                                                     | `5`                                                          |
| `webWorker.livenessProbe`                 | Liveness probe configuration for Zero-Trust-Web-Access Web Worker                                                    | Check `values.yaml` file                                     |                   
| `webWorker.readinessProbe`                | Readiness probe configuration for Zero-Trust-Web-Access Web Worker                                                   | Check `values.yaml` file                                     |         
| `webWorker.resources.limits`              | The resources limits for Zero-Trust-Web-Access Web Worker containers                                                 | `{}`                                                         |
| `webWorker.resources.requests`            | The requested resources for Zero-Trust-Web-Access Web Worker containers                                              | `{}`                                                         |

### Exposure parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `dispatcher.service.type`                 | Kubernetes service type                                                                                              | `LoadBalancer`                                               |
| `dispatcher.service.port`                 | Dispatcher service port                                                                                              | `9000`                                                       |
| `dispatcher.service.annotations`          | Dispatcher service extra annotations                                                                                 | `{}`                                                         |
| `webWorker.service.port`                  | Web Worker service port                                                                                              | `5800`                                                       |
| `webWorker.service.annotations`           | Web Worker service extra annotations                                                                                 | `{}`                                                         |


### HPA parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `HPA.enabled`                             | Enable Zero-Trust-Web-Access Horizontal Pod Autoscaler                                                               | `false`                                                      |
| `HPA.dispatcher.minReplicas`              | Dispatcher Minimum desired number of replicas                                                                        | `1`                                                          |
| `HPA.dispatcher.maxReplicas`              | Dispatcher Minimum desired number of replicas                                                                        | `14`                                                         |
| `HPA.dispatcher.cpuAvgUtil`               | Dispatcher CPU average utilization                                                                                   | `50`                                                         |
| `HPA.dispatcher.memAvgUtil`               | Dispatcher Memory average utilization                                                                                | `50`                                                         |
| `HPA.webWorker.minReplicas`               | Web Worker Minimum desired number of replicas                                                                        | `5`                                                          |
| `HPA.webWorker.maxReplicas`               | Web Worker Minimum desired number of replicas                                                                        | `14`                                                         |
| `HPA.webWorker.cpuAvgUtil`                | Web Worker CPU average utilization                                                                                   | `50`                                                         |
| `HPA.webWorker.memAvgUtil`                | Web Worker Memory average utilization                                                                                | `50`                                                         |                                                                                        

### Zero-Trust-Web-Access configuration parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `dispatcher.config.akeyless`              | Akeyless browser extension config - required                                                                         | Check `values.yaml` file                                     |
| `webWorker.config.policies`               | Web worker browser policy configuration                                                                              | Check `values.yaml` file                                     |
                       
