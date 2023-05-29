# Astro Check

A composite action that checks your Astro site before deployment (e.g as your CI workflow). Requires a check command defined in your `package.json`.

```json
// in package.json
"scripts": {
  "check": "astro check"
  // rest of your npm scripts
}
```

## Usage

### Inputs

#### Required

- `check-command` - Your supplied check command, defined in `package.json` Defaults to `check`.


#### Optional

- `path` - The root location of your Astro project inside the repository.
- `node-version` - The specific version of Node that should be used to build your site. Defaults to `16`.
- `package-manager` - The Node package manager that should be used to install dependencies and build your site. Automatically detected based on your lockfile.
- `resolve-dep-from-path` - If the dependency file should be resolved from the root location of your Astro project. Defaults to `true`.

### Example workflow:

#### Build and Deploy to GitHub Pages

Create a file at `.github/workflows/deploy.yml` with the following content.

```yml
name: Check my Astro site

on:
  # Trigger the workflow every time you created a pull request against the `main` branch
  # Using a different branch name? Replace `main` with your branch’s name
  pull_request:
    branches: [ main ]
  
# Allow this job to clone the repo and create a deployment
permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout your repository using git
        uses: actions/checkout@v3
      - name: Install, build, and upload your site output
        uses: radenpioneer/astro-check@main
        with:
          check-command: "check" # Needs to be defined in your `package.json`.
          # path: . # The root location of your Astro project inside the repository. (optional)
          # node-version: 16 # The specific version of Node that should be used to build your site. Defaults to 16. (optional)
          # package-manager: yarn # The Node package manager that should be used to install dependencies and build your site. Automatically detected based on your lockfile. (optional)
          # resolve-dep-from-path: false # If the dependency file should be resolved from the root location of your Astro project. Defaults to `true`. (optional)

```

## Attribution

- This Github Action is imported from Astro's official [`withastro/actions`](https://github.com/withastro/action), and modified for checking.
