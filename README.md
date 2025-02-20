# salmon-sync

This is a Github Action that syncs a folder to a S3 bucket using `rclone`.
This action is only meant to work for Deephaven's documentation.

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
  production:
    required: true
    type: boolean
    description: "If true, the files will be deployed to the production site. Otherwise they will be deployed to the preview site."
  temporary:
    required: true
    type: boolean
    description: "If true, the files will be marked as temporary and deleted after 14 days. Otherwise they will persist in S3 indefinitely."
  aws-role:
    required: true
    type: string
    description: "The AWS role to assume."
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
    production: true # false for pr previews
    temporary: false # true will delete non-production files after 14 days
    aws-role: ${{ vars.DOCS_AWS_ROLE }}
```
