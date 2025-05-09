# actions-golangci-lint

A reusable GitHub Action for running [golangci-lint](https://golangci-lint.run/) on Go projects. This action helps maintain code quality by running a comprehensive set of linters on your Go codebase.

## Features

- Runs golangci-lint on your Go codebase
- Configurable Go version and golangci-lint version
- Customizable working directory and linting arguments
- Easy to integrate into your workflow
- Provides detailed feedback on code quality issues
- Uses golangci-lint-action v8.0.0 internally
- Includes a default configuration with common best practices
- Supports custom configuration via `.golangci.yml` file
- Enables a comprehensive set of modern and recommended linters by default, including `prealloc`, `nestif`, `nolintlint`, `exportloopref`
- Disables deprecated or obsolete linters automatically (`golint`, `maligned`, `nakedret`)
- Analyzes test files by default

## Usage

### Basic Usage

```yaml
name: golangci-lint

on:
  pull_request: {}
  push:
    branches:
      - main
      - master

permissions:
  contents: read
  pull-requests: write

jobs:
  golangci-lint:
    uses: cloudbuildlab/actions-golangci-lint/.github/workflows/golangci-lint.yml@v0
```

### Advanced Usage

```yaml
name: golangci-lint

on:
  pull_request: {}
  push:
    branches:
      - main

permissions:
  contents: read
  pull-requests: write

jobs:
  golangci-lint:
    uses: cloudbuildlab/actions-golangci-lint/.github/workflows/golangci-lint.yml@v0
    with:
      go-version: '1.21'
      golangci-lint-version: 'v1.55.2'
      working-directory: './cmd'
      args: '--timeout=5m'
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `go-version` | Go version to use | `stable` |
| `golangci-lint-version` | golangci-lint version to use | `latest` |
| `working-directory` | Working directory to run linter in | `.` |
| `config-file` | Path to golangci-lint config file | `.golangci.yml` |
| `args` | Additional arguments to pass to golangci-lint | `` |

### Configuration File

The action supports configuration through a `.golangci.yml` file. By default, it looks for this file in your repository root. You can specify a different location using the `config-file` input.

Example `.golangci.yml`:

```yaml
run:
  timeout: 5m
  tests: true

linters:
  enable:
    - gofmt
    - govet
    - errcheck
    - staticcheck
    - gosimple
    - ineffassign
    - unused
    - misspell
    - gosec
    - unconvert
    - goconst
    - gocyclo
    - dupl
    - gocritic
    - revive
    - prealloc
    - nestif
    - nolintlint
    - exportloopref
  disable:
    - golint
    - maligned
    - nakedret
  settings:
    gocyclo:
      min-complexity: 15
    dupl:
      threshold: 100
    goconst:
      min-len: 2
      min-occurrences: 3
    misspell:
      locale: US
    revive:
      rules:
        - name: exported
          arguments:
            - disableStutteringCheck

issues:
  max-issues-per-linter: 50
  max-same-issues: 3
  new-from-rev: HEAD~1
```

### Default Configuration

The action includes a default configuration that will be used if no `.golangci.yml` file exists in your repository. The default configuration:

- Enables common and useful linters:
  - `gofmt`, `govet`, `errcheck`, `staticcheck`, `gosimple`, `ineffassign`, `unused`, `misspell`, `gosec`, `unconvert`, `goconst`, `gocyclo`, `dupl`, `gocritic`, `revive`, `prealloc`, `nestif`, `nolintlint`, `exportloopref`
- Disables deprecated or obsolete linters:
  - `golint`, `maligned`, `nakedret`
- Sets linter settings:
  - Cyclomatic complexity threshold: 15
  - Duplicate code threshold: 100
  - Constant string length: 2
  - US English spell checking
- Sets a 5-minute timeout and enables test file analysis
- Limits issues per linter and per same issue
- Only shows new issues since the last commit (`HEAD~1`) using `issues.new-from-rev: HEAD~1`

You can override this configuration by:

1. Creating your own `.golangci.yml` file in your repository
2. Using the `config-file` input to specify a different configuration file
3. Using the `args` input to pass additional arguments to golangci-lint

## Versioning

This workflow follows semantic versioning. You can use it in two ways:

1. **Major Version Tag** (Recommended):

   ```yaml
   uses: cloudbuildlab/actions-golangci-lint/.github/workflows/golangci-lint.yml@v0
   ```

   This will automatically use the latest release within the v0.x.x series.

2. **Specific Version**:

   ```yaml
   uses: cloudbuildlab/actions-golangci-lint/.github/workflows/golangci-lint.yml@v0.1.0
   ```

   This pins to a specific version for maximum stability.

### Version History

See the [Releases](../../releases) page for a full list of versions and changes.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
