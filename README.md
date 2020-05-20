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
