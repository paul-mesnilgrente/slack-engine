---
title: Using the Web API
lang: en
slug: web-api
order: 4
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
You can call [any Web API method](https://api.slack.com/methods) using the
[`WebClient`](https://slack.dev/node-slack-sdk/web-api) provided to your Slack
app as `Slack::Client` (given that your app has the appropriate scopes). When
you call one the client's methods, it returns a Promise containing the response
from Slack.

The token used to initialize Bolt can be found in the `context` object, which is
required for most Web API methods.
  </div>

```ruby
# Unix Epoch time for September 30, 2019 11:59:59 PM
const WHEN_SEPTEMBER_ENDS = 1569887999

message 'wake me up' do |message, context|
  # Call the chat.scheduleMessage method with a token
  result = Slack::Client.chat.scheduleMessage(
    # The token you used to initialize your app is stored in the context object
    token: context.botToken,
    channel: message.channel.id,
    post_at: WHEN_SEPTEMBER_ENDS,
    text: 'Summer has come and passed'
  )
rescue Slack::Client::BaseError => error
  Rails.logger.error(error)
end
```
</div>
