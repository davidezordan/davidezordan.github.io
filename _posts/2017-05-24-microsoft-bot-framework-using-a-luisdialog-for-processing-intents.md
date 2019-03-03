---
id: 8163
title: 'Microsoft Bot Framework: using a LuisDialog for processing intents'
date: 2017-05-24T21:52:45+00:00
author: davidezordan
layout: post
guid: http://www.davidezordan.net/blog/?p=8163
# permalink: /?p=8163
categories:
  - .NET
  - Azure
  - Cloud Computing
  - Programming
category: blog
tags:
  - Bot Framework
  - LUIS
---
<p style="text-align: justify;">In the previous post, <a href="http://www.davidezordan.net/blog/?p=8130" target="_blank" rel="noopener noreferrer">I blogged about integrating a Holographic 2D app with Bot Framework and LUIS</a>.</p>
<p style="text-align: justify;">I also spent some time going through these <a href="https://github.com/Microsoft/BotBuilder-Samples" target="_blank" rel="noopener noreferrer">great samples</a> for some presentations and found a very nice implementation for handling the Bot messages when LUIS intents are recognised.</p>
<p style="text-align: justify;">Instead of using the exposed LUIS endpoint and parsing the returned JSON, the framework already provides a specific <strong>LuisDialog&lt;&gt;</strong> type which can be used for handling the various intents, in order to make the code cleaner and more extensible.</p>
<p style="text-align: justify;">I've then modified the <strong>HoloLensBotDemo</strong> sample and added a new <strong>RootLuisDialog</strong>:</p>

<pre title="RootLuisDialog for handling intents" class="lang:default decode:true">[LuisModel("YourModelId", "YourSubscriptionKey")]
[Serializable]
public class RootLuisDialog : LuisDialog&lt;object&gt;
{
    [LuisIntent("")]
    [LuisIntent("None")]
    public async Task None(IDialogContext context, LuisResult result)
    {
        var errorResult = "Try something else, please.";
        await context.PostAsync(errorResult);
        context.Wait(this.MessageReceived);
    }

    [LuisIntent("FavouriteTechnologiesIntent")]
    public async Task FavouriteTechnologiesIntent(IDialogContext context, IAwaitable&lt;IMessageActivity&gt; activity, LuisResult result)
    {
        await context.PostAsync("My favourite technologies are Azure, Mixed Reality and Xamarin!");
        context.Wait(this.MessageReceived);
    }

    [LuisIntent("FavouriteColorIntent")]
    public async Task FavouriteColorIntent(IDialogContext context, IAwaitable&lt;IMessageActivity&gt; activity, LuisResult result)
    {
        await context.PostAsync("My favourite color is Blue!");
        context.Wait(this.MessageReceived);
    }

    [LuisIntent("WhatsYourNameIntent")]
    public async Task WhatsYourNameIntent(IDialogContext context, IAwaitable&lt;IMessageActivity&gt; activity, LuisResult result)
    {
        await context.PostAsync("My name is Davide, of course :)");
        context.Wait(this.MessageReceived);
    }

    [LuisIntent("HelloIntent")]
    public async Task HelloIntent(IDialogContext context, IAwaitable&lt;IMessageActivity&gt; activity, LuisResult result)
    {
        await context.PostAsync(@"Hi there! Davide here :) This is my personal Bot. Try asking 'What are your favourite technologies?'");
        context.Wait(this.MessageReceived);
    }
}</pre>
And this is all the code now needed for the Bot.

The updated source code is available on <strong><a href="https://github.com/davidezordan/HoloLens-Bot-Demo" target="_blank" rel="noopener noreferrer">GitHub</a></strong>.

Happy coding!

&nbsp;