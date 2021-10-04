# CN-Series Helm Chart ⛵⎈ 

This repository contains charts and templates for deploying the Palo Alto Networks CN-series containerized firewall using the [Helm Package Manager for Kubernetes](https://helm.sh)

The following YAML set are rendered correspondingto the PanOS version.

10.0.0&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;1.0.2<br />

10.1.2&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;2.0.2<br />      

10.1.0/10.1.2&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;2.0.0/2.0.1<br />



## Minimum requirements

* CN-Series
  * CN-Series 10.x.x container images
* Panorama
  * [Panorama](https://www.paloaltonetworks.com/network-security/panorama) 10.x.x
  * Kubernetes plugin for Panorama version 1.0.x,2.0.x
  * Panorama must be [accessible](https://docs.paloaltonetworks.com/pan-os/9-1/pan-os-admin/firewall-administration/reference-port-number-usage/ports-used-for-panorama.html) from the Kubernetes cluster
* Kubernetes
  * Kubernetes 1.16 - 1.21 cluster
  * A current kubeconfig file
* Helm
  * [Helm 3.6.0](https://helm.sh/docs/intro/install/) client

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

Use the directory ```helm_cnv1_10_0 for cnv1 10.0.x as a daemon set, helm_cnv1_10_1 for cnv1 10.1.0/10.1.1 as a daemon set, helm_cnv1_10_1_2 for cnv1 10.1.2 with/without multus for eks|aks|gke| and helm_cnv2 for deploying cnv2 as a service```.

4. Edit the `values.yaml` file and plug in your specific configs

```yaml
# The K8s environment 
# Valid deployTo tags are: [gke|eks|aks|openshift|native]
# Valid multus tags are : [enable|disable] Keep the multus as enable for openshift and native deployments.
# Multus option is valid only for cnv1 10.1.2.
cluster:
  deployTo: eks
    multus: disable

# Firewall tags
# Valid licenceBundle tags are: [basic|bundle1|bundle2] Valid only for 10.0.x
# Valid operationMode tags are: [daemonset|k8-service]    
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
 initImage:  docker.io/acmewidgets/pan_cn_mgmt_init
 initVersion: 1.0.0
 image: docker.io/acmewidgets/panos_cn_mgmt
 version: 10.0.0
 cpuLimit: 4

# DP container tags
dp:
 image: docker.io/acmewidgets/panos_cn_ngfw
 version: 10.0.0
 cpuLimit: 2

# CNI container tags
cni:
 image: docker.io/acmewidgets/pan_cni
 version: 1.0.0
 ```

5. To view the rendered YAMLs

```bash
helm install --debug --generate-name helm_cnv1_10_1/ --dry-run
```

6. To deploy the helm charts

```bash
helm install <deployment-name> directory
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
cn-series/cn-series	1.0.0        	10.0.0      	Palo Alto Networks CN-Series firewall Helm char...
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
