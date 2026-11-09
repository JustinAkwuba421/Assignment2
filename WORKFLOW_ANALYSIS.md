# GitHub Actions Workflow Analysis from google docs 

Analysis of `.github/workflows/deploy.yml`.

## 1. What triggers this workflow to run?

The workflow runs in two situations:

- When new code is pushed to the `main` branch.
- When someone opens or updates a pull request to the `main` branch.

## 2. What are the four main steps this workflow performs?

The workflow has four main steps:

1. Checks out the code.
2. Checks the HTML for errors.
3. Checks if the links work.
4. Uploads the finished files.

After that, another job deploys the files to GitHub Pages if everything passes.

## 3. What does the "Checkout code" step do and why is it necessary?

This step downloads the project's code so GitHub Actions can work with it. It is needed because the runner starts fresh and doesn't already have the project files.

## 4. What is the purpose of the environment configuration?

The environment tells GitHub that the project is being deployed to GitHub Pages. It also keeps track of the deployment and provides a link to the live website once it's published.

## 5. How does this automated deployment improve reliability compared to manual deployment?

Automated deployment makes things easier and helps prevent mistakes. It checks the code before publishing, so problems can be caught early. It also makes sure the same steps happen every time instead of relying on someone to do everything manually.

One thing to remember is that link-checking errors won't stop the deployment because that step is allowed to continue even if it finds a problem.

## 6. What would happen if you pushed code to a different branch (not main)?

Nothing would happen right away because the workflow only runs when code is pushed to the `main` branch.

However, if you open a pull request to merge that code into `main`, GitHub will check it for errors. The website won't be updated until the changes are merged into `main`.
