# package-macos-app

[![GitHub Release](https://img.shields.io/github/v/release/remarkablemark/package-macos-app)](https://github.com/remarkablemark/package-macos-app/releases)
[![test](https://github.com/remarkablemark/package-macos-app/actions/workflows/test.yml/badge.svg)](https://github.com/remarkablemark/package-macos-app/actions/workflows/test.yml)
[![lint](https://github.com/remarkablemark/package-macos-app/actions/workflows/lint.yml/badge.svg)](https://github.com/remarkablemark/package-macos-app/actions/workflows/lint.yml)

📦 Packages a prebuilt binary into a macOS app bundle with GitHub Actions.

## Quick Start

```yaml
on: push
jobs:
  build:
    runs-on: macos-latest
    steps:
      - name: Package macOS app
        uses: remarkablemark/package-macos-app@v1
```

## Usage

**Basic:**

```yaml
- uses: remarkablemark/package-macos-app@v1
```

See [action.yml](action.yml)

## Inputs

### `version`

**Optional**: The version. Defaults to `1.2.3`:

```yaml
- uses: remarkablemark/package-macos-app@v1
  with:
    version: 1.2.3
```

## License

[MIT](LICENSE)
