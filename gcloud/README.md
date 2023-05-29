# gcloud

The `gcr.io/cloud-builders/gcloud` image is a lightweight version of gcloud
maintained by the Cloud Build team. This image contains only the components
relevant to Cloud Build – a subset of the tools available in the full
`cloud-sdk` image. Because `gcr.io/cloud-builders/gcloud` is smaller, it lets
you run builds faster. 

Additionally, `gcr.io/cloud-builders/gcloud` supports pre-caching on worker
images. When Cloud Build creates a worker, it pre-caches all of the
`cloud-builder` images for you, so that your build steps have these resources
immediately available at build time.

## `gcloud` vs `gcloud-slim`

There are two variants of the `gcloud` builder in this repository:

* `gcloud` installs all optional gcloud components, and is much larger.
* `gcloud-slim` installs only the `gcloud` CLI and no components, and is
  smaller.

## Limitations

The `gcr.io/cloud-builders/gcloud` image may not support the most recent
versions of `gcloud`. The Cloud Build team doesn't provide historical pinned versions of
`gcloud` or support across multiple platforms.

## Alternatives

The Cloud SDK team maintains a `cloud-sdk` image that supports multiple tagged
versions of Cloud SDK across multiple platforms. Please visit
https://github.com/GoogleCloudPlatform/cloud-sdk-docker for details.

Suggested `cloud-sdk` images include:

    gcr.io/google.com/cloudsdktool/cloud-sdk
    gcr.io/google.com/cloudsdktool/cloud-sdk:alpine
    gcr.io/google.com/cloudsdktool/cloud-sdk:debian_component_based
    gcr.io/google.com/cloudsdktool/cloud-sdk:slim
    google/cloud-sdk
    google/cloud-sdk:alpine
    google/cloud-sdk:debian_component_based
    google/cloud-sdk:slim

Please note that the `gcloud` entrypoint must be specified if you are using these images.

When executed in the Cloud Build environment, commands are executed with
credentials of the [builder service
account](https://cloud.google.com/cloud-build/docs/permissions) for the build
project.

To migrate to the Cloud SDK team's official image, please make the following changes
to your `cloudbuild.yaml`:

```
- name: 'gcr.io/cloud-builders/gcloud'
+ name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
+ entrypoint: 'gcloud'
```

-------

## Examples

The following example demonstrates a build request that uses `gcloud`.

### Clone a Cloud Source Repository using `gcloud`

This `cloudbuild.yaml` invokes `gcloud source repos clone` to clone the
`default` Cloud Source Repository.

```
steps:
- name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  args: ['gcloud', 'source', 'repos', 'clone', 'default']
```
