# CN-Series Helm Chart ⛵⎈ 

This repo is for deploying the CN-series containerized firewall using the [Helm Package Manager for Kubernetes](https://helm.sh)

## Prerequisites

* Panorama
  * Panorama 9.2.0 or greater
  * Kubernetes plugin for Panorama version 1.0.0 or greater
  * [Panorama is accessible from the Kubernetes cluster](https://docs.paloaltonetworks.com/pan-os/9-1/pan-os-admin/firewall-administration/reference-port-number-usage/ports-used-for-panorama.html)
* Kubernetes
  * Kubernetes 1.14 cluster
  * A working kubeconfig file
* Helm
  * [Helm version 3](https://helm.sh/docs/intro/install/)

## Usage

1. [Generate the VM authorization key on Panorama](https://docs.paloaltonetworks.com/vm-series/9-1/vm-series-deployment/bootstrap-the-vm-series-firewall/generate-the-vm-auth-key-on-panorama.html)

2. Add the cn-series repo to your local Helm client

```
$ helm repo add cn-series https://paloaltonetworks.github.io/cn-series-helm
```

3. Deploy using the Helm chart repo

```
$ helm install cn-series/cn-series --name=my-deployment \
--set panorama.ip="<panorama hostname>" \
--set-string panorama.authKey="<vm auth key>" \
--set panorama.deviceGroup="<device group>" \
--set panorama.template="<template stack>" \
--set panorama.cgName="<collector group>" \
--set cni.confName="<cni name>" \
--set cni.binDir="<bin directory>"
```
Use the following table to determine the `cni.confName` and `cni.binDir` parameters.

| Param     | Value                | Environment|
|-----------|----------------------|------------|
| confName  | 10-calico.conflist   | GKE        |
| binDir    | /home/kubernetes/bin | GKE        |
| confName  | 10-aws.conflist      | EKS        |
| binDir    | /opt/cni/bin         | EKS        |
| confName  | 10-azure.conflist    | AKS        |
| binDir    | /host/opt/cni/bin    | AKS        |
| confName  | k8s.conflist         | minikube   |
| binDir    | /opt/cni/bin         | minikube   |


## Support

This template/solution is released under an as-is, best effort, support policy as part of the Beta Program. These scripts should be seen as community supported and Palo Alto Networks will contribute our expertise as and when possible. We do not provide technical support or help in using or troubleshooting the components of the project through our normal support options such as Palo Alto Networks support teams, or ASC (Authorized Support Centers) partners and backline support options. The underlying product used (the CM-Series firewall) by the scripts or templates are still supported, but the support is only for the product functionality and not for help in deploying or using the template or script itself. Unless explicitly tagged, all projects or work posted in our GitHub repository (at https://github.com/PaloAltoNetworks) or sites other than our official Downloads page on https://support.paloaltonetworks.com are provided under the best effort policy.