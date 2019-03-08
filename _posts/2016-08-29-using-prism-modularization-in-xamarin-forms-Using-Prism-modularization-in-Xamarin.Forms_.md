---
id: 7893
title: Using Prism modularization in Xamarin.Forms
date: 2016-08-29T10:48:29+00:00
author: davidezordan
layout: post
guid: http://www.davidezordan.net/blog/?p=7893
# permalink: /?p=7893
categories:
  - .NET
  - Android
  - iOS
  - Mobile
  - Programming
  - Universal Windows Platform
  - Xamarin
tags:
  - Android
  - iOS
  - Mobile
  - Universal Windows Platform
  - Xamarin.Forms
---
<p style="text-align: justify;">Recently, Prism for Xamarin.Forms 6.2.0 has been released with many notable improvements including a new bootstrapping process, AutoWireViewModel behaviour changes, deep-linking support,&nbsp;&nbsp;modularity and <a href="https://visualstudiogallery.msdn.microsoft.com/e7b6bde2-ba59-43dd-9d14-58409940ffa0" target="_blank" rel="noopener noreferrer">Prism&nbsp;template pack</a> enhancements (full release notes available <a href="https://github.com/PrismLibrary/Prism/wiki/Release-Notes-6.2.0" target="_blank" rel="noopener noreferrer">here</a>).</p>

<p style="text-align: justify;">Today, I fired up Visual Studio to have a play with&nbsp;this new version&nbsp;and decided to try the Xamarin.Forms support for Prism Modules: this is a very useful feature which&nbsp;allows to separate clearly the various parts of the application and improve quality, extensibility&nbsp;and readability of the code.</p>

<p style="text-align: justify;">After downloading the new template pack, I created a new solution selecting New Project =&gt; Templates =&gt; Visual C# =&gt; Prism =&gt; Forms =&gt; Prism Unity App:</p>

<p style="text-align: justify;"><img class="size-medium wp-image-7904 aligncenter" src="http://www.davidezordan.net/blog/wp-content/uploads/2016/08/PrismTemplatePack-256x300.png" alt="PrismTemplatePack" width="256" height="300"></p>

<p style="text-align: justify;">The new wizard is very useful and permits to select the platforms to target in the project: I selected Android, iOS and UWP and the project was generated targeting the three platforms with a shared PCL. NuGet packages were already updated to the latest version so no need for further actions.</p>

<p style="text-align: justify;">While exploring the new project structure and the new modularization stuff, I decided to create a new Xamarin.Forms portable class library containing a module with a single View/ViewModel (<strong>SamplePage&nbsp;</strong>/&nbsp;<strong>SamplePageViewModel</strong>) visualised when a user interacts with a button on the home page.</p>

<p style="text-align: justify;">The new module required the definition of the following class implementing the Prism <strong>IModule</strong> interface:</p>

<pre class="lang:default decode:true" title="IModule implementation">using Microsoft.Practices.Unity;
using Prism.Modularity;
using Prism.Unity;
using UsingModules.SampleModule.Views;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace UsingModules.SampleModule
{
    public class SampleModule : IModule
    {
        private readonly IUnityContainer _unityContainer;
        public SampleModule(IUnityContainer unityContainer)
        {
            _unityContainer = unityContainer;
        }

        public void Initialize()
        {
            _unityContainer.RegisterTypeForNavigation&lt;SamplePage&gt;();
        }
    }
}</pre>

<p style="text-align: justify;">To keep the logic separated from the rest of the app, I decided to register the navigation type for <strong>SamplePage&nbsp;</strong>inside the <strong>Initialize()</strong> method of the module which will be triggered when the module loads.</p>

<p style="text-align: justify;">I also applied Xamarin.Forms <a href="https://developer.xamarin.com/guides/xamarin-forms/xaml/xamlc/">XAML compilation</a>&nbsp;to the module to improve performance, which is always great to have ;)</p>

<pre class="lang:default decode:true " title="XAML compilation">[assembly: XamlCompilation (XamlCompilationOptions.Compile)]</pre>

<p style="text-align: justify;">It's worth noting that in this new Prism release the default value for the attached property&nbsp;<strong>ViewModelLocator.AutowireViewModel</strong> is set to true, so we can omit it and the framework will automatically&nbsp;associate <strong>SampleViewModel</strong> as the BindingContext for the view:</p>

<pre class="lang:default decode:true" title="SamplePage XAML in the module">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="UsingModules.SampleModule.Views.SamplePage"
             Title="SamplePage"&gt;
  &lt;StackLayout HorizontalOptions="Center" VerticalOptions="Center"&gt;
     &lt;Label Text="{Binding Title}" /&gt; 
  &lt;/StackLayout&gt;
&lt;/ContentPage&gt;</pre>

<pre class="lang:default decode:true" title="And the corresponding ViewModel">using Prism.Mvvm;
using Prism.Navigation;

namespace UsingModules.SampleModule.ViewModels
{
    public class SamplePageViewModel : BindableBase, INavigationAware
    {
        private string _title;
        public string Title
        {
            get { return _title; }
            set { SetProperty(ref _title, value); }
        }

        public void OnNavigatedFrom(NavigationParameters parameters)
        {

        }

        public void OnNavigatedTo(NavigationParameters parameters)
        {
            Title = "Sample Page from a Prism module";

            var parameterName = "par";
            if (parameters.ContainsKey(parameterName))
                Title += " - parameter: " + (string)parameters[parameterName];
        }
    }
}</pre>

<p style="text-align: justify;">I then explored the new breaking changes for the bootstrapping process: the application class now needs to inherit from the <strong>PrismApplication</strong>&nbsp;class and two new virtual methods <strong>OnInitialized()</strong>&nbsp;and <strong>RegisterTypes()</strong> permit respectively to specify the implementation for navigating&nbsp;to the home page and registering the&nbsp;known types / ViewModel's for navigation:</p>

<pre class="lang:default decode:true" title="App.xaml.cs">using System;
using Prism.Modularity;
using Prism.Unity;
using UsingModules.Views;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace UsingModules
{
    public partial class App : PrismApplication
    {
        public App(IPlatformInitializer initializer = null) : base(initializer) { }

        protected override void OnInitialized()
        {
            InitializeComponent();

            var navigationPage = "MainNavigationPage/MainPage";
            NavigationService.NavigateAsync($"{navigationPage}?title=Hello%20from%20Xamarin.Forms");
        }

        protected override void RegisterTypes()
        {
            Container.RegisterTypeForNavigation&lt;MainNavigationPage&gt;();
            Container.RegisterTypeForNavigation&lt;MainPage&gt;();
        }

        protected override void ConfigureModuleCatalog()
        {
            Type sampleModuleType = typeof(SampleModule.SampleModule);
            ModuleCatalog.AddModule(
              new ModuleInfo()
              {
                  ModuleName = sampleModuleType.Name,
                  ModuleType = sampleModuleType,
                  InitializationMode = InitializationMode.OnDemand
              });
        }
    }
}</pre>

<p style="text-align: justify;">The third overridden&nbsp;method, <strong>ConfigureModuleCatalog()</strong>, informs the app to initialize the catalog with the module we created previously and set the initialization mode to <strong>OnDemand</strong> which means the module is not activated when the application starts, but it must load explicitly.&nbsp;This feature is particularly useful in cases in which some functionalities of the app must initialise after some other requirements like, for instance, successful authentication&nbsp;or applications extended via modules.</p>

<p style="text-align: justify;">The sample view was in place, so I proceeded with the implementation of the HomePage: I wrapped the existing one in a NavigationPage to allow the correct back stack and then created two commands for initializing the module and navigating to the SamplePage defined previously:</p>

<pre class="lang:default decode:true" title="MainNavigationPage.xaml">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="UsingModules.Views.MainNavigationPage"
             Title="MainPage"&gt;

&lt;/NavigationPage&gt;</pre>

<pre class="lang:default decode:true " title="MainPage.xaml">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="UsingModules.Views.MainPage"
             Title="MainPage"&gt;
  &lt;StackLayout HorizontalOptions="Center" VerticalOptions="Center"&gt;
    &lt;Label Text="{Binding Title}" /&gt;
    
    &lt;Button Command="{Binding LoadSampleModuleCommand}" Text="Load Sample Module" /&gt;

    &lt;Button Command="{Binding NavigateToSamplePageCommand}" Text="Navigate to Sample Page" /&gt;
  &lt;/StackLayout&gt;
&lt;/ContentPage&gt;</pre>

and the corresponding ViewModel:

<pre class="lang:default decode:true" title="MainPageViewModel">using System.Windows.Input;
using Prism.Commands;
using Prism.Modularity;
using Prism.Mvvm;
using Prism.Navigation;

namespace UsingModules.ViewModels
{
    public class MainPageViewModel : BindableBase, INavigationAware
    {
        private readonly IModuleManager _moduleManager;
        private readonly INavigationService _navigationService;

        public MainPageViewModel(IModuleManager moduleManager, INavigationService navigationService)
        {
            _moduleManager = moduleManager;
            _navigationService = navigationService;

            LoadSampleModuleCommand = new DelegateCommand(LoadSampleModule, ()=&gt;!IsSampleModuleRegistered)
                .ObservesProperty(()=&gt;IsSampleModuleRegistered);
            NavigateToSamplePageCommand = new DelegateCommand(NavigateToSamplePage, ()=&gt;IsSampleModuleRegistered)
                .ObservesProperty(()=&gt;IsSampleModuleRegistered);
        }

        private bool _isSampleModuleRegistered;
        public bool IsSampleModuleRegistered
        {
            get { return _isSampleModuleRegistered; }
            set { SetProperty(ref _isSampleModuleRegistered, value); }
        }

        private string _title;
        public string Title
        {
            get { return _title; }
            set { SetProperty(ref _title, value); }
        }

        public ICommand LoadSampleModuleCommand { get; set; }

        public ICommand NavigateToSamplePageCommand { get; set; }

        private void NavigateToSamplePage()
        {
            _navigationService.NavigateAsync("SamplePage?par=test");
        }

        private void LoadSampleModule()
        {
            _moduleManager.LoadModule("SampleModule");
            IsSampleModuleRegistered = true;
        }

        public void OnNavigatedFrom(NavigationParameters parameters)
        {

        }

        public void OnNavigatedTo(NavigationParameters parameters)
        {
            if (parameters.ContainsKey("title"))
                Title = (string)parameters["title"] + " and Prism";
        }
    }
}</pre>

The module is initialized by injecting the Prism <strong>ModuleManager</strong>&nbsp;and then calling the <strong>LoadModule()</strong> method:

<pre class="lang:default decode:true ">private void LoadSampleModule()
{
   _moduleManager.LoadModule("SampleModule");
   IsSampleModuleRegistered = true;
}</pre>

The navigation to the page defined in the module is performed by:

<pre class="lang:default decode:true " title="Navigate to the SamplePage defined in the module">private void NavigateToSamplePage()
{
   _navigationService.NavigateAsync("SamplePage?par=test");
}</pre>

<p style="text-align: justify;">The property <strong>IsSampleModuleRegistered</strong> permitted to control the CanExecute() for the button commands using this nice fluent syntax using <strong>ObservesProperty(()=&gt;....)</strong> available in Prism:</p>

<pre class="lang:default decode:true " title="ObserveProperty">NavigateToSamplePageCommand = new DelegateCommand(NavigateToSamplePage, ()=&gt;IsSampleModuleRegistered)
.ObservesProperty(()=&gt;IsSampleModuleRegistered);</pre>

<p style="text-align: justify;">Here we go: I found the Prism implementation in this new version very stable and working well with Xamarin.Forms. The modularization capabilities are useful to write clean, maintainable and extensible apps.</p>

<p style="text-align: justify;">The source code is available for download as part of the official&nbsp;<a href="https://github.com/PrismLibrary/Prism-Samples-Forms/tree/master/UsingModules">Prism samples</a>&nbsp;on GitHub.</p>

<p style="text-align: justify;">Looking forward to exploring all the other capabilities available in Prism and Xamarin.Forms. Happy coding!</p>