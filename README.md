# ArgoCD ApplicationSet Setup Guide

This guide explains the steps to set up an ArgoCD ApplicationSet for managing your team's applications across different
environments.

## Prerequisites

- ArgoCD is installed and configured in your Kubernetes cluster.
- You have access to the Git repository where the configuration files are stored.
- `kubectl` is installed and configured to interact with your Kubernetes cluster.

---

## Steps

### 1. Create the Project File

Each team should have a project file to define the scope of their applications. Create a project file for your team, one
for production and one for non-production environments.

Example path: `argocd-config/projects/ms-team-project-nonprd.yaml`

Projects are created automatically using the `app-of-projects.yaml` file, which uses the app-of-apps pattern to create
new projects.
It syncs automatically with github and creates the project in ArgoCD.

---

### 2. Create the `app-values.yaml` File

Define the details of your applications for each environment in the `app-values.yaml` file.
This file contains the configuration for all applications managed by your team.
There should be one file for non-production and one file for prod

Example path: `teams/ms-team/app-values-nonprod.yaml`

---

### 3. Create the ApplicationSet

Define the `ApplicationSet` to dynamically generate applications based on the `app-values.yaml` file.

Example path: `argocd-config/application-sets/ms-team-applicationset-nonprod.yaml`

---

### 4. Apply the Configuration

Use `kubectl` to apply the configuration files to your cluster.

```bash
# Apply the ApplicationSet
kubectl apply -f argocd-config/application-sets/ms-team-applicationset.yaml
```

## Branching Strategy and CODEOWNERS file
The `CODEOWNERS` file is used to define the owners of the files in the repository.
This is important for managing
permissions and ensuring that the right people are notified of changes.

### Branching Strategy

- **`main` Branch**: This branch is used for production-ready code. Only merge changes to this branch after thorough testing.
- **Feature Branches**: For any new feature, bug fix, or enhancement, create a new branch from `main`. Use a descriptive name for the branch, such as:
  - `feature/<short-description>` for new features or enhancements.
  - `bugfix/<short-description>` for bug fixes.

### Pull Requests

- All changes must be submitted via a pull request (PR).
- Each PR must:
  - Be reviewed and approved by at least one of the `CODEOWNERS`.
  - Pass all automated tests and checks before merging.

### Commit Strategy

Use clear and concise commit messages following the format:
- `feat: <short description>` for new features or enhancements.
- `fix: <short description>` for bug fixes.