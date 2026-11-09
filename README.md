# Spark Plugin Catalog

The `spark` branch is the plugin catalog read by Spark Server. It includes
plugins from Silo's maintained catalog and plugins developed for Spark; plugin
IDs and release ownership remain with their source repositories.

`main` mirrors the upstream [Silo Plugin Catalog](https://github.com/Silo-Server/silo-plugins)
unchanged. Catalog updates from upstream are merged into `spark` by the sync
workflow.

## Spark plugins

- `spark.autoscan.arr` — [Sonarr & Radarr Autoscan](https://github.com/Joloxx9/spark-plugin-autoscan-arr)
- `silo.watchprovider.scrob` — [Scrob](https://github.com/Joloxx9/silo-plugin-watchprovider-scrob-local), Spark's Scrob build

To add or update a Spark plugin, run `go run ./cmd/update-catalog -repo <owner/name> -tag <tag>`
on `spark` and commit the resulting `manifest.json` update.

## Development

Use the Go version declared in `go.mod`:

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

The catalog tooling is licensed under `Apache-2.0`. See [LICENSE](LICENSE).
