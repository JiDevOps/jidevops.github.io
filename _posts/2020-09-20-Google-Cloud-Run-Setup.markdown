---
layout: post
title:  "Build and Deploy to Google Cloud Run"
date:   2020-09-20 17:00:00 +1200
categories: Azure
---
# Google Cloud Run

GCR is a fully managed computing platfor for deploying and scaling containerised applications. This enables developers to not worry about the underlying infrastructure and concentrate on writing portable applications in our favourtie langues such as Python, Java, Go etc.

## Setting up Google Cloud SDK

Google Cloud SDK is a set of tools that you can use to manage resources and applications hosted on Google Cloud.

Install Google Cloud SDK from [here](https://cloud.google.com/sdk/docs/install).

## Build and Deploy

Project ID is the Google Cloud Prpject ID.

Set project configuration.

`gcloud config set project <Project-ID>`

Build the container image using Cloud Build by running the command from directory containing the Docker file.

`gcloud builds submit --tag gcr.io/<Project-ID>/<Image-Name>`

Deploy the container image.

`gcloud run deploy --image gcr.io/<Project-ID>/<Image-Name> --platform managed`

You now have the option to set the service name, select a region e.g. Australia-SouthEast1 and prompted to allow authenticated invocations.

On success the URL is displayed. Visit the service URL to see the deployed container.

Remember to clean up your resources on Google Cloud. Cloud Run does not charge when the service is not in use, but you may be charged for storing the container image in Container Registry.