# github-actions-tutorial
A tutorial on GitHub Actions for SW Developers, based on the [demo workflow ](https://docs.github.com/en/actions/writing-workflows/quickstart) from GitHub.

## How to check the status of your workflows
- Under the "Actions" tab of the repo
- VSCode extension: GitHub Actions

## Triggers
A lot of [things](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#release) can trigger a workflow, the most common in our SW/FW repos are:
- push to branch
- PR opened, reopened, synched or ready for review
- Release published

In CI/CD, we also use:
- scheduled
- workflow calls
- workflow dispatch (= manual trigger)

[c48c57a](https://github.com/wingtra/github-actions-tutorial/commit/c48c57ac26e97c891a11d221507b399a896776db): Adding a manual trigger to our workflow and a branch filter for the push trigger.

## Runners and containers
A runner is the infrastructure on which the action runs, it can be GitHub hosted (instant set-up, no maintenance) or self-hosted (no costs for running actions).

GitHub runners have several basic host [images](https://github.com/actions/runner-images) for all usual operating systems (Windows, MacOS and Ubuntu). On top of the host, there is support to run a container for a more tailored environment.

[e50707a](https://github.com/wingtra/github-actions-tutorial/commit/e50707a7749bb4bf5b6e4b8066f87ccd1d4792e0): Changing host to run a newest default python version.
[281d324](https://github.com/wingtra/github-actions-tutorial/commit/281d324f80ec4dd8873681e9a5d039492fd064cb): Adding a container to run an older python version, not available on the host.

## Steps
A step is the basic unit of the workflow, if a step fails, by default, all following steps will be skipped and the workflow will fail.
The flow of execution can be controlled with:
- `if: <condition>` to make the step conditional.
- `continue-on-error: true` to keep the workflow running even if the step fails. Note that the step execution will stop at the failure point.

In [bb32051](https://github.com/wingtra/github-actions-tutorial/commit/bb32051930680ee3fc6c315808469fe226a7dda5) a failure has been added on purpose.
[0eb91b8](https://github.com/wingtra/github-actions-tutorial/commit/0eb91b85c03f9bde7f406807fc897b71d215c3b6): allows it by setting `continue-on-error`.
[6038efe](https://github.com/wingtra/github-actions-tutorial/commit/6038efe19b9eba06e42214f136807aa4c9fbe17f): skips the step with a conditional `if`.

Steps can be reused, by making use of composite actions, which packs multiple steps in one (with inputs possible), they can be defined locally, in each repo, or picked from the GitHub [marketplace](https://github.com/marketplace?type=actions).

Steps can also be reused with `matrix`, each element will trigger a full workflow in which the step execution can be adapted with `include` options.

[ed997ae](https://github.com/wingtra/github-actions-tutorial/commit/ed997ae79d0a25b6ad80af45c26c6910cd1066e4): shows how to run the workflow for multiple goodbye strings wth matrix. Note the use of `uses: actions/checkout@v4`, which is a composite action for the marketplace.

## Jobs hierarchy
Complete workflow can also be reused if they use the `workflow_call` trigger. Furthermore, jobs can be mode dependant on each other by using the keyword `needs`. If job A needs job C and job B needs job C, the workflow will execute first C, then A and B in parallel (if enough runners are available) or sequentially in an arbitrary order.

[3fb9c08](https://github.com/wingtra/github-actions-tutorial/commit/3fb9c08f379316dcbe8146edd793b22a805fcfe4): Show the usage of `needs` by adding a second job.