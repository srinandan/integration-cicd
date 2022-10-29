# integration-cicd

This sample repository demonstrates how one could build a CI/CD pipeline for [Application Integration](https://cloud.google.com/application-integration/docs/overview) with [Cloud Build](https://cloud.google.com/build/docs).

# Instructions

* The integration version JSON is downloaded [here](./src/sample.json). 
* Once the changes are checked in and merged, the developer can deploy manually or trigger the deployment through Cloud Build
* This repo uses a custom cloud builder called `integrationcli-builder`, based on [integrationcli](https://github.com/srinandan/integrationcli) and gcloud


## Steps

1. Download the integration from the UI or using the [CLI](https://github.com/srinandan/integrationcli/blob/main/docs/integrationcli_integrations_versions_get.md). Here is an example to download via CLI:

```sh

token=$(gcloud auth print-access-token)
integrationcli integrations versions get -n <integration-name> -v <version> -p <project-id> -r <region-name> -t $token > ./src/<integration-name>.json
```

You can also download via a snapshot number like this:

```sh

token=$(gcloud auth print-access-token)
integrationcli integrations versions get -n <integration-name> -s <snapshot> -p <dev-project-id> -r <region-name> -t $token > ./src/<integration-name>.json
```

2. Author overrides (specific for the environment) and store them in the overrides folder. Here is an example overrides for the URL in the REST task

```json
{
    "task_overrides": [{
        "taskId": "1",
        "parameters":  {
            "url": {
                "key": "url",
                "value": {
                    "stringValue": "https://httpbin.org/ip"
                }
            }
        }
    }]
}
```

3. Trigger the build manually

```sh

gcloud builds submit --config=cloudbuild.yaml --region=<region-name> --project=<qa-project-name>
```

The integration is labeled with the `SHORT_SHA`, the first seven characters of the commit id

### Integration Cloud Builder

This repo uses a custom cloud builder based. You can create the custom cloud builder from

1. The [cloudbuild.yaml](https://github.com/srinandan/integrationcli/blob/main/cloud-builder.yaml) file
2. The [Dockerfile](https://github.com/srinandan/integrationcli/blob/main/Dockerfile.builder)

```sh

git clone https://github.com/srinandan/integrationcli.git
gcloud builds submit --config=cloud-builder.yaml --project=project-name
```

Be sure to modify the [cloudbuild.yaml](./cloudbuild.yaml) file to point to the correct GCR repo.

___

## Support

This is not an officially supported Google product
