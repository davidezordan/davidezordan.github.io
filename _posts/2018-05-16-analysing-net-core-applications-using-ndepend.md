---
id: 8307
title: Analysing .NET Core applications using NDepend
date: 2018-05-16T14:55:55+00:00
author: Davide
layout: post
guid: https://www.davidezordan.net/blog/?p=8307
permalink: /?p=8307
categories:
  - .NET
  - Programming
tags:
  - .NET
  - .NET Core
  - 'C#'
---
I previously blogged about NDepend and its capabilities to perform static code analysis and obtain in-depth information about code metrics via its CQLinq features.

I have used in the past NDepend to analyse codebases of .NET apps with great results, always obtaining advanced suggestions on how to improve the code and often highlighting issues that could have prevented maintainability.

Recently, I was contacted by Patrick Smacchia, creator of NDepend, for providing some feedback about the last release, so I thought it was a good opportunity to dig more in-depth about the latest features available in the v2018.1 public release <a href="https://www.ndepend.com/whatsnew" target="_blank" rel="noopener">https://www.ndepend.com/whatsnew</a>.

I was very pleased to verify the support available for .NET Core, including .NET Core 2.1 Preview.

To test it, I downloaded a trial of NDepend from <a href="https://www.ndepend.com/download" target="_blank" rel="noopener">here</a> and installed on my system running Windows 10 April 2018 update with Visual Studio 2017 Version 15.7.0: all worked well and got the NDepend menu appearing when launching the IDE.
<h2>Exploring new .NET Core 2.1 APIs</h2>
Since I tried recently .NET Core 2.1.0 preview 2, I explored the differences between this version and version 2.0.7 available on my machine under the folder <strong>C:\Program Files\dotnet\shared\Microsoft.NETCore.App</strong>: using the <em>NDEPEND-&gt;Diff</em> menu I selected the assemblies related to the two versions

<img class="aligncenter size-large wp-image-8308" src="https://www.davidezordan.net/blog/wp-content/uploads/2018/05/NDepend-Select-assemblies-1024x419.png" alt="" width="660" height="270" />

And I was able to obtain a list of the new public methods available in the latest release:

<img class="aligncenter size-large wp-image-8309" src="https://www.davidezordan.net/blog/wp-content/uploads/2018/05/NDepend-New-public-methods-992x1024.png" alt="" width="660" height="681" />

Then a detailed view of other metrics using the NDepend dashboard including a complex dependency map:

<img class="aligncenter size-large wp-image-8310" src="https://www.davidezordan.net/blog/wp-content/uploads/2018/05/NDepend-Code-metrics-Diff-1024x491.png" alt="" width="660" height="316" />

It’s possible to extract numerous information related to the differences between the two .NET Core versions including assemblies, public methods, types, fields added/removed plus more.

A useful article available on the <a href="https://blog.ndepend.com/new-net-core-2-1-asp-net-core-2-1-apis/" target="_blank" rel="noopener">NDepend blog</a> explains the different options available and additional in-depth articles are referenced below:
<ul>
 	<li><a href="https://www.ndepend.com/docs/code-diff-in-visual-studio" target="_blank" rel="noopener">Advanced Code Diff from within Visual Studio</a></li>
 	<li><a href="https://www.ndepend.com/docs/cqlinq-features#Diff" target="_blank" rel="noopener">Querying the code diff</a></li>
 	<li><a href="https://www.ndepend.com/docs/reporting-code-diff" target="_blank" rel="noopener">Reporting Code Diff</a></li>
 	<li><a href="https://www.ndepend.com/docs/code-search#Change" target="_blank" rel="noopener">Search in Diff between two versions of the code</a></li>
</ul>
As a next step, I created a new blank ASP.NET Core Web Application and ran a code analysis and obtained a list of queries and rules starting from the NDepend dashboard:

<img class="aligncenter size-large wp-image-8311" src="https://www.davidezordan.net/blog/wp-content/uploads/2018/05/NDepend-Dashboard-1024x477.png" alt="" width="660" height="307" />

It is possible to discover a variety of rules, object-oriented design, breaking changes, naming conventions, source file organisation, architecture improvements and more to enhance the codebase of the .NET Core app: this is a real time saver when exploring new codebases or performing code reviews.

A comprehensive list of the new features available in NDepend v2018.1 is available <a href="https://www.ndepend.com/whatsnew" target="_blank" rel="noopener">here</a>, including support for .NET Core and  DDD ubiquitous language check as explained in depth on the official <a href="https://blog.ndepend.com/checking-ddd-ubiquitous-language-with-ndepend/" target="_blank" rel="noopener">blog</a>.

Happy coding!