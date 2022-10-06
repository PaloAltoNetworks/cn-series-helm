# CN-Series Helm Chart ⛵⎈ 

This repository contains charts and templates for deploying the Palo Alto Networks CN-series containerized firewall using the [Helm Package Manager for Kubernetes](https://helm.sh)

The Helm Charts support 10.1.x and 10.2.x PanOS versions. The Helm Charts is based on v3.0 yaml set which can be found at https://github.com/PaloAltoNetworks/Kubernetes/tree/v3.0.3

The Release Notes and Deployment Guide is at https://docs.paloaltonetworks.com/cn-series/cn-series-firewall-release-notes/cn-series-firewall-release-notes


## Minimum requirements

* CN-Series
  * CN-Series 10.1.x container images
* Panorama
  * [Panorama](https://www.paloaltonetworks.com/network-security/panorama) 10.1.x
  * Kubernetes plugin for Panorama version 1.0.x,2.0.x
  * Panorama must be [accessible](https://docs.paloaltonetworks.com/pan-os/9-1/pan-os-admin/firewall-administration/reference-port-number-usage/ports-used-for-panorama.html) from the Kubernetes cluster
* Kubernetes
  * Kubernetes 1.16 - 1.24 cluster
  * A current kubeconfig file
* Helm
  * [Helm 3.6+](https://helm.sh/docs/intro/install/) client

## Usage

### Method 1 - With Repo

1. [Generate the VM authorization key on Panorama](https://docs.paloaltonetworks.com/vm-series/9-1/vm-series-deployment/bootstrap-the-vm-series-firewall/generate-the-vm-auth-key-on-panorama.html)

2. Clone the repository from GitHub

```bash
$ git clone https://github.com/PaloAltoNetworks/cn-series-helm.git
```

3. Change into the repo directory

```bash
$ cd cn-series-helm
```

```
helm_cnv1 are charts that deploy as a daemon set
helm_cnv2 are charts that deploy as a service
helm_cnv3 are charts that deploy as a cnf
```


4. Edit the `values.yaml` file and plug in your specific configs. 
Make sure to read through the values.yaml to chose the specific deployment tyoe and additional configurations.

Use the public-facing CN-Series repository for images from https://console.cloud.google.com/gcr/images/pan-cn-series/GLOBAL 

Below is an example of `values.yaml` for cnv1

```yaml
# The K8s environment 
# Valid deployTo tags are: [gke|eks|aks|openshift|native]
# Valid multus tags are : [enable|disable] Keep the multus as enable for openshift and native deployments.
cluster:
  deployTo: eks
  multus: disable

# Panorama tags
panorama:
  ip: panorama.acmewidgets.com
  ip2: 
  authKey: "000000000000000"
  deviceGroup: my-devicegroup
  template: my-stack
  cgName: my-collector

# MP container tags
mp:
 initImage: gcr.io/pan-cn-series/pan_cn_mgmt_init
 initVersion: latest
 image: gcr.io/pan-cn-series/panos_cn_mgmt
 version: 10.2.3
 cpuLimit: 4

# DP container tags
dp:
 image: gcr.io/pan-cn-series/panos_cn_ngfw
 version: 10.2.3
 cpuLimit: 2

# CNI container tags
cni:
 image: gcr.io/pan-cn-series/pan_cni
 version: latest
 ```

5. To view the rendered YAMLs

```bash
helm install --debug --generate-name helm_cnv1/ --dry-run
```
Do a lint check on the helm charts

```bash
helm lint helm_cnv1/
```

6. To deploy the helm charts

```bash
helm install <deployment-name> helm_cnv1
```

### Method 2 - Without Repo 

1. [Generate the VM authorization key on Panorama](https://docs.paloaltonetworks.com/vm-series/9-1/vm-series-deployment/bootstrap-the-vm-series-firewall/generate-the-vm-auth-key-on-panorama.html)

2. Add the cn-series repo to your local Helm client

```bash
$ helm repo add my-project https://paloaltonetworks.github.io/cn-series-helm
"cn-series" has been added to your repositories
```

3. Confirm the repo has been added to your Helm client

```
$ helm search repo cn-series
NAME               	CHART VERSION	APP VERSION	DESCRIPTION
cn-series/cn-series	2.0.0        	10.2.0      	Palo Alto Networks CN-Series firewall Helm char...
```

4. Select the Kubernetes cluster

```bash
$ kubectl config set-cluster NAME
```

5. Deploy using the Helm chart repo

```bash
$ helm install cn-series/cn-series --name="deployment name" \
--set cluster.deployTo="gke|eks|aks|openshift"
--set cluster.multus="enable|disable"
--set panorama.ip="panorama hostname or ip" \
--set panorama.ip2="panorama2 hostname or ip" \
--set-string panorama.authKey="vm auth key" \
--set panorama.deviceGroup="device group" \
--set panorama.template="template stack" \
--set panorama.cgName="collector group" \
--set cni.image="container repo" \
--set cni.version="container version" \
--set mp.initImage="container repo" \
--set mp.initVersion="container version" \
--set mp.image="container repo" \
--set mp.version="container version" \
--set mp.cpuLimit="cpu max" \
--set dp.image="container repo" \
--set dp.version="container version" \
--set dp.cpuLimit="cpu max"
```
Add additional parameters to the above command with respect to your desired deployment and configuration.