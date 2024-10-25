# salmon-sync

This is a Github Action that syncs a folder to a Google Cloud bucket using `rclone`.
This action is only meant to work for Deephaven's documentation. It could be used in a more general purpose way to sync a folder into any Google cloud bucket (with the proper credentials), but that is subject to change and may break in any version.

## Parameters
```
inputs:
  source:
    required: true
    type: string
    description: 'The source directory to sync.'
  destination:
    required: true
    type: string
    description: 'The destination directory to sync. Relative to the bucket.'
  project_number:
    required: true
    type: string
    description: 'The Google Cloud project number.'
  bucket:
    required: true
    type: string
    description: 'The Google Cloud bucket to sync to. It is recommended to use the GitHub repo path (such as deephaven/salmon-sync) as the minimum base to prevent collisions.'
  credentials:
    required: true
    type: string
    description: 'The Google Cloud credentials. Should be base64 encoded.'
```

## Example
The action can be used as a step in a workflow
Here is an example that syncs from the local path `temp/blog` to the blog section of the bucket.
```
- name: Sync to the blog
  uses: deephaven/salmon-sync@v1
  with:
    source: temp/blog
    destination: deephaven/deephaven.io/blog
    project_number: ${{ secrets.DOCS_GOOGLE_CLOUD_PROJECT_NUMBER}}
    bucket: ${{ vars.DOCS_GOOGLE_CLOUD_BUCKET }}
    credentials: ${{ secrets.DOCS_GOOGLE_CLOUD_CREDENTIALS }}
```
