# Spark Plugin Catalog

The `spark` branch is the catalog Spark servers read as **Spark maintained**:
every plugin in Silo's maintained catalog, plus Spark's own plugins. `main`
mirrors `Silo-Server/silo-plugins` untouched; `sync-and-build.sh` in
`Joloxx9/silo-mac-build` fast-forwards it and merges it into `spark`.

Spark's own plugins in this catalog:

- `spark.autoscan.arr` – [spark-plugin-autoscan-arr](https://github.com/Joloxx9/spark-plugin-autoscan-arr)
- `silo.watchprovider.scrob` – [silo-plugin-watchprovider-scrob-local](https://github.com/Joloxx9/silo-plugin-watchprovider-scrob-local), Spark's Scrob build

To add or update one, run `go run ./cmd/update-catalog -repo <owner/name> -tag <tag>`
on `spark` and commit `manifest.json`.

---

# Silo Plugin Catalog

Catalog metadata and helpers for first-party Silo plugins.

Silo servers consume the generated `manifest.json` to discover approved plugin
releases, supported platforms, checksums, capabilities, and presentation
metadata. Source plugin repositories remain the authority for implementation
and release artifacts.

## Catalog updates

Plugin repositories should dispatch `plugin_release_published` after publishing
a release. Set `SILO_PLUGINS_DISPATCH_TOKEN` in the plugin repository so it can
call `repository_dispatch` on `Silo-Server/silo-plugins`.

`silo-plugins` uses `CATALOG_PUSH_TOKEN` to push catalog updates. Reading a
plugin's release metadata and its tagged `manifest.json` needs no extra
credential: every catalogued plugin repository is public, so the workflow's own
`github.token` is enough. A plugin has to be public before it dispatches.

To exercise ingestion locally against an existing tagged release, pass the
repository in `owner/name` form and the exact release tag:

```sh
go run ./cmd/update-catalog \
  -repo Silo-Server/silo-plugin-metadata-tmdb \
  -tag v1.2.23
```

For a private source repository, provide `GITHUB_TOKEN` through your normal
secret-injection workflow. The command rewrites `manifest.json`; review or
discard that diff after the check.

## Development

```sh
go test ./...
go vet ./...
go build ./...
```

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request. Plugin
implementation changes belong in the source plugin repository; catalog schema
and automation changes belong here.

## License

`silo-plugins` is licensed under `Apache-2.0`. See [LICENSE](LICENSE).
