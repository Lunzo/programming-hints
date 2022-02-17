---
title: "Error Logging with log4net"
date: 2014-03-21T11:44:00
# bookComments: false
# bookSearchExclude: false
tags: .NET C# "error handling" logging
---
When handling errors you want to do one of the following:

* Crash the program.
* Ignore the error.
* Try to fix the error and continue.

The correct approach depends on a number of factors. What is your application? Do you have enough information available to try and recover or infer what the user wanted to do? Is the error going to impact other operations later on? No matter which approach you choose for handling the error, you ought to use logging!

At my current job there are two fairly complicated systems I'm responsible for maintaining. The older of the two has no logging. When something goes wrong it is hard to find out what happened and sometimes it has been downright impossible! I should note that I didn't write this system although I have become familiar with the code.

The newer system, which I wrote, has excellent logging thanks to log4net. A quick scan of the log file allows me to pinpoint the cause of errors. It also enables me to display a generic error message to users (it's a web application) and keep the stack traces and unfriendly developer jargon away from them.

Now that I've explained the need for logging and the benefits to using it, let's look at how I you can do the same with log4net.

Firstly you import the log4net dependency with NuGet. Once imported, copy and paste the following line into the top of any classes which you want to add logging to. N.B. I've split it into two for readability here.

```
private static readonly log4net.ILog log = log4net.LogManager.GetLogger(
    System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);
```

This line is exactly the same in each file. The magic is using reflection to get the name of the current class. In the original log4j you have to copy, paste then remember to update the type name in each file.

Once you have a log variable defined, writing log output is as simple as calling one of the logging methods. log4net supports 4 levels of logging: Error, Warn, Info, Debug. For each level there are several overloads. There is also a `*Format` version which allows for formatted parameters similar to `String.Format`. Here is an example of error logging from an MVC application. I'm overriding the default error handler so any uncaught exceptions in my application are logged with log4net.

```
    /// <summary>
    /// Override of the default MVC HandleErrorAttribute which writes the exception message to log4net.
    /// </summary>
    public class MyHandleErrorAttribute : HandleErrorAttribute
    {
        private static readonly log4net.ILog log = log4net.LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

        public override void OnException(ExceptionContext filterContext)
        {
            log.Error("An uncaught exception has occurred", filterContext.Exception);
            if (filterContext.HttpContext.User.Identity.IsAuthenticated)
            {
                log.ErrorFormat("The currently logged in user is '{0}'", filterContext.HttpContext.User.Identity.Name);
            }
            base.OnException(filterContext);
        }
    }
```

Choosing where to write your logs is achieved by adding a few lines to your `(App|Web).config` file. You can define any number of `appenders` and log4net has support for many different places to output your logs. In the example below I'm writing the log output to a file which changes daily and to the debug stream - great when developing because it appears in my IDE.

```
  <configSections>
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler, log4net" requirePermission="false" />
  </configSections>
  <log4net>
    <appender name="Debug" type="log4net.Appender.DebugAppender">
      <layout type="log4net.Layout.PatternLayout">
        <conversionPattern value="%date %level %thread %logger - %message%newline" />
      </layout>
    </appender>
    <appender name="RollingFile" type="log4net.Appender.RollingFileAppender">
      <file value="LogFiles\file.log" />
      <appendToFile value="true" />
      <layout type="log4net.Layout.PatternLayout">
        <conversionPattern value="%date %level %thread %logger - %message%newline" />
      </layout>
    </appender>
    <root>
      <level value="DEBUG" />
      <appender-ref ref="RollingFile" />
      <appender-ref ref="Debug" />
    </root>
  </log4net>
```

Changing the level of logging is as simple as changing the value of the level tag from `DEBUG` to `WARN`. At the moment I'm using `DEBUG` for development and testing and using `INFO` or `WARN` in the production environment. The advantage of having this in a config file is that you can change the logging output and level while the application is running.

Obviously this post is only an introduction. Great documentation and more examples can be found at [the official log4net website](http://logging.apache.org/log4net/).