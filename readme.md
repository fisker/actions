# Common workflows I use

About [reuseable workflows](https://docs.github.com/en/actions/sharing-automations/reusing-workflows).

## autofix.yml

```yml
name: autofix.ci # needed to securely identify the workflow

on:
  pull_request:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  fix:
    if: github.repository == 'fisker/update'
    name: Run automated fix
    uses: fisker/shared-workflows/.github/workflows/autofix.yml@main
```

## scheduled-task.yml

```yml
name: Update

on:
  workflow_dispatch:
  schedule:
    # “At 00:00 on day-of-month 1 and on Sunday in January.” https://crontab.guru/#0_0_1_1_0
    - cron: "0 0 1 1 0"

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  update:
    if: github.event_name != 'schedule' || github.repository == 'fisker/update'
    permissions:
      contents: write
      pull-requests: write
    name: Run update
    uses: fisker/shared-workflows/.github/workflows/scheduled-task.yml@main
```

Make sure <kbd>Allow GitHub Actions to create and approve pull requests</kbd> option is checked in your repository settings (Under `Actions` -> `General`).
