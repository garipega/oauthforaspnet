---
layout: guide
title:  "Walkthrough: Configuring Twitter Sign-In for ASP.NET MVC 5 and Visual Studio 2013"
---

## Introduction
A lot of applications these days allow users to sign in using their existing login credentials from a social networking service such as Twitter.  This simplifies the login process as users do not have to remember multiple login credentials for various websites, and it also provides the application developer in turn access to certain demographical information from the user.

ASP.NET MVC 5 has support for social logins built in, but as an app developer you will still need to go trough a few steps to enable this on your application.  This guide will help you through the process in a step-by-step manner.

To follow this guide you will need to have a Twitter account.  If you do not have an account then head on over to the [Twitter Homepage](http://www.twitter.com) and register before you continue any further.

## Registering an application in Twitter
In order for your application to use Twitter as a login mechanism you will need to create an application in Twitter.  To do this head over to the [Twitter Developers Website](https://dev.twitter.com/).  If you are not yet signed then sign in. 

Go to your applications list by clicking on your profile picture on the top right and selecting "My applications".

![](/images/guides/twitter/applications_menu.png)

On the "Twitter Apps" page you will see a list of your existing Twitter applications (if you have any).  Click on the "Create New App" button.

![](/images/guides/twitter/create_new_app_button.png)

Supply the name and description for your application.  It is also important to supply a URL for both the **Website** and the **Callback URL** fields.   

![](/images/guides/twitter/create_application_1.png)

The Callback URL does not have to be the exact URL to which the post-authentication callback will be made - the ASP.NET Identity system will ensure that the correct Callback URL is supplied at runtime.  It is however important to specify a Callback URL otherwise the Twitter authentication will not work.

Finally read and accept the "Developer Rules of the Road" and click on the "Create your Twitter application" button.

![](/images/guides/twitter/create_application_2.png)

After the application has been created you can navigate to the "API Keys" tab and take note of the "API Key" and "API Secret" as these will be needed when enabling Twitter authentication in your ASP.NET MVC application.

![](/images/guides/twitter/twitter_app_keys_page.png)

## Enabling Twitter authentication in your ASP.NET MVC Application
The next step is to add the Twitter login to your ASP.NET MVC application.  For this we will create a new ASP.NET MVC application using Visual Studio. Go to File > New > Project and select the template for a new "ASP.NET Web Application" and click "OK".

![](/images/guides/twitter/new_project.png)

Next, select the MVC project template and ensure that the **authentication** method is set to "Individual User Accounts".  Click "OK".

![](/images/guides/twitter/new_project_mvc.png)

> After the project wizard has completed I would advise you to update your NuGet packages before you proceed.  To do this you can right click on the solution file and select "Manage NuGet Packages for Solution...".  In the "Manage Nuget Packages" dialog you can navigate to the Updates node and ensure that you install any updates.

Once the application has been created you can navigate to the `Startup.Auth` file located in the `App_Start` folder of your application and open the file.

![](/images/guides/twitter/navigate_startup_auth.png)

Locate the lines of code which enables the Twitter authentication (look for `app.UseTwitterAuthentication`) and uncomment it.  Take the values for the "API Key" and "API Secret" from your Twitter application and pass it through as the parameters for the `app.UseTwitterAuthentication` method call.  Pass the "API Key" as the value for the `consumerKey` parameter and the "API Secret" as the value for the `consumerSecret` parameter.

{% highlight csharp %}
app.UseTwitterAuthentication(
    consumerKey: "YUoDrKu6RWm4452Ft0qkSKmmg",
    consumerSecret: "foN1v32pj5vKpDZrosYzCvcArAPpsKd7uYNHobzB4DdoFxBo3o");
{% endhighlight %}

It is important to ensure that these parameters match the values from Twitter exactly, otherwise the Twitter authentication for your application will fail.

![](/images/guides/twitter/keys_matchup.png)

## Testing the application
You have now created an application in Twitter and enabled the Twitter authentication in your application.  The last step is to ensure that everything works.  Run your application by selecting the Debug > Start Debugging menu item or pressing the F5 key in Visual Studio.

The application will open in your web browser.  Select the "Log In" menu at the top.

![](/images/guides/twitter/application_start_screen.png)

Under the "Use another service to log in" section you will see a button which allows you to log in with Twitter.  Click the "Twitter" button.

![](/images/guides/twitter/application_login_screen.png)

You will be redirected to the Twitter website.  If you are not logged in to Twitter yet you will be prompted to do so.  Twitter will then prompt you to give the application access to your account.

![](/images/guides/twitter/twitter_auth_screen.png)

Click on the "Authorize App" button. You will be redirected back to your application and will need to supply your email address to complete the registration process.

![](/images/guides/twitter/complete_registration.png)

Once you have filled in your email address and clicked the "Register" button you will be logged into the application.  You can now log in to the application using your Twitter account in the future.
