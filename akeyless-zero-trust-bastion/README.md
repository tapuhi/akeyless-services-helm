# Akeyless-Zero-Trust-Bastion

Akeyless Zero Trust Bastion provides zero trust access to remote resources using Akeyless JIT credentials (dynamic secrets and SSH certificate issuers). 

## TL;DR

```bash
$ git clone git@github.com:akeylesslabs/akeyless-services-helm.git
$ cd akeyless-zero-trust-bastion
$ helm install my-release .
```

## Introduction
This chart bootstraps a Akeyless-Zero-Trust-Bastion deployment on a Kubernetes cluster using the Helm package manager.


## Preparation

### Network
Currently, when using DB application (mysql, mongodb.mssql) via the Zero Trust Bastion, it'll only work properly when using
load balancer with "sticky" session:
- Ingress - Make sure to use sticky session annotation, for example `nginx.ingress.kubernetes.io/affinity: "cookie"` in Nginx
- Cloud Provider LB - Make sure to config the LB to support sticky session, for example is AWS, using ELB:   
https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-sticky-sessions.html

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

The following table lists the configurable parameters of the Zero-Trust-Bastion chart, and their default values.

### Deployment parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `image.repository`                        | Zero-Trust-Bastion image name                                                                                        | `akeyless/zero-trust-bastion`                                |
| `image.tag`                               | Zero-Trust-Bastion image tag                                                                                         | `latest`                                                     |      
| `image.pullPolicy`                        | Zero-Trust-Bastion image pull policy                                                                                 | `Always`                                                     |  
| `image.dockerRepositoryCreds`             | Akeyless docker repository credentials                                                                               | `nil`                                                        |
| `updateStrategy`                          | Updating statefulset strategy                                                                                        | `RollingUpdate`                                              |  
| `containerName`                           | Zero-Trust-Bastion container name                                                                                    | `zero-trust-bastion`                                         |  
| `replicaCount`                            | Number of Zero-Trust-Bastion nodes                                                                                   | `1`                                                          |
| `livenessProbe`                           | Liveness probe configuration for Zero-Trust-Bastion                                                                  | Check `values.yaml` file                                     |                   
| `readinessProbe`                          | Readiness probe configuration for Zero-Trust-Bastion                                                                 | Check `values.yaml` file                                     |         
| `resources.limits`                        | The resources limits for Zero-Trust-Bastion containers                                                               | `{}`                                                         |
| `resources.requests`                      | The requested resources for Zero-Trust-Bastion containers                                                            | `{}`                                                         |


### Exposure parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `service.type`                            | Kubernetes Service type                                                                                              | `LoadBalancer`                                               |
| `service.port`                            | API port                                                                                                             | `8888`                                                       |
| `ingress.enabled`                         | Enable ingress resource                                                                                              | `false`                                                      |
| `ingress.path`                            | Path for the default host                                                                                            | `/`                                                          |
| `ingress.certManager`                     | Add annotations for cert-manager                                                                                     | `false`                                                      |
| `ingress.hostname`                        | Default host for the ingress resource                                                                                | `zero-trust-bastion.local`                                   |
| `ingress.annotations`                     | Ingress annotations                                                                                                  | `[]`                                                         |
| `ingress.tls`                             | Enable TLS configuration for the hostname defined at `ingress.hostname` parameter                                    | `false`                                                      |
| `ingress.existingSecret`                  | Existing secret for the Ingress TLS certificate                                                                      | `nil`                                                        |  

### Zero-Trust-Bastion configuration parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `config.rdpRecord.enabled`                | Enable RDP session recording                                                                                         | `false`                                                      |
| `config.rdpRecord.s3.bucketName`          | AWS S3 bucket name to store RDP recording                                                                            | `nil`                                                        |
| `config.rdpRecord.s3.bucketPrefix`        | AWS S3 bucket prefix                                                                                                 | `nil`                                                        |
| `config.rdpRecord.s3.region`              | AWS S3 bucket region                                                                                                 | `nil`                                                        |
| `config.rdpRecord.s3.awsAccessKeyId`      | AWS Access Key ID, not required if using EC2 IAM roles                                                               | `nil`                                                        |
| `config.rdpRecord.s3.awsSecretAccessKey`  | AWS Secret Access Key, not required if using EC2 IAM roles                                                           | `nil`                                                        |
| `config.rdpRecord.existingSecret`         | Specifies an existing secret to be used for bastion, management AWS credentials                                      | `nil`                                                        |

### HPA parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `HPA.enabled`                             | Enable Zero-Trust-Bastion Horizontal Pod Autoscaler                                                                  | `false`                                                      |
| `HPA.minReplicas`                         | Minimum desired number of replicas                                                                                   | `1`                                                          |
| `HPA.maxReplicas`                         | Minimum desired number of replicas                                                                                   | `14`                                                         |
| `HPA.cpuAvgUtil`                          | CPU average utilization                                                                                              | `50`                                                         |
| `HPA.memAvgUtil`                          | Memory average utilization                                                                                           | `50`                                                         |
                                                                                        