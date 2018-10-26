# google-cloud-storage-intro

Intro Google Cloud Storage Lab. Learn how to perform common tasks using `gsutil`.

![Final Environment](https://user-images.githubusercontent.com/3911650/47577929-55a8f480-d905-11e8-8f46-461c25715ea4.png)

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

    - In macOS/Linux:

        ```sh
        member="[GROUP_OR_USER]"
        project_id=$(gcloud config list --format 'value(core.project)')
        role=$(gcloud iam roles list --project $project_id \
                                     --filter "name:projects/$project_id/roles/studentrole*" \
                                     --format "value(name)")
        gcloud projects add-iam-policy-binding $project_id \
        --member $member  \
        --role $role
        ```

    - In Windows (PowerShell):

        ```ps1
        $member = "[GROUP_OR_USER]"
        $project_id = gcloud config list --format 'value(core.project)'
        $role = gcloud iam roles list --project $project_id `
                                      --filter "name:projects/$project_id/roles/studentrole*" `
                                      --format "value(name)"
        gcloud projects add-iam-policy-binding $project_id `
        --member $member  `
        --role $role
        ```

    An example of `[GROUP_OR_USER]` is `user:student@gmail.com`.
    
## Following Along

1. Start a Google Cloud Shell session.
1. Create a bucket (replace _[BUCKET_NAME]_ with the desired name):

    ```sh
    bucket_name=[BUCKET_NAME]
    gsutil mb -c multi_regional -l US gs://$bucket_name
    ```

1. Upload the Cloud Shell README to the bucket:

    ```sh
    gsutil cp -s regional README-cloudshell.txt gs://$bucket_name/    
    ```

1. Perform a detailed listing of the bucket to confirm the storage class and other details:

    ```sh
    gsutil ls -L gs://$bucket_name/* | more
    ```

1. Enable versioning on the bucket:

    ```sh
    gsutil versioning set on gs://$bucket_name
    ```

1. Overwrite the README and observe the new __Generation__ number for the object:

    ```sh
    gsutil cp README-cloudshell.txt gs://$bucket_name/
    gsutil ls -L gs://$bucket_name/* | more
    ```

1. List all available versions for the object:

    ```sh
    gsutil ls -a gs://$bucket_name/plan.txt
    ```
    Note the generation number is appended to identify different versions of the object.

## Tearing Down

When finished, remove the GCP resources with:

```sh
gsutil rm -r gs://$bucket_name
gsutil rb gs://$bucket_name
gcloud projects remove-iam-policy-binding [PROJECT_ID] \
    --member [GROUP_OR_USER]  \
    --role projects/[PROJECT_ID]/roles/studentrole
gcloud deployment-manager deployments delete lab
```
