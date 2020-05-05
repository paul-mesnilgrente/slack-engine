---
title: Listening to events
lang: en
slug: event-listening
order: 3
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
You can listen to [any Events API event](https://api.slack.com/events) using the
`event()` method after subscribing to it in your app configuration. This allows
your app to take action when something happens in Slack, like a user reacting to
a message or joining a channel.

The `event()` method requires an `eventType` of type string.
  </div>

```ruby
class SlackController < Slack::BaseController
  WELCOME_CHANNEL_ID = 'C12345'

  # When a user joins the team, send a message in a predefined channel asking
  # them to introduce themselves
  event 'team_join', with: answer_team_join

private
  def answer_team_join(event, context)
    result = Slack::Client.chat.postMessage(
      token: context.botToken,
      channel: WELCOME_CHANNEL_ID,
      text: "Welcome to the team, #{event.user}! 🎉 You can introduce yourself in this channel."
    )
    Rails.logger.debug(result)
  end
end
```
</div>


<details class="secondary-wrapper">
  <summary class="section-head">
    <h4>Filtering on message subtypes</h4>
  </summary>

  <div class="details-content" markdown="1">

<div class="section-description" markdown="1">
A `message()` listener is equivalent to `event('message')`

You can filter on subtypes of events by using the built-in `matchEventSubtype()`
middleware. Common message subtypes like `bot_message` and `message_replied` can
be found [on the message event page][message-subtypes].
</div>

```ruby
# Matches all messages from bot users
message subtype('bot_message') do |message|
  Rails.logger.debug("The bot user #{message.user} said #{message.text}")
end
```
</div>
</details>

[message-subtypes]: https://api.slack.com/events/message#message_subtypes
