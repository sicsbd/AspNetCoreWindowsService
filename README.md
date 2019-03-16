# AspNetCoreWindowsService

This is a simple sample repository to show how simple it is to run .NET Core and ASP.NET Core as a Windows Service with .NET Core 1.1 and it's obsolete.

With .NET Core 2.2 you can do much better with: https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/windows-service?view=aspnetcore-2.2

I tried to keep it bare minimum in order to appreciate what is needed. You can probably modify your current .NET Core API or ASP.NET MVC Project just by copy-pasting a few lines of this code to make it run as a Windows Service!

Highlights:

1) You have to target .NET 4.6.1 framework. Since this application is intended to run as a Windows Service it makes sense since Linux or Mac don't have a Windows service so it can't be part of the .NET Core framework.

```json
    // project.json

    "frameworks": {
    "net461": { }
    },
```

2) You have to use Kestrel web server when running as a Windows Service.

```csharp
    // Program.cs

    var host = new WebHostBuilder()
        .UseKestrel()
        .UseContentRoot(contentPath)
        .UseStartup<Startup>()
        .Build();
```

3) If you are using MVC you need to set the ContentRoot directory.

```csharp
    // Program.cs

    if (isConsole)
        contentPath = Path.GetDirectoryName(Process.GetCurrentProcess().MainModule.FileName);
    else
    {
        contentPath = Directory.GetCurrentDirectory();
    }
```

4) By default it runs as a service unless it detects you are Debugging or you pass the command line parameter --console

```csharp
    // Program.cs

    if (Debugger.IsAttached || args.Contains("--console"))
    {
        isConsole = true;
    }
    else
    {
        isConsole = false;
    }
```
