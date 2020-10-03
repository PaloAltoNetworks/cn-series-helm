# CN-Series Helm Chart ⛵⎈ 

This repository contains charts and templates for deploying the Palo Alto Networks CN-series containerized firewall using the [Helm Package Manager for Kubernetes](https://helm.sh)

## Minimum requirements

* CN-Series
  * CN-Series 10.0.0 container images
* Panorama
  * [Panorama](https://www.paloaltonetworks.com/network-security/panorama) 10.0.0
  * Kubernetes plugin for Panorama version 1.0.0
  * Panorama must be [accessible](https://docs.paloaltonetworks.com/pan-os/10-0/pan-os-admin/firewall-administration/reference-port-number-usage/ports-used-for-panorama.html) from the Kubernetes cluster
* Kubernetes
  * Kubernetes 1.13 - 1.18 cluster
  * A current kubeconfig file
* Helm
  * [Helm 3](https://helm.sh/docs/intro/install/) client

A full list of supported Kubernetes environments may be found here: 

[https://docs.paloaltonetworks.com/cn-series/10-0/cn-series-deployment/cn-series-firewall-for-kubernetes/cn-series-deployment-environments.html](https://docs.paloaltonetworks.com/cn-series/10-0/cn-series-deployment/cn-series-firewall-for-kubernetes/cn-series-deployment-environments.html)

## Usage

### Method 1 - With Repo

1. [Generate the VM authorization key on Panorama](https://docs.paloaltonetworks.com/vm-series/10-0/vm-series-deployment/bootstrap-the-vm-series-firewall/generate-the-vm-auth-key-on-panorama.html)

2. Clone the repository from GitHub

```bash
$ git clone https://github.com/PaloAltoNetworks/cn-series-helm.git
```

3. Change into the repo directory

```bash
$ cd cn-series-helm
```

4. Edit the `values.yaml` file and plug in your specific configs

```yaml
# The K8s environment 
# Valid deployTo tags are: [gke|eks|aks|openshift]
cluster:
  deployTo: gke

# Firewall tags
# Valid licenceBundle tags are: [basic|bundle1|bundle2]
firewall:
 operationMode: daemonset
 failoverMode: failopen
 licenseBundle: bundle2

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
 initImage:  docker.io/paloaltonetworks/pan_cn_mgmt_init
 initVersion: latest
 image: docker.io/paloaltonetworks/panos_cn_mgmt
 version: latest
 cpuLimit: 4

# DP container tags
dp:
 image: docker.io/paloaltonetworks/panos_cn_ngfw
 version: latest
 cpuLimit: 2

# CNI container tags
cni:
 image: docker.io/paloaltonetworks/pan_cni
 version: latest
 ```

5. Install the Helm chart

```bash
$ helm install my-deployment .
```


### Method 2 - Without Repo 

1. [Generate the VM authorization key on Panorama](https://docs.paloaltonetworks.com/vm-series/9-1/vm-series-deployment/bootstrap-the-vm-series-firewall/generate-the-vm-auth-key-on-panorama.html)

2. Add the cn-series repo to your local Helm client

```bash
$ helm repo add paloaltonetworks https://paloaltonetworks.github.io/cn-series-helm
"cn-series" has been added to your repositories
```

3. Confirm the repo has been added to your Helm client

```
$ helm search repo cn-series
NAME               	         CHART VERSION	 APP VERSION	  DESCRIPTION
paloaltonetworks/cn-series	 0.1.5        	 10.0.0      	  Palo Alto Networks CN-Series firewall Helm char...
```

4. Select the Kubernetes cluster

```bash
$ kubectl config set-cluster NAME
```

5. Deploy using the Helm chart repo

```bash
$ helm install my-deployment paloaltonetworks/cn-series \
--set cluster.deployTo="gke|eks|aks|openshift"
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

## Support

This template/solution is released under an as-is, best effort, support
policy. These scripts should be seen as community supported and Palo
Alto Networks will contribute our expertise as and when possible. We do
not provide technical support or help in using or troubleshooting the
components of the project through our normal support options such as
Palo Alto Networks support teams, or ASC (Authorized Support Centers)
partners and backline support options. The underlying product used (the
VM-Series firewall) by the scripts or templates are still supported, but
the support is only for the product functionality and not for help in
deploying or using the template or script itself.

Unless explicitly tagged, all projects or work posted in our GitHub
repository (at <https://github.com/PaloAltoNetworks>) or sites other
than our official Downloads page on <https://support.paloaltonetworks.com>
are provided under the best effort policy.
