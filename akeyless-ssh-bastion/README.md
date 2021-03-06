# Akeyless-SSH-Bastion

[Akeyless-SSH-Bastion](https://docs.akeyless.io/docs/how-to-configure-ssh#akeyless-ssh-bastion) Traffic SSH connections with signed certificate authentication, together with session recording. 

## TL;DR

```bash
$ git clone git@github.com:akeylesslabs/akeyless-services-helm.git
$ cd akeyless-ssh-bastion
$ helm install my-release .
```

## Introduction
This chart bootstraps a Akeyless-SSH-Bastion deployment on a Kubernetes cluster using the Helm package manager.


## Preparation

### Storage
Currently, the helm chart requires a storage class with ReadWriteMany access modes.  
Since a storage class is more environment specific, you will need to provide one before proceeding.
In addition, please provide 2 PersistentVolumes with `persistentVolumeReclaimPolicy: retain` and reference those PVs in the `values.yaml` file

e.g when running on AWS with EKS:
https://docs.aws.amazon.com/eks/latest/userguide/efs-csi.html

### Network
When using SSH sessions behind load balancer such as ELB, the session can be closed due to **idle connection timeout**, so its advise to increase it
to a reasonable high value, or event unlimited.

e.g when running on AWS with ELB:
https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/config-idle-timeout.html?icmpid=docs_elb_console

### Prerequisites

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

The following table lists the configurable parameters of the SSH-Bastion chart and their default values.

### Statefulset parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `image.repository`                        | SSH-Bastion image name                                                                                               | `akeyless/ssh-proxy`                                         |
| `image.tag`                               | SSH-Bastion image tag                                                                                                | `latest`                                                     |      
| `image.pullPolicy`                        | SSH-Bastion image pull policy                                                                                        | `Always`                                                     |  
| `image.dockerRepositoryCreds`             | Akeyless docker repository credentials                                                                               | `nil`                                                        |
| `updateStrategy`                          | Updating statefulset strategy                                                                                        | `RollingUpdate`                                              |  
| `containerName`                           | SSH-Bastion container name                                                                                           | `ssh-proxy`                                                  |  
| `replicaCount`                            | Number of SSH-Bastion nodes                                                                                          | `1`                                                          |
| `livenessProbe`                           | Liveness probe configuration for SSH-Bastion                                                                         | Check `values.yaml` file                                     |                   
| `readinessProbe`                          | Readiness probe configuration for SSH-Bastion                                                                        | Check `values.yaml` file                                     |         
| `resources.limits`                        | The resources limits for SSH-Bastion containers                                                                      | `{}`                                                         |
| `resources.requests`                      | The requested resources for SSH-Bastion containers                                                                   | `{}`                                                         |


### Exposure parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `service.type`                            | Kubernetes Service type                                                                                              | `LoadBalancer`                                               |
| `service.port`                            | ssh port                                                                                                             | `22`                                                         |
| `service.curlProxyPort`                   | Akeyless curl proxy port                                                                                             | `9900`                                                       |


### Persistence parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `persistence.volumes`                     | requires data persistence to be shared within all pods in the cluster, only ReadWriteMany is supported               | Check `values.yaml` file                                     |


### HPA parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `HPA.enabled`                             | Enable SSH-Bastion Horizontal Pod Autoscaler                                                                         | `false`                                                      |
| `HPA.minReplicas`                         | Minimum desired number of replicas                                                                                   | `1`                                                          |
| `HPA.maxReplicas`                         | Minimum desired number of replicas                                                                                   | `14`                                                         |
| `HPA.cpuAvgUtil`                          | CPU average utilization                                                                                              | `50`                                                         |
| `HPA.memAvgUtil`                          | Memory average utilization                                                                                           | `50`                                                         |
                                                                                        

### SSH-Bastion configuration parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `config.CAPublicKey`                      | CA’s public key (ca.pub) - required                                                                                  | `nil`                                                        |
| `config.sessionTermination.enabled`       | Enable session termination, ex. OKTA, Keycloak                                                                       | `false`                                                      |
| `config.sessionTermination.apiURL`        | API URL                                                                                                              | `nil`                                                        |
| `config.sessionTermination.apiToken`      | API Token                                                                                                            | `nil`                                                        |
| `config.logForwording.enabled`            | Enable [log forwarding](https://docs.akeyless.io/docs/ssh-log-forwarding)                                            | `false`                                                      |
| `config.logForwording.settings`           | Log forwarding configuration                                                                                         | `nil`                                                        |
                       
