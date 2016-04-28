---
title: Create Aliases in Command Prompt
layout: post
---
There are multiple ways of creating aliases or macros in Command Prompt. There is a built in DOSKEY command that creates aliases but you may want to make the aliases persist through sessions. That is where batch files come in. If you want to make a project environment with project specific aliases then I recommend using Method 2: DOSKEY. If you have generic tools that you want globally available then Method 1: Batch files is the best option.

1. Batch files
2. DOSKEY

### Method 1: [Batch files](https://technet.microsoft.com/en-us/library/bb490869.aspx)

<pre>
Example 1: foobar.bat
@echo off
call foo.bat
call bar.bat

Example 2: np.bat
@echo off
notepad.exe %*
</pre>

One way of creating an alias is to create a batch file named after the alias (*macroname*.bat), and then call the commands you want executed in the script. The benefits to this approach are that the commands are easy to make global across your system without needing a custom command prompt shortcut. This approach lends itself best for general tools you want available at all times.

#### Where to save the batch files?

Where you save the batch file is important because the Command Prompt only has references to certain directories that are defined in your PATH variable. The easiest option is to save your batch file under the `System32` directory because this folder is defined in the PATH variable by default. If you want a cleaner separation of concerns, you can add a custom directory to your PATH variable by going to `Control Panel > System and Security > System > Advanced system settings > Advanced tab > Environment Variables...`.

<div class="row">
	<div class="col-md-6">
		<p><img class="img-responsive" src="/assets/systempropertiesdialogwindow.png" alt="System Properties Dialog" /></p>
	</div>
	<div class="col-md-6">
		<p><img class="img-responsive" src="/assets/environmentvariablesdialogwindow.png" alt="Environment Variables Dialog" /></p>
	</div>
</div>

From here, you have the option of either modifying the *user variables* to make the aliases only affect the current user, or modify the *system variables* to make the aliases globally available to all users. Modify the appropriate PATH variable and add the directory where your alias batch files live. For example, I keep my batch files under the directory D:\Tools.

![Edit environment variable](/assets/editenvironmentvariabledialogwindow.png){: .img-responsive }

#### Passing arguments

One important feature to have is the ability to pass through arguments from the alias to the calling program. Batch files have this ability built in and can access arguments via the percent syntax `%1-%9` indexing starting at one or `%*` for all variables. In a simple alias you will most likely want to just pass all variables directly through using the `%*` reference.

#### Why @echo off?

Adding `@echo off` to the beginning of the batch script is to suppress the [command-echoing feature](https://technet.microsoft.com/en-us/library/bb490897.aspx), which prints each command to standard output. The @ symbol will suppress echoing of the echo command.

#### Why use CALL command?

The CALL program allows you to execute a *batch script* from another batch file without causing the parent script to stop. The default behavior when calling a batch file from another batch file is to exit after execution. For example, in the parent batch file `foobar.bat`, only foo.bat would be executed. CALL is **not** necessary to execute an executable, only other batch files. <http://stackoverflow.com/questions/11638705/why-does-calling-a-nested-batch-file-without-prepending-call-to-the-line-exit>

	foobar.bat
	---
	foo.bat
	bar.bat

#### Suppressing the "Terminate Batch Job" prompt

Programs executed from a batch file will show the prompt "Terminate batch job (Y/N)?". The prompt is an annoyance because even if you pass [N]o the program terminates. One way you can allow your batch file to close is to pass the [NUL device](http://ss64.com/nt/nul.html) into the program call. <http://superuser.com/questions/35698/how-to-supress-terminate-batch-job-y-n-confirmation>

	deploy.bat
	---
	@echo off
	call bundle exec jekyll serve --watch --drafts < NUL

Advantage: 

* Does not need a custom command prompt shortcut
* Aliases can be made global or per user
* You have full batch scripting capabilities

Disadvantage:

* Cannot make environment specific aliases
* Sharing aliases requires copying multiple files


### Method 2: [DOSKEY](https://technet.microsoft.com/en-us/library/bb490894.aspx)

<pre>
Syntax:
DOSKEY <i>macroname</i>=<i>commands</i>

Examples:
doskey np=notepad $*
doskey mkfile=set /p z=$L NUL $G $1
</pre>

The DOSKEY program allows you to define commands to a simple macro name. The aliases created with DOSKEY only live in the current session. This lends itself as a great option to use if you want project specific aliases/macros because you can setup custom macros per command prompt session. The aliases can be persisted but require either a special command prompt shortcut or messing with your registry files.

#### Persist using shortcut

The cleanest way of persisting aliases across sessions is to create a custom command prompt shortcut that executes a batch file via the `/k` option that initializes the environment with all of your custom aliases and macros. You can also set the command prompts title and colors if needed.

	cmd.exe /k customEnvironment.bat

	customEnvironment.bat
	---
	title "My Custom Environment"
	doskey np=notepad $*


#### Persist using Command Processor's [AutoRun registry variable](http://stackoverflow.com/a/21040825)

Another option to persist aliases is to modify the command processor's `AutoRun` registry variable to automatically execute a script when Command Prompt is opened.

1. Run `regedit` and navigate to `HKEY_CURRENT_USER > Software > Microsoft > Command Processor`
2. Right-Click > New > String Value
3. Enter the variable name `AutoRun` and the value should point to your environment initialization batch file

![Command Processor's AutoRun registry value](/assets/commandprocessorautorunregistry.png)

#### DOSKEY syntax

The DOSKEY program has special syntax that mirrors batch file syntax. For example, parameters are referenced using the dollar sign rather than percents. Likewise, the redirection commands also have equivalent counterparts. The table below summarizes the syntax equivalences.

<table class="table table-striped table-bordered table-condensed">
	<thead>
		<tr>
			<th>Description</th>
			<th><a href="http://ss64.com/nt/syntax-args.html">Batch Syntax</a></th>
			<th><a href="https://technet.microsoft.com/en-us/library/bb490894.aspx#ECAA">DOSKEY Syntax</a></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>output redirection to file</td>
			<td>></td>
			<td>$G</td>
		</tr>
		<tr>
			<td>output redirection and append to file</td>
			<td>>></td>
			<td>$G$G</td>
		</tr>
		<tr>
			<td>input from file</td>
			<td><</td>
			<td>$L</td>
		</tr>
		<tr>
			<td>pipe output as input to another command</td>
			<td>|</td>
			<td>$B</td>
		</tr>
		<tr>
			<td>multiple commands</td>
			<td>&amp;</td>
			<td>$T</td>
		</tr>
		<tr>
			<td>parameters</td>
			<td>%1-9, %*</td>
			<td>$1-9, $*</td>
		</tr>
	</tbody>
</table>

Advantages:

* Ability to initialize environment per project
* Aliases can be shared through one file

Disadvantages:

* Requires registry to make aliases global
