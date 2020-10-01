---
layout: post
title:  "Google Cloud Run GitHub Actions"
date:   2020-10-01 22:01:00 +1200
categories: Google Cloud Run
---
## Google Cloud Run GitHub Actions

To automate the process of building and deploying images to Google Cloud Run, we can utilize Google Cloud Run GitHub Action. 

Related documentation is available [here](https://github.com/GoogleCloudPlatform/github-actions/blob/master/example-workflows/cloud-run/README.md).

# Pre-requisites

1. [Create a Google Cloud Service Account](https://cloud.google.com/iam/docs/creating-managing-service-accounts)

2. Add the following Cloud IAM roles

`Cloud Run Admin`

`Cloud Build Editor`

`Cloud Build Service Account`

`Viewer`

`Service Account User`

3. [Create a Service Account JSON key](https://cloud.google.com/iam/docs/creating-managing-service-account-keys)

4. Add a GitHub Secret to your repository:

`RUN_SA_KEY` : Content of service account JSON Key that was generated in previous step.

5. Set workflow variables

`PROJECT_ID` : Google Cloud Project ID

`SERVICE_NAME` : Service Name. This name will be used as the image name and service name.

# Example GitHub Action Workflow

```
    # Setup GCloud CLI
    - uses: GoogleCloudPlatform/github-actions/setup-gcloud@master
      with:
        version: '286.0.0'
        service_account_key: ${{ secrets.RUN_SA_KEY }}
        project_id: $PROJECT_ID

    # Build and push image to Google Container Registry
    - name: Build
      run: |-
        gcloud builds submit \
          --quiet \
          --tag "gcr.io/$PROJECT_ID/$SERVICE_NAME:$GITHUB_SHA"

    # Deploy image to Cloud Run
    - name: Deploy
      run: |-
        gcloud run deploy "$SERVICE_NAME" \
          --quiet \
          --region "$RUN_REGION" \ # E.g. centralus1
          --image "gcr.io/$PROJECT_ID/$SERVICE_NAME:$GITHUB_SHA" \
          --platform "managed" \
          --allow-unauthenticated
```
