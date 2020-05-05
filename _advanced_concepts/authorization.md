---
title: Authorization
lang: en
slug: authorization
order: 2
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
Authorization is the process of deciding which Slack credentials (such as a bot
token) should be available while processing a specific incoming event.

Custom apps installed on a single workspace can simply use the `token` option at
the time of `App` initialization. However, when your app needs to handle several
tokens, such as cases where it will be installed on multiple workspaces or needs
access to more than one user token, the `authorize` option should be used
instead.

The `authorize` option can be set to a function that takes an event source as
its input, and should return a Promise for an object containing the authorized
credentials. The source contains information about who and where the event is
coming from by using properties like `teamId` (always available), `userId`,
`conversationId`, and `enterpriseId`.

The authorized credentials should also have a few specific properties:
`botToken`, `userToken`, `botId` (required for an app to ignore messages from
itself), and `botUserId`. You can also include any other properties you'd like
to make available on the [`context`](#context) object.

You should always provide either one or both of the `botToken` and `userToken`
properties. At least one of them is necessary to make helpers like `say()` work.
If they are both given, then `botToken` will take precedence.
</div>

```ruby
app = App.new(authorize: authorizeFn, signingSecret: ENV['SLACK_SIGNING_SECRET'])

# NOTE: This is for demonstration purposes only.
# All sensitive data should be stored in a secure database
# Assuming this app only uses bot tokens, the following object represents a model for storing the credentials as the app is installed into multiple workspaces.

installations = [
  {
    enterpriseId: 'E1234A12AB',
    teamId: 'T12345',
    botToken: 'xoxb-123abc',
    botId: 'B1251',
    botUserId: 'U12385',
  },
  {
    teamId: 'T77712',
    botToken: 'xoxb-102anc',
    botId: 'B5910',
    botUserId: 'U1239',
  },
]

authorizeFn = |teamId, enterpriseId|
  # Fetch team info from database
  installations.each do |team|
    # Check for matching teamId and enterpriseId in the installations array
    if (team.teamId == teamId) && (team.enterpriseId == enterpriseId)
      # This is a match. Use these installation credentials.
      return {
        # You could also set userToken instead
        botToken: team.botToken,
        botId: team.botId,
        botUserId: team.botUserId
      }
    end
  end

  raise StandardError, 'No matching authorizations'
}
```
