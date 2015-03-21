---
layout: provider
title:  Foursquare
logo: foursquare.png
links:
  - name: Foursquare Home Page
    url: https://www.foursquare.com/
  - name: Foursquare Developer Home
    url: https://developer.foursquare.com/
  - name: Your Apps
    url: https://foursquare.com/developers/apps
sdks:
  - name: SharpSquare
    url: https://github.com/TICLAB/SharpSquare
---

## 1. Register an app in Foursquare

Go to the [Foursquare Apps page](https://foursquare.com/developers/apps) and click on the "Create a new app" button. After you completed all the information and created the application, take not of the **Client Id** and **Client secret** as your will need these when enabling the Foursquare OAuth provider in your ASP.NET application.

Next select "Dropbox API App" and the type of datastore access you would like. Specify the name of thea application and click "Create app". After the app has been created, take note of the "App key" and "App secret" as you will need these to enable the Dropbox authentication in your application

![](/images/dropbox-app-key-and-secret.png)

After the app has been created be sure to set the **Redirect URI**. This is the URL to your application with the suffix `/signin-dropbox`, e.g. `http://localhost:4515/signin-dropbox`.

## 2. Install the Nuget Package

Install the Nuget Package which contains the Dropbox OAuth provider.

{% highlight bash %}
Install-Package Install-Package Owin.Security.Providers
{% endhighlight %}

## 3. Register Provider

Locate the file in your project called `\App_Start\Startup.Auth.cs`. Ensure that you have imported the `Owin.Security.Providers.Dropbox` namespace:

{% highlight csharp %}
using Owin.Security.Providers.Dropbox;
{% endhighlight %}

In the `ConfigureAuth` method add the following line of code:

{% highlight csharp %}
app.UseDropboxAuthentication("Your app key", "Your app secret");
{% endhighlight %}

## 4. Advanced Configuration

### Request extra permissions

The permissions required by your application is specified when you register the application in the Dropbox App Console. You cannot request any other permissions during the authentication process.

### Specify an alternative callback path

By default the Dropbox provider will request Dropbox to redirect to the path `/signin-dropbox` after the user has signed in and granted permissions on Dropbox. You can specify an alternative redirect URL:

{% highlight csharp %}
var options = new DropboxAuthenticationOptions
{
    AppKey = "Your app key",
    AppSecret = "Your app secret",
    CallbackPath = new PathString("/oauth-redirect/dropbox")
};
app.UseDropboxAuthentication(options);
{% endhighlight %}

### Retrieve access token and other user information returned from Dropbox

You can retrieve the user information returned from Dropbox in the `OnAuthenticated` callback function which gets invoked after the user has authenticated with Dropbox:

{% highlight csharp %}
var options = new DropboxAuthenticationOptions
{
    AppKey = "Your app key",
    AppSecret = "Your app secret",
    Provider = new DropboxAuthenticationProvider
    {
        OnAuthenticated = async context =>
        {
            // Retrieve the OAuth access token to store for subsequent API calls
            string accessToken = context.AccessToken;

            // Retrieve the user's full name
            string userFullName = context.Name;

            // You can even retrieve the full JSON-serialized user
            var serializedUser = context.User;
        }
    }
};
app.UseDropboxAuthentication(options);
{% endhighlight %}