# Publication metadata

The [manuscript configuration](manuscript/config.yaml) records the GitHub
repository, concept and version DOIs, and declared publication destinations.
The [example configuration](manuscript/config.yaml.example) provides placeholders
for forks. Citation sidecars remain in the project root.

From the [pinned Template checkout](quickstart.md#render-the-descriptive-manuscript),
regenerate the root README status block with the explicit configuration path:

```sh
uv run python -m infrastructure.publishing.status_report \
  --project "$project" --config "$project/docs/manuscript/config.yaml" --write
```

The generated footer at the pinned revision uses the legacy config path. Use the
explicit `--config` argument above when regenerating from this checkout.

The tracked [output snapshot](../output/) accompanies the published version.
Build new output in a disposable checkout when verifying changes. Update the
snapshot only when preparing a release.
