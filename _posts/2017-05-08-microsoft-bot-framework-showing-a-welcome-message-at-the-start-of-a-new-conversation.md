---
id: 8119
title: 'Microsoft Bot Framework: showing a welcome message at the start of a new conversation'
date: 2017-05-08T14:33:08+00:00
author: Davide
layout: post
guid: http://www.davidezordan.net/blog/?p=8119
permalink: /?p=8119
categories:
  - .NET
  - Azure
  - Cloud Computing
  - Programming
category: blog
tags:
  - Bot Framework
  - Microsoft Azure
---
<p style="text-align: justify;">Recently I've worked on some projects related to <a href="https://docs.microsoft.com/en-us/Bot-Framework/index" target="_blank" rel="noopener noreferrer"><strong>Bot Framework</strong></a> and enjoyed the functionalities which permit to automate actions in response to user interactions.</p>
<p style="text-align: justify;">It is important to provide the user with a great experience: one "nice touch" can be achieved by providing a welcome message at the beginning of a new conversation.</p>
<p style="text-align: justify;">The first solution I tried was triggering the welcome message in the <em>ConversationUpdate</em> activity:</p>

<pre title="Simple welcome message" class="lang:default decode:true">private Activity HandleSystemMessage(Activity message)
{
    if (message.Type == ActivityTypes.DeleteUserData)
    {
        // Implement user deletion here
        // If we handle user deletion, return a real message
    }
    else if (message.Type == ActivityTypes.ConversationUpdate)
    {
        // Handle conversation state changes, like members being added and removed
        // Use Activity.MembersAdded and Activity.MembersRemoved and Activity.Action for info
        // Not available in all channels

        // Note: Add introduction here:
        ConnectorClient connector = new ConnectorClient(new Uri(message.ServiceUrl));
        Activity reply = message.CreateReply("Hello from my simple Bot!");
        connector.Conversations.ReplyToActivityAsync(reply);
    }
    else if (message.Type == ActivityTypes.ContactRelationUpdate)
    {
        // Handle add/remove from contact lists
        // Activity.From + Activity.Action represent what happened
    }
    else if (message.Type == ActivityTypes.Typing)
    {
        // Handle knowing tha the user is typing
    }
    else if (message.Type == ActivityTypes.Ping)
    {
    }

    return null;
}</pre>
<p style="text-align: justify;">When I ran this, the message was presented twice in the <strong><a href="https://docs.microsoft.com/en-us/bot-framework/debug-bots-emulator" target="_blank" rel="noopener noreferrer">Bot Framework Emulator</a></strong>:</p>
<img class="alignleft wp-image-8122 size-large" src="http://www.davidezordan.net/blog/wp-content/uploads/2017/05/Welcome-Twice-1024x541.png" alt="" width="660" height="349" />
<p style="text-align: justify;">After some investigation, I discovered that the <em>ConversationUpdate</em> activity is triggered both when the connection to the Bot is established and when a new user joins the conversation.</p>
<p style="text-align: justify;">As explained on <strong><a href="https://github.com/Microsoft/BotFramework-Emulator/issues/99" target="_blank" rel="noopener noreferrer">GitHub</a></strong>, the correct way to handle this case is by showing the welcome message only when a new user is added:</p>

<pre title="Showing welcome message when user is added" class="lang:default decode:true">private Activity HandleSystemMessage(Activity message)
{
    if (message.Type == ActivityTypes.DeleteUserData)
    {
        // Implement user deletion here
        // If we handle user deletion, return a real message
    }
    else if (message.Type == ActivityTypes.ConversationUpdate)
    {
        // Handle conversation state changes, like members being added and removed
        // Use Activity.MembersAdded and Activity.MembersRemoved and Activity.Action for info
        // Not available in all channels

        // Note: Add introduction here:
        IConversationUpdateActivity update = message;
        var client = new ConnectorClient(new Uri(message.ServiceUrl), new MicrosoftAppCredentials());
        if (update.MembersAdded != null &amp;&amp; update.MembersAdded.Any())
        {
            foreach (var newMember in update.MembersAdded)
            {
                if (newMember.Id != message.Recipient.Id)
                {
                    var reply = message.CreateReply();
                    reply.Text = $"Welcome {newMember.Name}!";
                    client.Conversations.ReplyToActivityAsync(reply);
                }
            }
        }
    }
    else if (message.Type == ActivityTypes.ContactRelationUpdate)
    {
        // Handle add/remove from contact lists
        // Activity.From + Activity.Action represent what happened
    }
    else if (message.Type == ActivityTypes.Typing)
    {
        // Handle knowing tha the user is typing
    }
    else if (message.Type == ActivityTypes.Ping)
    {
    }

    return null;
}</pre>
<p style="text-align: justify;">Using this approach the welcome message is displayed properly:</p>
<img class="size-large wp-image-8123 alignleft" src="http://www.davidezordan.net/blog/wp-content/uploads/2017/05/Welcome-Message-1024x541.png" alt="" width="660" height="349" />
<p style="text-align: justify;">Happy coding!</p>