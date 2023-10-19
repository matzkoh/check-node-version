[![Test](https://github.com/matzkoh/check-node-version/actions/workflows/default.yml/badge.svg)](https://github.com/matzkoh/check-node-version/actions/workflows/default.yml)

# matzkoh/check-node-version

This GitHub Action checks if commands like `nodenv install` will definitely succeed.

It checks the `node-build` repository, which is the source of the installation, and fails if the target version does not exist.

## Examples

### Basic

```yaml
steps:
  - uses: actions/checkout@v4
    with:
      sparse-checkout: /.node-version
      sparse-checkout-cone-mode: false

  - uses: matzkoh/check-node-version@v1
```

### Optional

```yaml
steps:
  - uses: actions/checkout@v4
    with:
      sparse-checkout: /.node-version
      sparse-checkout-cone-mode: false

  - id: check-node-version
    continue-on-error: true # continue even if the node version is not yet released
    uses: matzkoh/check-node-version@v1
    with:
      node-version-path: .nvmrc # use a file other than `.node-version` if desired

  - run: echo ${{ steps.check-node-version.outcome == 'success' && 'released 😀' || 'not yet released 😢' }}
```

## Inputs

See [action.yml](action.yml)

| Name                | Description                                                         | Default         | Required |
| ------------------- | ------------------------------------------------------------------- | --------------- | -------- |
| `node-version-path` | Path to the file containing the node version to check               | `.node-version` | ❌       |
| `node-version`      | Node version to check. If not specified, it will be read from file. |                 | ❌       |

## Outputs

See [action.yml](action.yml)

| Name                 | Description                                                          |
| -------------------- | -------------------------------------------------------------------- |
| `node-version`       | Same as `node-version` input. Checked version.                       |
| `node-build-version` | Latest release version of brew `node-build` formula used for checks. |
