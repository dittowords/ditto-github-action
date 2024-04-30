# ditto-github-action

The Ditto github action creates a PR with the most recent Ditto text updates.

## Prerequisites

**To include the Ditto GitHub action as a part of your repo's workflow, the repo must have integration with the [Ditto CLI](https://github.com/dittowords/cli).** If your repository does not have a `ditto` directory with a filled-out `config.yml` **as a result of initializing the Ditto CLI** this action will not work.

## Inputs

| Name            | Required | Description                                                                                                                                                  |
| --------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ditto-api-key   | yes      | [Generated Ditto API key](https://developer.dittowords.com/api-reference#creating-an-api-key)                                                                |
| ditto-dir       | no       | `ditto` directory location. Only required if the `ditto` directory is not located at the root of your repository. Must include 'ditto' (e.g. `./src/ditto`). |
| pr-title-prefix | no       | String to prefix the pull request title with (e.g. "Web App Ditto Updates" would result in a PR titled: `Web App Ditto Updates YYYY-MM-DDT`)                 |

## Example Workflow

```
name: Update Ditto Text
on: workflow_dispatch
jobs:
  UpdateDittoText:
    runs-on: ubuntu-latest
    steps:
      - name: Pull Ditto text and create a PR
        uses: dittowords/ditto-github-action@v0.2.0
        with:
          ditto-api-key: ${{ secrets.DITTO_API_KEY }}
```

### With Multiple Ditto Config files

```
name: Update Ditto Text
on: workflow_dispatch
jobs:
  UpdateWebDittoText:
    runs-on: ubuntu-latest
    steps:
      - name: Pull Ditto text for Web app and create a PR
        uses: dittowords/ditto-github-action@v0.2.0
        with:
          ditto-api-key: ${{ secrets.DITTO_API_KEY }}
          ditto-dir: "./web/ditto"
          pr-title-prefix: "Web Ditto text update"

      - name: Pull Ditto text for iOS app and create a PR
        uses: dittowords/ditto-github-action@v0.2.0
        with:
          ditto-api-key: ${{ secrets.DITTO_API_KEY }}
          ditto-dir: "./ios/ditto"
          pr-title-prefix: "iOS App Ditto text update"
```

This example workflow allows you to [manually start the workflow](https://docs.github.com/en/actions/managing-workflow-runs/manually-running-a-workflow) resulting in the creation of a PR.

It is recommended to pass the `ditto-api-key` via a [GitHub secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) to prevent your API key from getting exposed to the public.
