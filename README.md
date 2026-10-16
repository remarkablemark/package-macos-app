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

      - name: Zip app bundle
        env:
          APP_PATH: ${{ steps.package.outputs.app-path }}
        run: ditto -c -k --keepParent "$APP_PATH" my-app.zip

      - name: Upload app bundle
        uses: actions/upload-artifact@v7
        with:
          name: my-app
          path: my-app.zip
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

**Required**: App version (`CFBundleShortVersionString` and `CFBundleVersion`). Must start with a digit (e.g., `1.2.3` or `1.2.3-beta.1`). For tag-triggered builds, pass the tag:

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

Path to the created `.app` bundle. The consuming step must reference the packaging step by its `id:` (see Quick Start).

Zip the bundle to preserve the executable bit — GitHub artifacts do not:

```yaml
- name: Zip app bundle
  env:
    APP_PATH: ${{ steps.package.outputs.app-path }}
  run: ditto -c -k --keepParent "$APP_PATH" My.App.zip
```

For job-to-job transfer on non-macOS runners, use `tar czf` instead of `ditto`.

## License

[MIT](LICENSE)
