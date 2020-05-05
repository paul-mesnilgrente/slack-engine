---
title: Listening and responding to shortcuts
lang: en
slug: shortcuts
order: 8
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
The `shortcut()` method supports both [global shortcuts][global-shortcuts] and
[message shortcuts][message-shortcuts].

Shortcuts are invokable UI elements within Slack clients. For global shortcuts,
they are available in the composer and search menus. For message shortcuts, they
are available in the context menus of messages. Your app can use the
`shortcut()` method to listen to incoming shortcut events. The method requires a
`callback_id` parameter of type `string` or `RegExp`.

Shortcuts must be acknowledged with `ack()` to inform Slack that your app has
received the event.

Shortcuts include a `trigger_id` which an app can use to
[open a modal](#creating-modals) that confirms the action the user is taking.

When configuring shortcuts within your app configuration, you'll continue to
append `/slack/events` to your request URL.

⚠️ Note that global shortcuts do **not** include a channel ID. If your app needs
access to a channel ID, you may use a [`conversations_select`][conv-select]
element within a modal. Message shortcuts do include channel ID.
</div>

```ruby
# The open_modal shortcut opens a plain old modal
shortcut 'open_modal' do |shortcut, ack, context, client|
  # Acknowledge shortcut request
  ack

  # Call the views.open method using one of the built-in WebClients
  result = Slack::Client.views.open({
    # The token you used to initialize your app is stored in the `context` object
    token: context.botToken,
    trigger_id: shortcut.trigger_id,
    view: {
      type: "modal",
      title: {
        type: "plain_text",
        text: "My App"
      },
      close: {
        type: "plain_text",
        text: "Close"
      },
      blocks: [
        {
          type: "section",
          text: {
            type: "mrkdwn",
            text: "About the simplest modal you could conceive of :smile:\n\nMaybe <https://api.slack.com/reference/block-kit/interactive-components|*make the modal interactive*> or <https://api.slack.com/surfaces/modals/using#modifying|*learn more advanced modal use cases*>."
          }
        },
        {
          type: "context",
          elements: [
            {
              type: "mrkdwn",
              text: "Psssst this modal was designed using <https://api.slack.com/tools/block-kit-builder|*Block Kit Builder*>"
            }
          ]
        }
      ]
    }
  })

  Rails.logger.debug(result)
rescue Slack::Client::BaseError => error
  Rails.logger.error(error)
end
```
</div>

<details class="secondary-wrapper" markdown="1">
  <summary class="section-head" markdown="0">
    <h4>Listening to shortcuts using a constraint object</h4>
  </summary>

  <div class="details-content" markdown="1">

<div class="section-description" markdown="1">
  You can use a constraints object to listen to `callback_id`s, and `type`s.
  Constraints in the object can be of type string or RegExp object.
</div>

```ruby
# Your middleware will only be called when the callback_id matches 'open_modal' AND the type matches 'message_action'
shortcut callback_id: 'open_modal', type: 'message_action' do |shortcut, context|
  # Acknowledge shortcut request
  ack

  # Call the views.open method using one of the built-in WebClients
  result = client.views.open({
    # The token you used to initialize your app is stored in the `context` object
    token: context.botToken,
    trigger_id: shortcut.trigger_id,
    view: {
      type: "modal",
      title: {
        type: "plain_text",
        text: "My App"
      },
      close: {
        type: "plain_text",
        text: "Close"
      },
      blocks: [
        {
          type: "section",
          text: {
            type: "mrkdwn",
            text: "About the simplest modal you could conceive of :smile:\n\nMaybe <https://api.slack.com/reference/block-kit/interactive-components|*make the modal interactive*> or <https://api.slack.com/surfaces/modals/using#modifying|*learn more advanced modal use cases*>."
          }
        },
        {
          type: "context",
          elements: [
            {
              type: "mrkdwn",
              text: "Psssst this modal was designed using <https://api.slack.com/tools/block-kit-builder|*Block Kit Builder*>"
            }
          ]
        }
      ]
    }
  })

  Rails.logger.debug(result)
rescue Slack::Client::BaseError => error
  Rails.logger.error(error)
end
```
</div>
</details>

[global-shortcuts]: https://api.slack.com/interactivity/shortcuts/using#global_shortcuts
[message-shortcuts]: https://api.slack.com/interactivity/shortcuts/using#message_shortcuts
[conv-select]: https://api.slack.com/reference/block-kit/block-elements#conversation_select
