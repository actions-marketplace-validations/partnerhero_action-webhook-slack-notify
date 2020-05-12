# Slack Notify - GitHub Action
[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public..](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)


A [GitHub Action](https://github.com/features/actions) to send a message to a Slack channel.

**Screenshot**

<img width="485" alt="action-slack-notify-rtcamp" src="https://user-images.githubusercontent.com/4115/54996943-9d38c700-4ff0-11e9-9d35-7e2c16ef0d62.png">

## Usage

You can use this action after any other action. Here is an example setup of this action:

1. Create a `.github/workflows/slack-notify.yml` file in your GitHub repo.
2. Add the following code to the `slack-notify.yml` file.

```yml
on: push
name: Slack Notification Demo
jobs:
  slackNotification:
    name: Slack Notification
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Slack Notification
      uses: rtCamp/action-slack-notify@v2.0.0
      env:
        SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
```

3. Create `SLACK_WEBHOOK` secret using [GitHub Action's Secret](https://developer.github.com/actions/creating-workflows/storing-secrets). You can [generate a Slack incoming webhook token from here](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks).


## Environment Variables

By default, action is designed to run with minimal configuration but you can alter Slack notification using following environment variables:

Variable          | Default                                               | Purpose
------------------|-------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------
SLACK_CHANNEL     | Set during Slack webhook creation                     | Specify Slack channel in which message needs to be sent
SLACK_USERNAME    | `DevOpsBot`                                           | The name of the sender of the message. Does not need to be a "real" username
SLACK_ICON        | ![rtBot Avatar](https://github.com/rtBot.png?size=32) | User/Bot icon shown with Slack message. It uses the URL supplied to this env variable to display the icon in slack message.
SLACK_ICON_EMOJI  | -                                                     | User/Bot icon shown with Slack message, in case you do not wish to add a URL for slack icon as above, you can set slack emoji in this env variable. Example value: `:bell:` or any other valid slack emoji.
SLACK_COLOR       | `good` (green)                                        | You can pass an RGB value like `#efefef` which would change color on left side vertical line of Slack message.
SLACK_MESSAGE     | Generated from git commit message.                    | The main Slack message in attachment. It is advised not to override this.
SLACK_TITLE       | Message                                               | Title to use before main Slack message.
SHOW_ACTIONS_URL  | true                                                  | Show the actions url field
SITE_NAME         | development                                           | Environment sitename
SITE_URL          | -                                                     | Site URL

You can see the action block with all variables as below:

```yml
    - name: Slack Notification
      uses: partnerhero/action-slack-notify@v2.0.0
      env:
        SLACK_CHANNEL: general
        SLACK_COLOR: '#3278BD'
        SLACK_ICON: https://github.com/rtCamp.png?size=48
        SLACK_MESSAGE: 'Post Content :rocket:'
        SLACK_TITLE: Post Title
        SLACK_USERNAME: Devops Bot
        SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
        SHOW_ACTIONS_URL: true
        SITE_NAME: staging
        SITE_URL: http://myapp.staging.com
```

Below screenshot help you visualize message part controlled by different variables:

<img width="600" alt="Screenshot_2019-03-26_at_5_56_05_PM" src="https://user-images.githubusercontent.com/4115/54997488-d1f94e00-4ff1-11e9-897f-a35ab90f525f.png">

The `Site` and `SSH Host` details are only available if this action is run after [Deploy WordPress GitHub action](https://github.com/rtCamp/action-deploy-wordpress).

## Hashicorp Vault (Optional)

This GitHub action supports [Hashicorp Vault](https://www.vaultproject.io/). 

To enable Hashicorp Vault support, please define following GitHub secrets:

Variable      | Purpose                                                                       | Example Vaule
--------------|-------------------------------------------------------------------------------|-------------
`VAULT_ADDR`  | [Vault server address](https://www.vaultproject.io/docs/commands/#vault_addr) | `https://example.com:8200`
`VAULT_TOKEN` | [Vault token](https://www.vaultproject.io/docs/concepts/tokens.html)          | `s.gIX5MKov9TUp7iiIqhrP1HgN`

You will need to change `secrets` line in `slack-notify.yml` file to look like below.

```yml
on: push
name: Slack Notification Demo
jobs:
  slackNotification:
    name: Slack Notification
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Slack Notification
      uses: rtCamp/action-slack-notify@v2.0.0
      env:
        VAULT_ADDR: ${{ secrets.VAULT_ADDR }}
        VAULT_TOKEN: ${{ secrets.VAULT_TOKEN }}
```

GitHub action uses `VAULT_TOKEN` to connect to `VAULT_ADDR` to retrieve slack webhook from Vault.

In the Vault, the Slack webhook should be setup as field `webhook` on path `secret/slack`.

## License

[MIT](LICENSE) © 2019 rtCamp

This action was forked from https://github.com/rtCamp/action-slack-notify created by [rtCamp](https://github.com/rtCamp/).

### Made with love and coffe! @ [PartnerHero](https://partnerhero.com/)

<a
  href="https://partnerhero.com/">
  <img
    src="https://partnerhero.com/static/ph-logo-red-74c934089759f0f99f33e26cd77baf06.svg"
    alt="Outsourcing for Startups. From bootstrap through IPO and beyond. We’ve got you.">
</a>
