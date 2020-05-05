---
title: Listening to messages
lang: en
slug: message-listening
order: 1
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
To listen to messages that [your app has access to receive][message-perms],
you can use the `message()` method which filters out events that aren’t of type
`message`.

`message()` accepts an optional `pattern` parameter of type `string` or `RegExp`
object which filters out any messages that don’t match the pattern.
  </div>

```ruby
# routes.rb
slack_on_rails :message, controllers: { messages: 'slack/messages' }

# app/controllers/slack/messages_controller.rb
class MessageController < SlackOnRails::MessageController
  # This will match any message that contains 👋
  message ':wave:', with: :answer_wave

private
  def answer_wave(message)
    say("Hello, #{message.user}")
  end
end
```
</div>


<details class="secondary-wrapper">
  <summary class="section-head">
    <h4>Using a RegExp pattern</h4>
  </summary>

  <div class="details-content" markdown="1">

<div class="section-description" markdown="1">
A RegExp pattern can be used instead of a string for more granular matching.

All of the results of the RegExp match will be in `context`.
</div>

```ruby
class MessageController < SlackOnRails::MessageController
  # This will match any message that contains 👋
  message /^(hi|hello|hey).*/, with: :answer_wave

private
  def answer_wave(message, context)
    greeting = context[0]
    say("Hello, #{message.user}")
    say("${greeting}, how are you?")
  end
end
```

</div>
</details>

[message-perms]: https://api.slack.com/messaging/retrieving#permissions
