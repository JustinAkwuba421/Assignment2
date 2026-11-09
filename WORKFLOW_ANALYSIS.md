# GitHub Actions Workflow Analysis (typed in doc)

Analysis of `.github/workflows/deploy.yml`.

## 1. What triggers this workflow to run?

The `on:` section has two triggers:

- **`push` to `main`**: The workflow runs whenever new code is pushed to the `main` branch. This also happens when a pull request is merged.
- **`pull_request` targeting `main`**: The workflow runs when someone opens a pull request, reopens it, or makes new changes to it.

These triggers help check the code before it gets added to the main project.

## 2. What are the four main steps this workflow performs?

The `build-and-test` job has four main steps:

1. **Checkout code** – Gets the project files so GitHub Actions can use them.
2. **Validate HTML** – Checks the HTML files for mistakes.
3. **Check links** – Looks for broken links in the project.
4. **Upload artifact** – Saves the website files so they can be used for deployment.

There is also a separate `deploy` job. This job has one step called **Deploy to GitHub Pages**, which puts the website online.

The deploy job only runs if the first job is successful.

## 3. What does the "Checkout code" step do and why is it necessary?

The Checkout code step uses `actions/checkout@v4` to download the project files onto the GitHub Actions runner.

This is important because GitHub Actions starts with a fresh computer that does not already have the project files.

Without this step, GitHub Actions would not be able to check the HTML, test the links, or upload the website files.

## 4. What is the purpose of the environment configuration?

The `environment` section tells GitHub that the project is being deployed to the `github-pages` environment.

- `name: github-pages` tells GitHub where the website is being deployed.
- `url: ${{ steps.deployment.outputs.page_url }}` saves the website link after the deployment is finished.

This makes it easier to find the live website and see when a deployment happens.

## 5. How does this automated deployment improve reliability compared to manual deployment?

Automated deployment makes things easier and helps prevent mistakes.

- **Automatic checks:** The workflow checks the HTML every time it runs.
- **Stops failed deployments:** If the build-and-test job fails, the website will not be deployed.
- **Same steps every time:** GitHub follows the same process each time, which helps avoid mistakes.
- **Better control:** Changes are deployed from the `main` branch instead of every branch.
- **Keeps a record:** GitHub shows when the workflow ran and if it was successful.

One thing to remember is that the **Check links** step has `continue-on-error: true`. This means that even if the link check fails, the workflow can still continue.

## 6. What would happen if you pushed code to a different branch (not main)?

If I pushed code to another branch besides `main`, the workflow would not run because it is only set to run when code is pushed to `main`.

However, if I created a pull request to merge that branch into `main`, the workflow would run the build-and-test job to check the code.

The deploy job would not run because the code has not been pushed to `main` yet.

This is helpful because developers can test their changes before putting them on the live website.
