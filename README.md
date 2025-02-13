# salmon-sync

This is a Github Action that syncs a folder to a Google Cloud bucket using `rclone` and then send an authenticated request to the doc site to invalidate the cache for the doc version.
This action is only meant to work for Deephaven's documentation. It could be used in a more general purpose way to sync a folder into any Google cloud bucket (with the proper credentials), but that is subject to change and may break in any version.

## Parameters

```yml
inputs:
  source:
    required: true
    type: string
    description: "The source directory to sync."
  destination:
    required: true
    type: string
    description: "The destination directory to sync. Relative to the bucket. It is recommended to use the GitHub repo path (such as deephaven/salmon-sync) as the minimum base to prevent collisions."
  bucket:
    required: true
    type: string
    description: "The Google Cloud bucket to sync to."
  credentials:
    required: true
    type: string
    description: "The Google Cloud credentials. Should be base64 encoded."
  cache-bust-token:
    required: true
    type: string
    description: "The cache-bust token"
  docs-url:
    required: true
    type: string
    description: "The doc site URL"
```

## Example

The action can be used as a step in a workflow
Here is an example that syncs from the local path `temp/blog` to the blog section of the bucket.

```yml
- name: Sync to the blog
  uses: deephaven/salmon-sync@v1
  with:
    source: temp/blog
    destination: deephaven/deephaven.io/blog
    bucket: ${{ vars.DOCS_PROD_BUCKET }} # or ${{ vars.DOCS_PREVIEW_BUCKET }}
    credentials: ${{ secrets.DOCS_GOOGLE_CLOUD_CREDENTIALS }}
    cache-bust-token: ${{ secrets.DOCS_CACHE_BUST_TOKEN }}
    docs-url: ${{ vars.DOCS_PROD_URL }} # or ${{ vars.DOCS_PREVIEW_URL }}
```
