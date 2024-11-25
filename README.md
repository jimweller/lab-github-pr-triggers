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
