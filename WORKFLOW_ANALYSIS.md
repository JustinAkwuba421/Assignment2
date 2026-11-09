# GitHub Actions Workflow Analysis

Analysis of `.github/workflows/deploy.yml`.

## 1. What triggers this workflow to run?

The `on:` section defines two triggers:

- **`push` to `main`**: runs whenever commits land on `main`, including when a pull request is merged.
- **`pull_request` targeting `main`**: runs when a pull request into `main` is opened, reopened, or gets new commits.

## 2. What are the four main steps this workflow performs?

The `build-and-test` job has four steps:

1. **Checkout code**
2. **Validate HTML**
3. **Check links**
4. **Upload artifact**

A separate `deploy` job then runs one more step, **Deploy to GitHub Pages**, which publishes the uploaded artifact. That job only starts if `build-and-test` succeeds.

## 3. What does the "Checkout code" step do and why is it necessary?

It uses `actions/checkout@v4` to download the repository, at the commit that triggered the workflow, onto the GitHub Actions runner.

It is necessary because each job starts on a fresh virtual machine that does not contain the project's files. Without this step, the validation, link check, and upload steps would have no code to work on.

## 4. What is the purpose of the environment configuration?

The `environment` block ties the `deploy` job to the `github-pages` deployment environment. `name: github-pages` tells GitHub which environment is being deployed to, and `url: ${{ steps.deployment.outputs.page_url }}` records the live site's address once the deployment finishes.

This lets GitHub track deployment history, show a link to the live site, and apply any rules set on that environment, such as which branches may deploy to it.

## 5. How does this automated deployment improve reliability compared to manual deployment?

- **Checks run every time.** HTML validation runs before each deployment, so nobody can forget it.
- **Failures stop the release.** `needs: build-and-test` means broken HTML never reaches the live site.
- **The process is repeatable.** Every deployment follows the same steps on a clean machine, which removes "it worked on my computer" problems and manual copy mistakes.
- **Only reviewed code goes live.** Deployment happens only from `main`, which is protected by pull requests.
- **There is a record.** The Actions tab shows what was deployed, when, by which commit, and whether it passed.

One limitation: the **Check links** step has `continue-on-error: true`, so a problem in that step is reported but does not block deployment.

## 6. What would happen if you pushed code to a different branch (not main)?

The push alone would not start the workflow, because `on.push.branches` only lists `main`, and nothing would be deployed.

If that branch then had a pull request opened against `main`, the `pull_request` trigger would run `build-and-test` so the changes could be checked before merging. The `deploy` job would still be skipped, because its condition `github.event_name == 'push' && github.ref == 'refs/heads/main'` is false. The site only updates after the code reaches `main`.
