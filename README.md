# salmon-sync

This is a Github Action that syncs a folder to a S3 bucket using `rclone`.
This action is only meant to work for Deephaven's documentation.

## Parameters

> [!IMPORTANT]
> This action uses OIDC to authenticate with AWS.
> Your job must include the `id-token: write` permission and specify any other permissions it needs.
> See the [GitHub docs](https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication#permissions-for-the-github_token) for more info.

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
  aws-role:
    required: true
    type: string
    description: "The AWS role to assume."
  production:
    required: false
    default: "false"
    type: boolean
    description: "If true, the files will be deployed to the production site. Otherwise they will be deployed to the preview site."
  temporary:
    required: false
    default: "true"
    type: boolean
    description: "If true, the files will be marked as temporary and deleted after 14 days. Otherwise they will persist in S3 indefinitely."
```

## Example

The action can be used as a step in a workflow
Here is an example that syncs from the local path `temp/blog` to the blog section of the bucket.

```yml
jobs:
  upload-to-salmon:
    runs-on: ubuntu-24.04
    permissions:
      id-token: write # Needed to authenticate with AWS
      contents: read # If you want to checkout the repo
    steps:
      - name: Checkout the repo
        uses: actions/checkout@v4

      - name: Build docs
        run: echo "Build docs here"

      - name: Sync docs
        uses: deephaven/salmon-sync@v1
        with:
          source: temp/blog
          destination: deephaven/deephaven.io/blog
          production: true # false for pr previews
          temporary: false # true will delete non-production files after 14 days
          aws-role: ${{ vars.DOCS_AWS_ROLE }}
```
