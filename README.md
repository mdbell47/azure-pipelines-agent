# azure-pipeline-runner

## Build and Push Container Image

`docker login` or `podman login` : If you're changing the `FROM` block

`docker build -t azurerunner:v1 .` or `podman build -t azurerunner:v1 .` : Update `ENV FQDN_OF_AZURE_HOST=` if you need to import a cert to the OS

`podman tag localhost/azurerunner:v1 YourImageRegistryURL.com:v1`

`docker push YourImageRegistryURL.com:v1` or `podman push YourImageRegistryURL.com:v1`



## Option A - Run container locally

`podman run -e AZP_URL="<Azure DevOps instance>" -e AZP_TOKEN="<Personal Access Token>" -e AZP_POOL="<Agent Pool Name>" -e AZP_AGENT_NAME="Docker Agent - Linux" --name "azp-agent-linux" localhost/azurerunner:v1`

------

## Option B - Run on Kubernetes/OpenShift
1. `secret.yaml` - Apply the Secret resource YAML then manually modify it afterwards with your values:

   - `kubect apply --namespace yourNamespace -f manifests/secret.yaml`

      or

   - `oc apply --namespace yourNamespace -f manifests/secret.yaml`



2. `deployment.yaml` - Update `image` and `imagePullSecrets` values and Apply the Deployment Resource YAML


   - `kubect apply --namespace yourNamespace -f manifests/deployment.yaml`

      or

   - `oc apply --namespace yourNamespace -f manifests/deployment.yaml`