# Label Pull Requests with Merge Conflicts Automatically

## Setup

> Label

Create a label called `conflict` on the repository - you can pick any color for it

> Github access token

Set up a personal access token (just in case this is not already created). 

Note that you need to have the correct admin permissions for the repository so that the token you create (which is associated with your account) can back-merge without requiring PR approval. You can give this token all permissions except for Delete Repository. See the GitHub docs here to create a personal access token: [Creating a personal access token - GitHub Docs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

> Secrets

Add your github token as a Secret called `ACTIONS_SECRET`

Add a [Slack Income Webhook token](https://api.slack.com/messaging/webhooks) as a Secret called `SLACK_WEBHOOK_URL`

> Workflow file

Make sure to create a conflicts.yml config file under `.github/workflows` with the following structure

```
name: Auto Label Conflicts
on: [push, pull_request]

jobs:
  auto-label:
    runs-on: ubuntu-latest
    steps:
      - uses: yieldstreet/conflicts-github-action@main
        with:
          conflict_label_name: 'conflict'
          github_token: ${{ secrets.GITHUB_TOKEN }}
          max_retries: 5
          wait_ms: 15000
          detect_merge_changes: false
          slack_webhook_url: ${{ secrets.SLACK_WEBHOOK_URL }}
          slack_webhook_channel: '#release-management'
```

You can customize the configuration fields as needed.
