# Upload Backstage Techdocs

## Example Workflow

```yaml
name: Publish TechDocs

on:
  push:
    branches:
    - main
    paths:
    - mkdocs.yml
    - 'docs/**/*'
  workflow_dispatch:

jobs:
  publish:
    timeout-minutes: 5
    steps:
    - uses: actions/checkout@v4
    - uses: elisa-actions/upload-backstage-techdocs@v1
      with:
        entity: default/Component/my-entity
        s3-access-key-id: ${{ secrets.BACKSTAGE_ACCESS_KEY_ID }}
        s3-secret-access-key: ${{ secrets.BACKSTAGE_SECRET_ACCESS_KEY }}
        s3-bucket: ${{ secrets.BACKSTAGE_BUCKET }}
```

## Techdocs Configuration

You can use the following example as basis of your `mkdocs.yml` configuration file. If you don't want to store the file
into your repository root, you can define the file location with `source-dir` parameter for the action.

```yaml
# (mandatory) Enable MkDocs plugins - techdocs-core is required for Backstage integration.
plugins:
  - techdocs-core
# (mandatory) Define the name of your site, e.g. your project/component name.
site_name: Example
# (optional) Configure the Backstage TechDocs metadata
repo_url: https://github.companyname.com/<organisation>/<repository>/tree/main/docs
edit_uri: https://github.companyname.com/<organisation>/<repository>/edit/main/docs
# (optional) Define the base path of your documentation. Defaults to 'docs'
docs_dir: docs
# (optional) Define the navigation structure of your site. Useful if you have multiple documentation pages or want to skip some of them.
nav:
  - Home: README.md
# (optional) Configure markdown extensions. Custom fences for Mermaid are required only for non-Backstage rendering.
markdown_extensions:
  pymdownx.extra:
    pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format ''
```
