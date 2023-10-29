
****************************************MAUI CUSTOM LOADER*******************************
===>USE NUGET PACKAGE OF "MAUI.Custom.LoaderEase"

===>COMPLETE TUTORIAL PATH :"https://www.youtube.com/watch?v=WLsubl_L9Jk"

===>MAUI LOADER INITIAL SETUP

 MAUILoaderRegisterationSetup.SetConfigurationForXamarinCustomLoader(mauiWaitLoaderColor: Color.FromArgb("#FFFFFF"), loaderTextColor: Color.FromArgb("#FFFFFF"), loaderHeightRequest: 250.0, loaderWidthRequest: 250.0, loaderFontSize: 16.0);

===>DEPENDECNY INJECTION SERVICE
using CommunityToolkit.Maui;
using MAUI.Custom.LoaderEase.AppPresentations.CommonSource;
using MAUI.Custom.LoaderEase.AppPresentations.Services;
using MAUI.Custom.LoaderEase.Interfaces;

namespace MauiLoaderTesting
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            try
            {
                var builder = MauiApp.CreateBuilder();
                builder.UseMauiApp<App>().ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });
                RegisterEssentials(builder.Services);
                RegisterPages(builder.Services);
                RegisterViewModels(builder.Services);
                builder.UseMauiCommunityToolkit();//Registering for MAUI loader
                var app = builder.Build();
                MauiServiceHandler.MauiAppBuilder = app;
                return app;
            }
            catch (Exception ex)
            {
                return null;
            }
        }
        static void RegisterPages(in IServiceCollection services)
        {
            services.AddTransient<MainPage>();
        }
        static void RegisterViewModels(in IServiceCollection services)
        {
            services.AddTransient<MainPageViewModel>();
        }
        static void RegisterEssentials(in IServiceCollection services)
        {
            //LOADER SERVICE HANDLER
            services.AddSingleton<ICustomLoaderHandlerService, CustomLoaderHandlerService>();
        }
    }
}

====>MVVM CONSUMING PROCESS

using MAUI.Custom.LoaderEase.Interfaces;
using MAUI.Custom.LoaderEase.Utility;

namespace MauiLoaderTesting
{
    public class MainPageViewModel
    {
        ICustomLoaderHandlerService loaderHandler = null;

        [Obsolete]
        public MainPageViewModel(ICustomLoaderHandlerService loaderHandler)
        {
            this.loaderHandler = loaderHandler;
            ShowWaitWindow();
            CloseLoader();
        }
        private void ShowWaitWindow()
        {
            if (loaderHandler != null)
                loaderHandler.ShowCustomLoader(message: "Processing", loaderType: LoaderType.SonicSyncLoader);

            //LoaderHandler.ShowCustomLoader(message: "Searching", loaderType: LoaderType.QuantumQuikLoader);
        }
        [Obsolete]
        private void CloseLoader()
        {
            Device.StartTimer(TimeSpan.FromSeconds(10), () =>
            {
                loaderHandler.HideCustomLoader();
                //LoaderHandler.HideCustomLoader();
                return false;
            });
        }
    }
}

