---
title: Inspect Method Return Values in Visual Studio Debugger
layout: post
---

Visual Studio has a great hidden feature that allows developers to view method return values in a debugging session *without* storing the values in temporary variables. The feature is perfect for nested method calls where mutliple return values are returned in one line, as seen in Line 3 below.

<script src="https://gist.github.com/splttingatms/4edf0bc9bfb3c66dcf1def4a06289380.js"></script>

### Autos/Locals Windows
The return values are automatically added to both the Autos Window (Debug > Windows > Autos) and Locals Window (Debug > Windows > Locals).

<figure>
	<img class="img-responsive" alt="Autos Window" src="/assets/inspect-return-value/autos_window_cropped.png">
	<figcaption>Autos Window</figcaption>
</figure>

<figure>
	<img class="img-responsive" alt="Locals Window" src="/assets/inspect-return-value/locals_window_cropped.png">
	<figcaption>Locals Window</figcaption>
</figure>

### Immediate/Watch Windows
Return values are also stored in *pseudo variables* called `$ReturnValue*`, where the asterisk refers to the method call index. You can either call the variable directly under the Immediate Window (Debug > Windows > Immediate) or add them to your Watch list (Debug > Windows > Watch > Watch 1).

Note: The name of the variable changed between VS2013 and VS2015. Previously the pseudo variable without an index `$ReturnValue` to referred to the last function's return value, but in VS2015 you must specify an index.

<figure>
	<img class="img-responsive" alt="Immediate Window" src="/assets/inspect-return-value/immediate_window_cropped.png">
	<figcaption>Immediate Window</figcaption>
</figure>

<figure>
	<img class="img-responsive" alt="Watch Window" src="/assets/inspect-return-value/watch_window_cropped.png">
	<figcaption>Watch Window</figcaption>
</figure>