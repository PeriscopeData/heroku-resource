# heroku-resource
Heroku resource for concourse. Can build slugs from a directory and promote apps in a pipeline.
# Build
- get: version
- get: source-dir
- put: heroku
  timeout: 10m
  attempts: 1
  params:
    bucket: {{bucket}}
    tar_args: --exclude extras/go --exclude doc/ --exclude .git/a
    build_dir: source-dir
    buildpacks:
    - url: https://github.com/heroku/heroku-buildpack-ruby
    slug_version: version/number

# Deploy
- put: heroku
  timeout: 1m
  attempts: 1
  params:
    pipeline_id: {{pipeline-id}}
    source_app_id: {{source-app-id}}
    target_app_id: {{target-app-id}}

- name: heroku-resource
  type: docker-image
  source:
    repository: periscopedata/heroku-resource
    tag: latest

- name: heroku
  type: heroku-resource
  source:
    heroku_api_key: {{heroku-api-key}}
    heroku_app_name: {{heroku-app-name}}
