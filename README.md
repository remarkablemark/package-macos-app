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
          binary: ./my-app
          version: 1.2.3

      - name: Upload app bundle
        uses: actions/upload-artifact@v7
        with:
          name: my-app
          path: ${{ steps.package.outputs.app-path }}
```

## Usage

**Basic:**

```yaml
- uses: remarkablemark/package-macos-app@v1
  with:
    binary: ./my-app
    version: 1.2.3
```

**Custom name, bundle identifier, and icon:**

```yaml
- uses: remarkablemark/package-macos-app@v1
  with:
    binary: ./my-app
    name: My App
    bundle-id: com.example.my-app
    version: 1.2.3
    icon: ./assets/AppIcon.icns
    output: dist
```

The action creates the following bundle structure:

```
My App.app/
└── Contents
    ├── Info.plist
    ├── MacOS
    │   └── My App
    └── Resources
        └── AppIcon.icns
```

The bundle is **unsigned** — sign it with `codesign` before distribution if needed.

See [action.yml](action.yml)

## Inputs

### `binary`

**Required**: Path to the prebuilt macOS binary.

### `version`

**Required**: App version (`CFBundleShortVersionString` and `CFBundleVersion`). For tag-triggered builds, pass the tag (e.g., `1.2.3`):

```yaml
- uses: remarkablemark/package-macos-app@v1
  with:
    binary: ./my-app
    version: ${{ github.ref_name }}
```

### `name`

**Optional**: App name. Defaults to the binary filename without extension.

### `bundle-id`

**Optional**: Bundle identifier (`CFBundleIdentifier`). Defaults to `com.example.<app-name>`.

### `icon`

**Optional**: Path to an `.icns` icon file, copied to `Contents/Resources`.

### `output`

**Optional**: Directory where the `.app` bundle is created. Defaults to `.`.

## Outputs

### `app-path`

Path to the created `.app` bundle.

## License

[MIT](LICENSE)
