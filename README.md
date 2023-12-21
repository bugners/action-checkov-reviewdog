# GitHub Action: Run checkov with reviewdog

## Inputs

**Required**. Must be in form of `github_token: ${{ secrets.github_token }}`.

### `reporter`

Optional. Reporter of reviewdog command [`github-pr-check`,`github-pr-review`].
The default is `github-pr-check`.

### `filter_mode`

Optional. Filtering for the reviewdog command [`added`,`diff_context`,`file`,`nofilter`].

The default is `added`.

See [reviewdog documentation for filter mode](https://github.com/reviewdog/reviewdog/tree/master#filter-mode) for details.

### `fail_on_error`

Optional. Exit code for reviewdog when errors are found [`true`,`false`].

The default is `false`.

See [reviewdog documentation for exit codes](https://github.com/reviewdog/reviewdog/tree/master#exit-codes) for details.

### `working_directory`

Optional. Directory to run the action on, from the repo root.
The default is `.` ( root of the repository).

### `skip_check`

Optional. Specify comma separated strings of checks that should be ignored.

### `baseline`

Optional. Allows you to include a baseline file with known findings that should be ignored.

### `download_external_modules`

Optional. Indicates whether any external modules should be downloaded.
The default is `false`

## Example usage

```yml
name: checkov-reviewdog
on: [pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    permissions:
      checks: write
      contents: read
      pull-requests: write
    name: checkov-reviewdog-scan
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: checkov-reviewdog
        uses: bugners/action-checkov-reviewdog@main
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          reporter: github-pr-review # Optional. Change reporter
          fail_on_error: "true" # Optional. Fail action if errors are found
          filter_mode: "nofilter" # Optional. Check all files, not just the diff
          working_directory: "."
          skip_check: "CKV_GCP_13" # Optional. Skip specific checks
          baseline: ".checkov.baseline" #Do not report results for checks in the baseline file
          download_external_modules: false  # Optional. Try downloading any external modules
```
