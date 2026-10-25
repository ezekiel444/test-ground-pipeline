# test-ground-pipeline

A minimal GitHub Actions test fixture for conditional deployment jobs. The
deployment workflow is a simulation: it only prints the selected parameters and
does not connect to Azure or require deployment secrets.

## Conditions

| Job | Runs when |
| --- | --- |
| `deploy-dev` | The ref is `dev`, or a manual run selects `dev` |
| `deploy-preprod` | The ref is `preprod`, or a manual run selects `preprod` |
| `deploy-prod` | The event is a manual dispatch, the ref is `production`, and `prod` is selected |

Every push and pull request runs `ci`. Dev and preprod may also deploy
automatically on their matching branches, exactly as in the screenshot; a
manual run selecting either environment also runs that deployment. Prod is
manual-only and requires dispatching from the `production` branch. Deployments
are simulations and require `ci` to succeed.

## Trying the cases

Every branch push and pull request runs `ci`. On a feature-branch push or PR
into `main`, deployment jobs should be skipped. To test the screenshot's
branch conditions, push to `dev` and `preprod` and observe their simulated
deployments. Pull requests do not use the target branch name as their ref, so
they should run CI without matching those branch conditions.

To test the deployment conditions as well:

- Manually dispatch from any branch and select `dev` to run `deploy-dev`.
- Manually dispatch from any branch and select `preprod` to run `deploy-preprod`.
- Manually dispatch from `production` and select `prod` to run `deploy-prod`.
- Selecting `prod` from a branch other than `production` does not deploy.

Open **Actions → CI and conditional deployments → Run workflow**, choose the
branch, and select the required environment. The production condition is
intentionally stricter than dev/preprod: a `prod` selection is not enough
unless the dispatch ref is `production`.

Set `azure_function_to_deploy` to a function name to verify it is passed
through, or leave it as `all`.
