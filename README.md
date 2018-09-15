# google-cloud-storage-intro

Intro Google Cloud Storage Lab

## Getting Started

1. Ensure the Following APIs are enabled (enable with gcloud services enable [service]):

    - iam.googleapis.com
    - storage-component.googleapis.com

1. Deploy the deployment manager config in the `infrastructure` directory:

```sh
gcloud deployment-manager deployments create lab --config infrastructure/deployment.yaml
```
