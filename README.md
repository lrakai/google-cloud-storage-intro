# google-cloud-storage-intro

Intro Google Cloud Storage Lab

## Getting Started

1. Ensure the Following APIs are enabled (enable with gcloud services enable [service]):

    - iam.googleapis.com
    - storage-component.googleapis.com

1. [Ensure the default Google APIs service account (used by deployment manager) has permission to create roles](https://cloud.google.com/deployment-manager/docs/configuration/set-access-control-resources):

    ```sh
    gcloud projects add-iam-policy-binding [PROJECT_ID] \
    --member serviceAccount:[PROJECT_NUMBER]@cloudservices.gserviceaccount.com  \
    --role roles/iam.roleAdmin
    ```

    You can use `gcloud list projects` to get the project ID and number.

1. Deploy the deployment manager config in the `infrastructure` directory:

    ```sh
    gcloud deployment-manager deployments create lab --config infrastructure/deployment.yaml
    ```

1. Bind the Lab role to the student user or group

    ```sh
    gcloud projects add-iam-policy-binding [PROJECT_ID] \
    --member [GROUP_OR_USER]  \
    --role projects/[PROJECT_ID]/roles/studentrole
    ```

    An example of `[GROUP_OR_USER]` is `student@gmail.com`.