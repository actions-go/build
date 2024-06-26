# actions-go/build

A simple github action to build actions written in golang.

The best use of this action is to be embedded in a composite action.

```yaml
name: 'My go action'
description: My action is awesome
author: 'John Doe'
runs:
  using: "composite"
  steps:
    - id: build-action
      name: Build go action
      uses: actions-go/build@v1
      with:
        action_path: ${{ github.action_path }} # specifies the working directory for go build (usually the folder of your `go.mod` files).
        action: ${{ github.action }}
        go-version: "1.21" # defaults to 1.21
        go-main: '.' # defaults to '.'
    - name: Run my action
      shell: bash
      run: ${{ steps.build-action.outputs.install-path }}
```

The action is built in `${action_path}/${action}-(Darwin|Linux|Windows)-(x86_64|arm64)`.
If the build path already exists and is executable, the pre-built action will be used.
This means that you can create releases with a few committed files:
`action.yaml`, `${action}-(Darwin|linux)-(x86_64|arm64)` and `Readme.md`
and drop all your sources to avoid distributing your source code and re-building the action every time.
