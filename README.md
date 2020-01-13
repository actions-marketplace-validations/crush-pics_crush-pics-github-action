# Crush.pics for GitHub

Crush.pics for GitHub automatically compress JPEG, PNG and GIF images in GitHub Pull Requests.

- Uses the best image compression algorithms available
- Runs in [GitHub Actions](https://github.com/features/actions)

## Add Crush.pics for GitHub to your repository

1. Open or create the `.github/workflows/crush-pics.yml` file.
2. Paste in the following:

```yml
name: Crush images
on: pull_request
jobs:
  crush:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v1
      - name: Crush images
        uses: ./
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
          api-key: ${{ secrets.API_KEY }}
```

The `GITHUB_TOKEN` secret is [automatically generated by GitHub](https://help.github.com/en/articles/virtual-environments-for-github-actions#github_token-secret). This automatic token is [scoped only to the repository that is currently running the action.](https://help.github.com/en/articles/virtual-environments-for-github-actions#token-permissions)
The `API_KEY` is your Crush.pics API key. Get one by creating a free account on [Crush.pics Web app](https://app.crush.pics)

## Configure Crush.pics for GitHub

By default Crush.pics for GitHub will use our best available Balanced compression. However, if you’d like to ignore specific file paths, or change image compression options, read on.

Change the configuration options by adding a `.github/crush-pics/config.yml` file:

```yml
compression_mode: balanced # [lossy | lossless | balanced]
compression_level: 85 # [65-100]
strip_tags: false
```

- `compression_level ` is valid for `lossy` mode only. `balanced` & `lossless` will ignore this setting

## Running the action only when images are changed

Crush.pics for GitHub is designed to run for each Pull Request. In some repositories, images are seldom updated. To run the action only when images have changed, use GitHub Action’s [`on.push.paths`](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#onpushpull_requestpaths) workflow configuration:

```yml
name: Crush images
on:
  pull_request:
    paths:
      - '**.jpg'
      - '**.jpeg'
      - '**.png'
      - '**.gif'
```

The above workflow will only run when `jpg`, `png` or `gif` files are changed.