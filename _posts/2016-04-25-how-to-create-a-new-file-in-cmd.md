---
layout: post
title:  "How to Create a File in Window's Command Prompt"
---
Command prompt provides many tools for creating a new file. Each tool has its pros and cons that may be better suited for the task at hand. If you want to create empty files, see methods 1-3. If you want to create text files with specific content then skip to method 4.

1. fsutil
2. copy
3. redirection syntax
4. redirection syntax with text

### Method 1: [fsutil](https://technet.microsoft.com/en-us/library/cc788058.aspx)
<pre>
Syntax:
FSUTIL file createnew <i>filename</i> <i>bytelength</i>

Example: 
fsutil file createnew sample.txt 0
</pre>

Fsutil is a program that performs tasks that are related to the file system. One of its abilities is to create a file with a specified name and file size. The content will consist of all zeros. In ASCII, the decimal value 0 is the null (NUL) character.

The behavior of fsutil is to fail if a file with the specified filename already exists. Unfortunately there is no flag to override this behavior.

	c:\>fsutil file createnew sample.txt 0
	Error:  The file exists.

This method is perfect if you do not care about the contents of the file and need an exact file size. The downside is that fsutil [requires administrator privileges](https://technet.microsoft.com/en-us/library/cc753059.aspx) to run. Also, the inability to control the overwrite behavior makes this more of a challenge to use in a batch script; it is possible but you will need to capture the error which is more involved than the options below.

#### Use if:

* You need a specific file size
* You need to throw an error if file exists

#### Do not use if:

* You want to specify the text in the file
* You need to overwrite the file if it exists
* You do not have administrator privileges


### Method 2: [copy](https://technet.microsoft.com/en-us/library/bb490886.aspx)

<pre>
Syntax:
COPY <i>source</i> <i>destination</i>

Example:
copy /y NUL sample.txt > NUL
</pre>

Copy is a program that copies files from one location to another. An empty file can be created by cleverly specifying the special [NUL device](http://ss64.com/nt/nul.html). Since the NUL device acts as an empty file, you are essentially copying empty contents to the destination file.

Copy will prompt the user whether to overwrite the file if the destination file already exists and the copy command is being executed in the command prompt, as seen below. The prompt can be suppressed by passing the /y flag. The behavior is to not prompt if COPY is being executed from a batch file.

	c:\>copy NUL sample.txt
	Overwrite sample.txt? (Yes/No/All): y
			1 file(s) copied.

Finally, the copy command outputs whether the file was copied successfully, as seen below. This output can be suppressed by redirecting the standard output to NUL device.

	c:\>copy /y NUL sample.txt
        	1 file(s) copied.

#### Use if:

* You need an empty file (zero bytes)
* You want to force overwrite behavior
* You want to suppress success text output
* You want to copy an already existing file

#### Don't use if:

* You want to specify the text in the file


### Method 3: [redirection syntax](http://ss64.com/nt/syntax-redirection.html)

<pre>
Syntax:
<i>command</i> <b>></b> <i>filename</i>

Examples:
type NUL > sample.txt	
cd. > sample.txt
echo: 2> sample.txt
</pre>

The command redirection operator (`>`) writes the command output to a file or a device instead of the Command Prompt window. This allows you to capture the output of any program and save it to a file. Note that the redirection operator (`>` or `2>`) will overwrite any existing file without warning.

Although you can redirect any output, the examples provided will create an empty file, simply because the commands do not output anything. 

For example, TYPE is a program that prints the contents of a file. By specifying the special NUL device, you are printing nothing. Saving this to a destination file will result in a zero byte empty file. 

The same logic applies to the change directory program (CD). The program does not output anything if you specify the period, which tells CD to change directory to the current directory, essentially a no-op.

The ECHO example is a special case though. As you will see below, ECHO can actually be used to save text to a file. The problem is that ECHO appends a carriage return and a line feed character to the end of the file automatically. This is a problem if you want a completely empty file but the can be changed by choosing to redirect the STDERR error text output instead. Standout error output has the numeric handle 2 and can be redirected with the redirection syntax (`2>`). ECHO will never output any error text so saving the error output to a file will result in an empty file. 

The extra colon in the example is to [echo a blank line](https://technet.microsoft.com/en-us/library/bb490897.aspx) on the screen. Without parameters, ECHO displays the current echo setting.

	C:\>echo 2> sample.txt
	ECHO is on.

#### Use if:

* You need an empty file (zero bytes)
* You want to overwrite existing files

#### Don't use if:

* You want to specify the text in the file


### Method 4: [redirection syntax with text](http://ss64.com/nt/syntax-redirection.html)

<pre>
Syntax:
ECHO <i>message</i>

Example:
echo hello> sample.txt
</pre>

<pre>
Syntax:
SET /P <i>variable</i>=[<i>promptString</i>]

Example:
set /p z=Hello, World!< NUL > sample.txt
</pre>

There are two options if you would like to create a file with specified text. The first option is the echo program, but it will automatically add a carriage return and a line feed character. As you can see below, the size of the file is greater than the expected 13 bytes (1 byte per character in the string "Hello, World!"). 

Also note that there is no space after `message` and before the redirection operator because the echo command will interpret the space as being in the message.

	C:\>echo Hello, World!> sample.txt

	C:\>dir sample.txt
	 Volume in drive C has no label.
	 Volume Serial Number is C2E7-BD5A

	 Directory of C:\

	04/25/2016  01:52 PM                15 sample.txt
	               1 File(s)             15 bytes
	               0 Dir(s)  18,041,389,056 bytes free

You can also see the extra characters if you enable whitespace characters in Notepad++ by going to `View > Show Symbol > Show All Characters`.

![Whitespace characters in text-editor](/assets/echoextracharactersnpp.png){: .img-responsive }

If you care about outputting the exact contents and nothing more, you will need to use the second option, which utilizes the [SET program](http://ss64.com/nt/set.html). I didn't come up with this myself but I found the reference [here](http://ss64.com/nt/echo.html), [here](http://stackoverflow.com/a/1702790), and [here](https://groups.google.com/forum/#!msg/microsoft.public.win2000.cmdprompt.admin/CHS0gwjZQDA/L85IIcFcLgkJ).

The SET program displays, sets, or removes command prompt environment variables. The /P switch will prompt the user for input and set the variable equal to the input. Passing in NUL through the input redirection command will cause the variable used to remain unchanged. Since the output of the SET command was the prompt, you then save this to the output file specified in the output redirection command.

This trick will also produce a file of zero length if no prompt text is specified. The SET method is the simplest way to create text files with specific text without extra new line characters.

#### Use if:

* You need a file with specific text content
* You want control over new line characters