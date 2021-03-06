# Convox Release Buildkite Plugin [![Changelog](https://img.shields.io/badge/-Changelog-blue)](./CHANGELOG.md)

A [Buildkite plugin](https://buildkite.com/docs/agent/v3/plugins) that creates Convox releases.

- Convox release is created via the Convox CLI (`convox build`)
- Build and Release IDs can be saved to metadata, and used in future pipeline steps

## Example

The following pipeline will build a release for the `test-app` app in the `test-rack` rack, equivalent to running `convox build --rack=test-rack --app=test-app`:

```yaml
steps:
  - plugins:
    - liamdawson/convox-build#v1.0.0:
        rack: test-rack
        app: test-app
```

You can store the Convox Build/Release IDs in metadata and use them later:

```yaml
steps:
  - plugins:
    - liamdawson/convox-build#v1.0.0:
        rack: test-rack
        app: test-app
        metadata:
          release-id: convox-release-id

  - wait:

  - command: buildkite-agent meta-data get convox-release-id
```

## Troubleshooting

### Manifest not found

```shell
$ convox build --rack myrack --app myapp
Packaging source... OK
Uploading source... OK
Starting build... OK
ERROR: open convox.yml: no such file or directory
```

As of Convox CLI `3.0.39`, listing the manifest file in a local `.dockerignore` prevents the file from being loaded by the build process, even if it can be found via a local `stat` or similar. This behaviour has not been observed in version `20200903164833`.
