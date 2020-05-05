---
title: Sending messages
lang: en
slug: message-sending
order: 2
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
Within your controllers, `say()` is available whenever there is an
associated conversation (for example, a conversation where the event or action
which triggered the listener occurred). `say()` accepts a string to post simple
messages and JSON payloads to send more complex messages. The message payload
you pass in will be sent to the associated conversation.

In the case that you'd like to send a message outside of a listener or you want
to do something more advanced (like handle specific errors), you can call
`Slack::Client.chat.postMessage` using the [client][web-api] attached to your
Slack on Rails instance.
  </div>

```ruby
# Listens for messages containing "knock knock" and responds with an italicized
# "who's there?"
message 'knock knock' do
  say("_Who's there?_")
end
```
</div>


<details class="secondary-wrapper">
  <summary class="section-head">
    <h4>Sending a message with blocks</h4>
  </summary>

  <div class="details-content" markdown="1">

<div class="section-description" markdown="1">
`say()` accepts more complex message payloads to make it easy to add
functionality and structure to your messages.

To explore adding rich message layouts to your app, read through
[the guide on our API site][composing-layouts] and look through templates of
common app flows [in the Block Kit Builder][block-kit-builder].
</div>

```ruby
# Sends a section block with datepicker when someone reacts with a 📅 emoji
event 'reaction_added' do |event|
  if event.reaction === 'calendar'
    say({
      blocks: [{
        type: 'section',
        text: {
          type: 'mrkdwn',
          text: 'Pick a date for me to remind you'
        },
        accessory: {
          type: 'datepicker',
          action_id: 'datepicker_remind',
          initial_date: '2019-04-28',
          placeholder: {
            type: 'plain_text',
            text: 'Select a date'
          }
        }
      }]
    })
  end
end
```
</div>
</details>

[composing-layouts]: https://api.slack.com/messaging/composing/layouts
[block-kit-builder]: https://api.slack.com/tools/block-kit-builder?template=1
[web-api]: #using-the-web-api
