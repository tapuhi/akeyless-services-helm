# akeyless-services-helm

This repo holds a helm chart for deploying API Gateway in a kubernetes cluster.  

## Preparation

Currently, the helm chart requires a storage class with ReadWriteMany access modes.  
Since a storage class is more environment specific, you will need to provide one before proceeding.  

e.g when running on AWS with EKS:
https://docs.aws.amazon.com/eks/latest/userguide/efs-csi.html

## Installing

The `values.yaml` file holds default values, replace the values with the ones from your environment where needed.  

To install the chart, clone the repo and run:
```bash
helm install api-gateway akeyless-services/ -f akeyless-services/values.yaml
``` 