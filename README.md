# package-macos-app

[![GitHub Release](https://img.shields.io/github/v/release/remarkablemark/package-macos-app)](https://github.com/remarkablemark/package-macos-app/releases)
[![test](https://github.com/remarkablemark/package-macos-app/actions/workflows/test.yml/badge.svg)](https://github.com/remarkablemark/package-macos-app/actions/workflows/test.yml)
[![lint](https://github.com/remarkablemark/package-macos-app/actions/workflows/lint.yml/badge.svg)](https://github.com/remarkablemark/package-macos-app/actions/workflows/lint.yml)

📦 Packages a prebuilt binary into an unsigned macOS app bundle with GitHub Actions.

## Quick Start

```yaml
on: push
jobs:
  build:
    runs-on: macos-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v7

      - name: Package macOS app
        id: package
        uses: remarkablemark/package-macos-app@v1
        with:
          binary: ./my-cli

      - name: Upload app bundle
        uses: actions/upload-artifact@v4
        with:
          name: my-cli
          path: ${{ steps.package.outputs.app-path }}
```

## Usage

**Basic:**

```yaml
- uses: remarkablemark/package-macos-app@v1
  with:
    binary: ./my-cli
```

**Custom name, bundle identifier, and icon:**

```yaml
- uses: remarkablemark/package-macos-app@v1
  with:
    binary: ./my-cli
    name: My CLI
    bundle-id: io.example.my-cli
    version: 1.2.3
    icon: ./assets/AppIcon.icns
    output: dist
```

The action creates the following bundle structure:

```
My CLI.app/
└── Contents
    ├── Info.plist
    ├── MacOS
    │   └── My CLI
    └── Resources
        └── AppIcon.icns
```

The bundle is **unsigned** — sign it with `codesign` before distribution if needed.

See [action.yml](action.yml)

## Inputs

### `binary`

**Required**: path to the prebuilt macOS binary.

### `name`

**Optional**: the app name. Defaults to the binary filename without extension.

### `bundle-id`

**Optional**: the bundle identifier (`CFBundleIdentifier`). Defaults to `com.example.<app-name>`.

### `version`

**Optional**: the version (`CFBundleShortVersionString` and `CFBundleVersion`). Defaults to `1.2.3`:

```yaml
- uses: remarkablemark/package-macos-app@v1
  with:
    binary: ./my-cli
    version: 2.3.4
```

### `icon`

**Optional**: path to an `.icns` icon file, copied to `Contents/Resources`.

### `output`

**Optional**: the directory where the `.app` bundle is created. Defaults to `.`.

## Outputs

### `app-path`

Path to the created `.app` bundle.

## License

[MIT](LICENSE)
