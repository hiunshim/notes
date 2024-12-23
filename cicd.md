

Minimalistic github action that formats all go code
```
on:
  pull_request:
    branches: [main]
jobs:
  format:
    name: Enforce Code Format
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v4
      - name: Set up Bazel
        uses: bazel-contrib/setup-bazel@0.9.0
        with:
          bazelisk-cache: true
          disk-cache: ${{ github.workflow }}
          repository-cache: true
      - name: Check code format
        run: |
          bazelisk run @go_sdk//:bin/gofmt -- -l . > gofmt_output.txt || true
          if [ -s gofmt_output.txt ]; then
            echo "Following files are not properly formatted:"
            cat gofmt_output.txt
            echo "Please run: bazelisk run @go_sdk//:bin/gofmt -- -w ."
            exit 1
          else
            echo "All files are properly formatted!"
          fi

```

Github action that compares only the changed files to see if they've been formatted
```
name: lint

on:
  pull_request:
    branches: [main]

jobs:
  format:
	name: Enforce Code Format
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v4
      - name: Set up Bazel
        uses: bazel-contrib/setup-bazel@0.9.0
        with:
          bazelisk-cache: true
          disk-cache: ${{ github.workflow }}
          repository-cache: true
          
      - name: Fetch base and head refs (useful for fork-based PRs)
        run: |
          git fetch --no-tags origin ${{ github.event.pull_request.base.ref }}
          git fetch --no-tags origin ${{ github.event.pull_request.head.ref }}

      - name: Get changed Go files
        id: changed-files
        run: |
          echo "Base SHA: ${{ github.event.pull_request.base.sha }}"
          echo "Head SHA: ${{ github.event.pull_request.head.sha }}"

          FILES=$(git diff --name-only --diff-filter=ACMRT \
            ${{ github.event.pull_request.base.sha }}...${{ github.event.pull_request.head.sha }} \
            | grep '\.go$' || true)

          if [ -z "$FILES" ]; then
            echo "No changed .go files."
          else
            echo "Changed .go files:"
            echo "$FILES"
          fi

          echo "files=$FILES" >> $GITHUB_OUTPUT

      - name: Check code format
        if: steps.changed-files.outputs.files != ''
        run: |
          echo "Checking format for:"
          echo "${{ steps.changed-files.outputs.files }}"

          bazelisk run @go_sdk//:bin/gofmt -- -l ${{ steps.changed-files.outputs.files }} > gofmt_output.txt || true

          if [ -s gofmt_output.txt ]; then
            echo "Files not properly formatted:"
            cat gofmt_output.txt
            echo "Please run: bazelisk run @go_sdk//:bin/gofmt -- -w ."
            exit 1
          fi

          echo "All changed .go files are properly formatted."

      - name: No Go files changed
        if: steps.changed-files.outputs.files == ''
        run: echo "Skipping gofmt—no changed .go files found."

```