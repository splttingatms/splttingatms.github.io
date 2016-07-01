---
title: Unescape strings in Visual Studio debugger
layout: post
---

Visual Studio has a great built-in feature to display strings un-escaped while debugging! The feature is accessed via the Watch window using the [no quotes `nq` format specifier](https://msdn.microsoft.com/en-us/library/e514eeby(v=vs.100).aspx). The specifier is designed to display strings with no quotes, but missing from the documentation is the fact that it also un-escapes the entire string!

First, add the variable you would like to see un-escaped to the Watch window but append `,nq` after the variable name. This will instruct Visual Studio to display the string with no quotes and un-escaped. 

In the example below, we have two variables that contain strings with escaped characters. The watch window shows the variables as displayed with and without the specifier.

<script src="https://gist.github.com/splttingatms/d4e8b56d6a0c5b4a96d76f4cf9c8a22c.js"></script>

<figure>
	<img class="img-responsive" alt="Format Specifiers in Watch Window" src="/assets/no-quote-format-specifier/watch-window.png">
	<figcaption>Format Specifiers in Watch Window</figcaption>
</figure>