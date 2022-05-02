# GitHub Push Subdirectories

GitHub Action to publish subdirectories to separate GitHub repositories.

## Why?

When building Gatsby Themes with a monorepo it's common to need to
be able to develop your corresponding starters in the same repo as
well. This allows you to automatically push your starters to their
own repo so they can be used with `gatsby new`.

## Usage

```yml
name: Publish Starters
on: push
jobs:
  master:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: publish:starters
        uses: design4pro/actions-github-push-subdirectories@master
        env:
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          args: starters design4pro
```

The `GITHUB_TOKEN` will automatically be defined, the `GH_TOKEN` needs to be set in the `Secrets` section of your repository options. You can retrieve the `GH_TOKEN` [here](https://github.com/settings/tokens) (set the `repo` permission).

The action accepts three arguments - the first two are mandatory, the third (key from `package.json`), four (GitHub email) and five branch name, are optional.

1. Name of the folder that contains your examples. Even if you only have one example currently it also should be placed inside its own folder (e.g. `starters/foo-bar`) as the script will read all folders inside the examples.
2. GitHub username
3. Repository name of the respective example. By default the `name` key from the example's `package.json` is used, e.g. the `name` of your example is `gatsby-starter-foobar`, then the script will try to push to `github.com/USERNAME/gatsby-starter-foobar`.
4. Branch name (optional) - `main` default
5. GitHub email (optional)

### Custom starter names

You could define the key `starter-name` in your example's `package.json`, like:

```json
{
  "starter-name": "gatsby-starter-foobar",
}
```

Use the action with the third argument now:

```yml
args: starters design4pro starter-name master
```
