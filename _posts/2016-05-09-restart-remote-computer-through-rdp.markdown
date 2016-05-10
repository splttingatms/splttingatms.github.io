---
title: Restart a Remote Computer through RDP
layout: post
---
I noticed Remote Desktop Protocol (RDP) replaces the usual power options of "Shutdown" and "Restart" with "Disconnect" when remoting into a computer. As you can see below, the power options under the start menu and through Window's [Quick Link Menu](http://windows.microsoft.com/en-us/windows-10/keyboard-shortcuts) only show the "Disconnect" option.

![Start Menu Power Option](/assets/restart-remote-computer-through-rdp/startmenupoweroptions.png){: .img-responsive}

![Quick Link Menu Power Option](/assets/restart-remote-computer-through-rdp/quicklinkmenupoweroptions.png){: .img-responsive}

You are still able to shutdown or reboot the remote computer but you will need to do so through the [SHUTDOWN Program](https://technet.microsoft.com/en-us/library/bb491003.aspx). The program allows you to either shutdown or reboot a computer through the command line option, but you can make a desktop shortcut to make a really convenient button available in the taskbar.

	Syntax (not complete):
	shutdown [{-s | -r }] [-f] [-t xx]
	-s: Shutdown the local computer
	-r: Reboot after shutdown
	-f: Forces running applications to close
	-t xx: Sets the timer for system shutdown in xx seconds. The default is 20 seconds.

	Examples:
	shutdown -s -t 10
	Shutdown the local computer in 10 seconds, but wait for applications to close.

	shutdown -r -f -t 00
	Reboot the local computer immediatly, forcing running applications to close.

Follow the steps below to create a shortcut with a custom icon and place it into your taskbar.

1. Right-click on the Desktop > New > Shortcut to create a new shortcut.
![Create New Shortcut Menu](/assets/restart-remote-computer-through-rdp/newshortcutmenuoption.png){: .img-responsive}

2. Enter the shutdown command with the parameters you want.
![Create New Shortcut Location Dialog](/assets/restart-remote-computer-through-rdp/createshortcutrebootexample.png){: .img-responsive}

3. Give the shortcut a friendly name.
![Create New Shortcut Name Dialog](/assets/restart-remote-computer-through-rdp/createshortcutrebootexamplename.png){: .img-responsive}

4. Now that your shortcut is created, you can give it a unique icon. Right-click the shortcut > Properties > Change icon.
![Shortcut Properties](/assets/restart-remote-computer-through-rdp/rebootshortcutproperties.png){: .img-responsive}

5. Dismiss the warning dialog explaining that the shortcut program does not have built-in icons. You are instead going to select one of the system icons.
![Change Shortcut Icon Warning Dialog](/assets/restart-remote-computer-through-rdp/rebootshortcutchangeicondialog.png){: .img-responsive}

6. Select one of the built-in system icons.
![Change Shortcut Icon Dialog](/assets/restart-remote-computer-through-rdp/rebootshortcutchangeicondialogselected.png){: .img-responsive}

7. Your shortcut should update with the selected icon.
![Final Reboot Shortcut](/assets/restart-remote-computer-through-rdp/rebootshortcutfinal.png){: .img-responsive}

8.	Drag your newly created shortcut to your taskbar to make it easily accessible. Tip: You can select taskbar shortcuts using the keyboard shortcuts <kbd>Win + 1-9</kbd>.
![Reboot Shortcut in Taskbar](/assets/restart-remote-computer-through-rdp/rebootshortcutintaskbar.png){: .img-responsive}