# CN-series 🚀 Helm Chart ⛵ 

This repo is for deploying CN-series using [Helm Package Manager for Kubernetes](https://helm.sh)

## Steps 🔢

1. Install Panorama

2. Get the Panorama IP and authKey

3. Download this repo (git clone or download the zip), and `cd cn-series-helm`

4. Update the `values.yaml` file with proper configuration

5. Make sure your kubernetes cluster is reachable and your kubeconfig file is properly set

6. Dryrun to validate the helm deployment using:

 `helm install --dry-run --debug gunjan-cn-series .`

7. If the dryrun is successful, you can deploy the CN-series helm chart using:

`helm install gunjan-cn-series --set-string panorama.authKey="<insert auth key>" --set panorama.ip="34.68.228.203" .` 




## How to Install Helm ⚒️

Follow this guide https://helm.sh/docs/intro/install/


## Support Policy

**Community-Supported aka Best Effort**

These Kubernetes YAML files are released under an as-is, best effort, support policy as part of the Beta Program. These scripts should be seen as community supported and Palo Alto Networks will contribute our expertise as and when possible. We do not provide technical support or help in using or troubleshooting the components of the project through our normal support options such as Palo Alto Networks support teams, or ASC (Authorized Support Centers) partners and backline support options. The underlying product used (the CM-Series firewall) by the scripts or templates are still supported, but the support is only for the product functionality and not for help in deploying or using the template or script itself. Unless explicitly tagged, all projects or work posted in our GitHub repository (at https://github.com/PaloAltoNetworks) or sites other than our official Downloads page on https://support.paloaltonetworks.com are provided under the best effort policy.