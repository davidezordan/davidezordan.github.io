---
id: 8187
title: 'Experiments with HoloLens, Bot Framework and LUIS: adding text to speech'
date: 2017-07-27T12:40:07+00:00
author: Davide
layout: post
guid: http://www.davidezordan.net/blog/?p=8187
permalink: /?p=8187
categories:
  - .NET
  - Azure
  - Mixed Reality
  - Programming
  - Universal Windows Platform
category: blog
tags:
  - Bot Framework
  - HoloLens
  - LUIS
  - Mixed Reality
  - UWP
---
<p style="text-align: justify;">Previously I blogged about creating a <a href="http://www.davidezordan.net/blog/?p=8130" target="_blank" rel="noopener"><strong>Mixed Reality 2D app integrating with a Bot using LUIS</strong></a> via the Direct Line channel available in the Bot Framework.</p>
<p style="text-align: justify;">I decided to add more interactivity to the app by also enabling text to speech for the messages received by the Bot: this required the addition of a new <strong>MediaElement</strong> for the Speech synthesiser to the main XAML page:</p>

<pre title="Adding MediaElement for text to Speech" class="lang:default decode:true ">&lt;Page
    x:Class="HoloLensBotDemo.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"&gt;

    &lt;Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"&gt;
        &lt;Grid.ColumnDefinitions&gt;
            &lt;ColumnDefinition Width="10"/&gt;
            &lt;ColumnDefinition Width="Auto"/&gt;
            &lt;ColumnDefinition Width="10"/&gt;
            &lt;ColumnDefinition Width="*"/&gt;
            &lt;ColumnDefinition Width="10"/&gt;
        &lt;/Grid.ColumnDefinitions&gt;
        &lt;Grid.RowDefinitions&gt;
            &lt;RowDefinition Height="50"/&gt;
            &lt;RowDefinition Height="50"/&gt;
            &lt;RowDefinition Height="50"/&gt;
            &lt;RowDefinition Height="Auto"/&gt;
        &lt;/Grid.RowDefinitions&gt;
        &lt;TextBlock Text="Command received: " Grid.Column="1" VerticalAlignment="Center" /&gt;
        &lt;TextBox x:Name="TextCommand" Grid.Column="3" VerticalAlignment="Center"/&gt;

        &lt;Button Content="Start Recognition" Click="StartRecognitionButton_Click" Grid.Row="1" Grid.Column="1" VerticalAlignment="Center" /&gt;

        &lt;TextBlock Text="Status: " Grid.Column="1" VerticalAlignment="Center" Grid.Row="2" /&gt;
        &lt;TextBlock x:Name="TextStatus" Grid.Column="3" VerticalAlignment="Center" Grid.Row="2"/&gt;

        &lt;TextBlock Text="Bot response: " Grid.Column="1" VerticalAlignment="Center" Grid.Row="3" /&gt;
        &lt;TextBlock x:Name="TextOutputBot" Foreground="Red" Grid.Column="3" 
                   VerticalAlignment="Center" Width="Auto" Height="Auto" Grid.Row="3"
                   TextWrapping="Wrap" /&gt;
        &lt;MediaElement x:Name="media" /&gt;
    &lt;/Grid&gt;
&lt;/Page&gt;</pre>
<p style="text-align: justify;">Then I initialized a new <strong>SpeechSynthesizer</strong> at the creation of the page:</p>

<pre title="SpeechSynthesizer initialisation" class="lang:default decode:true ">public sealed partial class MainPage: Page
{
    private SpeechSynthesizer synthesizer;
    private SpeechRecognizer recognizer;

    public MainPage()
    {
        this.InitializeComponent();

        InitializeSpeech();
    }

    private async void InitializeSpeech()
    {
        synthesizer = new SpeechSynthesizer();
        recognizer = new SpeechRecognizer();

        media.MediaEnded += Media_MediaEnded;
        recognizer.StateChanged += Recognizer_StateChanged;

        // Compile the dictation grammar by default.
        await recognizer.CompileConstraintsAsync();
    }

    private void Recognizer_StateChanged(SpeechRecognizer sender, SpeechRecognizerStateChangedEventArgs args)
    {
        if (args.State == SpeechRecognizerState.Idle)
        {
            SetTextStatus(string.Empty);
        }

        if (args.State == SpeechRecognizerState.Capturing)
        {
            SetTextStatus("Listening....");
        }
    } 
…….
</pre>
<p style="text-align: justify;">And added a new <strong><em>Speech()</em></strong> method using the media element:</p>

<pre title="Speech() method implementation" class="lang:default decode:true ">private async void Speech(string text)
{
    if (media.CurrentState == MediaElementState.Playing)
    {
        media.Stop();
    }
    else
    {
        try
        {
            // Create a stream from the text. This will be played using a media element.
            SpeechSynthesisStream synthesisStream = await synthesizer.SynthesizeTextToStreamAsync(text);

            // Set the source and start playing the synthesized audio stream.
            media.AutoPlay = true;
            media.SetSource(synthesisStream, synthesisStream.ContentType);
            media.Play();
        }
        catch (System.IO.FileNotFoundException)
        {
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components unavailable");
            await messageDialog.ShowAsync();
        }
        catch (Exception)
        {
            media.AutoPlay = false;
            var messageDialog = new Windows.UI.Popups.MessageDialog("Unable to synthesize text");
            await messageDialog.ShowAsync();
        }
    }
}</pre>
<p style="text-align: justify;">When a new response is received from the Bot, the new <strong>Speech()</strong> method is called:</p>

<pre class="lang:default decode:true ">var result = await directLine.Conversations.GetActivitiesAsync(convId);
if (result.Activities.Count &gt; 0)
{
    var botResponse = result
        .Activities
        .LastOrDefault(a =&gt; a.From != null &amp;&amp; a.From.Name != null &amp;&amp; a.From.Name.Equals("Davide Personal Bot"));
    if (botResponse != null &amp;&amp; !string.IsNullOrEmpty(botResponse.Text))
    {
        var response = botResponse.Text;

        TextOutputBot.Text = "Bot response: " + response;
        TextStatus.Text = string.Empty;

        Speech(response);
    }
}</pre>
<p style="text-align: justify;">And then the recognition for a new phrase is started again via the <strong>MediaEnded</strong> event to simulate a conversation between the user and the Bot:</p>

<pre class="lang:default decode:true ">private void Media_MediaEnded(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
    StartRecognitionButton_Click(null, null);
}</pre>
<p style="text-align: justify;">As usual, the source code is available for download on <strong><a href="https://github.com/davidezordan/HoloLens-Bot-Demo" target="_blank" rel="noopener">GitHub</a></strong>.</p>