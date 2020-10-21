## Preparation

Currently, the helm chart requires a storage class with ReadWriteMany access modes.  
Since a storage class is more environment specific, you will need to provide one before proceeding.
In addition, please provide 2 PersistentVolumes with `persistentVolumeReclaimPolicy: retain` and reference those PVs in the `values.yaml` file

e.g when running on AWS with EKS:
https://docs.aws.amazon.com/eks/latest/userguide/efs-csi.html

#### Preqrequisites

##### Horizonal Auto-Scaling
Horizontal auto-scaling is based on the HorizonalPodAutoscaler object.  
For it to work properly, Kubernetes metrics server must be installed in the cluster - https://github.com/kubernetes-sigs/metrics-server

## Installing

The `values.yaml` file holds default values, replace the values with the ones from your environment where needed.  

To install the chart, clone the repo and run:
```bash
helm install api-gateway akeyless-services/ -f akeyless-services/values.yaml
``` 