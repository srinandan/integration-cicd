# integration-cicd

This sample repository demonstrates how one could build a CI/CD pipeline for Application Integration with Cloud Build.

# Instructions

* The integration version JSON is downloaded [here](./src/sample.json). 
* Once the changes are checked in and merged, the developer can trigger a deploy manually or triggered through Cloud Build
* This repo uses a custom cloud builder called `integrationcli-builder`, based on [integrationcli](https://github.com/srinandan/integrationcli)


## Steps

1. Download the integration from the UI or using the [CLI](https://github.com/srinandan/integrationcli/blob/main/docs/integrationcli_integrations_versions_get.md). Here is an example to download via CLI:

```sh

token=$(gcloud auth print-access-token)
integrationcli integrations versions get -n sample -v <version> -p <project-id> -r <region-name> -t $token > ./src/sample.json
```

2. Trigger the build manually

```sh

gcloud builds submit --config=cloudbuild.yaml --region=region-name --project=project-name --substitutions _NAME=sample,_SNAPSHOT=1,_FILE=./src/sample.json
```

___

## Support

This is not an officially supported Google product