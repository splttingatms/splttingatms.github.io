---
title: Removing OWIN Hosting Trace Listener
layout: post
---

Microsoft's OWIN hosting does something unexpected when starting up a self-hosted service, such as an ASP.NET Web API service. Calling `Microsoft.Owin.Hosting.WebApp.Start()` will automatically add a trace listener with the name *HostingTraceListener*. Below is some code that displays assigned trace listeners before and after starting up a service.

<script src="https://gist.github.com/splttingatms/90b240de8ab3aec9f2583275e48ec5d7.js"></script>

<figure>
	<img class="img-responsive" alt="Trace Listener Results" src="/assets/removing-owin-hosting-trace-listener/trace-listener-results.png">
	<figcaption>Trace Listener Results</figcaption>
</figure>

You can actually see where Katana adds the trace listener in the source code of the [HostingEngine class](http://katanaproject.codeplex.com/SourceControl/latest#src/Microsoft.Owin.Hosting/Engine/HostingEngine.cs). When `Start(StartContext context)` executes, it calls `EnableTracing(context)` which creates the trace listener and adds it to the configuration.

```c#
private void EnableTracing(StartContext context)
{
    var textListener = new TextWriterTraceListener(context.TraceOutput, "HostingTraceListener");
    Trace.Listeners.Add(textListener);
    ...
```

In most situations the extra listener would be fine, but you will start seeing repeated logs if you added a console trace listener manually. This is because the HostingTraceListener outputs to console by default. The code below produces double logs unexpectedly because of the sneaky Owin trace listener.

<script src="https://gist.github.com/splttingatms/23677bbc4dc8a94d1fb162e994009cdc.js"></script>

<figure>
	<img class="img-responsive" alt="Double Logs from hidden trace listener" src="/assets/removing-owin-hosting-trace-listener/double-logs.png">
	<figcaption>Double Logs from hidden trace listener</figcaption>
</figure>

### How to remove HostingTraceListener

There is a way to remove the trace listener though. Simply call Trace.Listeners.Remove and pass in the name of the OWIN listener as below. Make sure to call remove *after* starting the service because the listener won't exist beforehand.

```c#
System.Diagnostics.Trace.Listeners.Remove("HostingTraceListener");
```

