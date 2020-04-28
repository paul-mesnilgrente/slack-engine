---
title: Listening and responding to commands
---
Listening and responding to commands

Your app can use the command() method to listen to incoming slash command events. The method requires a commandName of type string.

Commands must be acknowledged with ack() to inform Slack your app has received the event.

There are two ways to respond to slash commands. The first way is to use say(), which accepts a string or JSON payload. The second is respond() which is a utility for the response_url. These are explained in more depth in the responding to actions section.

When configuring commands within your app configuration, you’ll continue to append /slack/events to your request URL.

```js
// The echo command simply echoes on command
app.command('/echo', async ({ command, ack, say }) => {
  // Acknowledge command request
  await ack();

  await say(`${command.text}`);
});
```
