# Github PR Triggers

A simple repository to show executing actions based on the state of a PR. Many
assets and actions need to exist for the duration of a PR. These triggers allow
for the creation and deletiong of those. Additionally, there is the ability to
filter on certain files or folders witht the `path:` attribute in the `on:`
clause.

## Usage

- Do a PR on a change to DONOTREADME.md and see nothing happen.
- Do a PR on README.md and watch things happen
  - Create the PR and watch github actions for a message
  - Add a new commit to the PR and see actions
  - Merge the PR and see actions
  - OR close the PR and see actions

## Ephemeral or Pre-Prod Branch Only Workflows

There's an interesting use case where you want to run a workflow that only
exists in a branch. This comes up for me all the time where either my
permissions are only in the workflow via OIDC (and not my terminal). Or the
workflow takes a really long time and I need fast feedback on a small portion.

What you can do is create the workflow in a branch as a bare stub. Set the
trigger to something gauranteed, like `push:`. Push once to the branch. Now the
workflow will show up in the UI and `gh workflow list`. So, now you have a
branch only workflow you can use for debugging.

For example:

Create a branch

`gco -b branch-only-workflow`

Make a workflow file in `.github/workflows/only-branch.yaml`

```yaml
name: Only Branch Workflow
on:
  workflow_dispatch:
  push:
jobs:
  new-pr:
    runs-on: ubuntu-latest 
    steps:
      - name: Do the Debug
        run: echo "Ran without ever being committed to main"
```

add, commit and push

```bash
git add --verbose . && git commit --message "branch only workflow" && git push origin "$(git_current_branch)"
```

Now the worfklow is visible in the UI and the command line. You can delete the `push:` if you want.

```bash
❯ gh workflow list
NAME                             STATE   ID       
Only Branch Workflow             active  130732267
```

Now, you can run it from the UI or the command line

```bash
> gh workflow run only-branch.yaml --ref branch-only-workflow
✓ Created workflow_dispatch event for only-branch.yaml at branch-only-workflow
```

- <https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows>
  - Look for events that DO NOT say `This event will only trigger a workflow run if the workflow file is on the default branch.`
- <https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/triggering-a-workflow>
  - `Some events also require the workflow file to be present on the default branch of the repository in order to run.`
- <https://github.com/orgs/community/discussions/25746>
- <https://stackoverflow.com/questions/63362126/github-actions-how-can-i-run-a-workflow-created-on-a-non-master-branch-from-t>
