---
created: 2025-12-12 06:29:01
tags:
  - note
  - windows
  - config
  - browser
  - tip
  - plugin
---
# THIS SHOULD REALLY NOT BE USED

# Content

1. [[#Fonts|Fonts]]
	1. [[#Fonts#Installation|Installation]]
	2. [[#Fonts#Fonts that I use|Fonts that I use]]
2. [[#Theming|Theming]]
	1. [[#Theming#[Activate Windows](https://msguides.com/windows-10)|Activating Windows]]
	2. [[#Theming#Patching|Patching]]
	3. [[#Theming#Windows Theme|Windows Theme]]
	4. [[#Theming#Cursor|Cursor]]
	5. [[#Theming#System Font|System Font]]
	6. [[#Theming#Icons|Icons]]
	7. [[#Theming#OldNewExplorer|OldNewExplorer]]
3. [[#OS|OS]]
	1. [[#OS#Control Panel|Control Panel]]
	2. [[#OS#Settings|Settings]]
	3. [[#OS#Task Manager:|Task Manager:]]
	4. [[#OS#Taskbar -> Right Click:|Taskbar -> Right Click:]]
4. [[#System Image|System Image]]
5. [[#Applications|Applications]]
	1. [[#Applications#OBS|OBS]]
	2. [[#Applications#Visual Studio|Visual Studio]]
	3. [[#Applications#Empty Standby List|Empty Standby List]]
	4. [[#Applications#ShareX|ShareX]]
	5. [[#Applications#Unity|Unity]]
6. [[#Reboot|Reboot]]

# Fonts
## Installation
- ### When installing a font, install a variable weight and all the needed static weights.
## Fonts that I use
- ### [Cascadia](https://github.com/microsoft/cascadia-code)
- ### [Comfortaa](https://fonts.google.com/specimen/Comfortaa)

<br>

# Theming
## [Activate Windows](https://msguides.com/windows-10)
### Run CMD as an Admin and enter the following commands:
	- `slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX`
	- `slmgr /skms kms8.msguides.com`
	- `slmgr /ato`
## Patching
- ### [Download the UltraUXThemePatcher](https://mhoefs.eu/software_uxtheme.php?lang=en)
- ### Run the installation
- ### Reboot
## Windows Theme
- ### [[Windows 10 Themes#[Nord](https //github.com/niivu/Windows-10-themes/tree/main/Nord)|Download the theme]]
- ### Copy all the folder contents to: `C:\Windows\Resources\Themes`
- ### [[#Settings|Select the theme]]
## Cursor
- ### [Download the Minimalistic Nord Cursor](https://www.deviantart.com/skyeo84/art/Minimalistic-V3-nord-cursor-909562097)
- ### Open the Outlined folder and install the install.inf file
## System Font
- ### Download the [Windows Font Changer](https://github.com/nnra6864/WindowsFontChanger)
- ### Run it
- ### Select the font of your choice
- ### Do not Reboot
## Icons
- ### [Download the Nord Icon Pack](https://www.deviantart.com/niivu/art/Nord-Icon-Theme-837265260)
- ### Extract the pack and remove the .remove extension from the file in the Nord 7TSP Folder
- ### [Download 7TSP](https://www.deviantart.com/devillnside/art/7TSP-GUI-2019-Edition-804769422)
- ### Run it
- ### Add a Custom Pack and select the file that you removed the extension from(should end up in .7z now)
- ### Reboot
## OldNewExplorer
- ### [Download the OldNewExplorer](https://www.majorgeeks.com/files/details/oldnewexplorer.html)
- ### Run it
- ### Use command bar instead of Ribbon ✔
- ### Uncheck all of the sub-options that got checked ❌
- ### Install

<br>

# OS
## Control Panel
- ### View by: Large Icons
- ### Mouse
	- #### Buttons
		- ##### Double Click Speed -> Fast
	- #### Pointer Options
		- ##### Pointer Speed -> 6(Slow+6xRight Arrow)
		- ##### Enhance Pointer Precision ❌
- ### Ease Of Access Center
	- #### Always read this section aloud ❌
	- #### Always scan this section ❌
	- #### Make the keyboard easier to use
		- ##### Set up Sticky Keys
			- ###### Turn on Sticky Keys when SHIFT is pressed five times ❌
			- ###### Display a warning message when turning a setting on ❌
			- ###### Make a sound when turning a setting on or off ❌
			- ###### Lock modifier keys when pressed twice in a row ❌
			- ###### Turn off Sticky Keys when two keys are pressed at once ❌
			- ###### Play a sound when modifier keys are pressed ❌
			- ###### Display the Sticky Keys icon on the task bar ❌
		- ##### Turn on Toggle Keys by holding down the NUM LOCK key for 5 seconds ❌
- ### User Accounts
	- #### Change User Account Control settings
		- ##### Never Notify
- ### File Explorer Options
	- #### General
		- ##### Open File Explorer to: This PC
		- ##### Show recently used files in Quick access ❌
		- ##### Show frequently used folders in Quick access ❌
	- #### View
		- ##### Display the full path in the title bar ✔
		- ##### Hidden files and folders -> Show hidden files, folders and drives ✔
		- ##### Hide empty drives ❌
		- ##### Hide extensions for known file types ❌
		- ##### Hide protected operation system files (Recommended) ❌
		- ##### Restore previous folder windows at logon ✔
## Settings
- ### System
	- #### Notifications & actions
		- ##### Get notifications from apps and other senders ❌
		- ##### Show me the Windows welcome experience after updates and occasionally when I sign in to highlight what's new and suggested ❌
		- ##### Suggest ways I can finish setting up my device to get the most out of Windows ❌
		- ##### Get tips, tricks, and suggestions as you use Windows ❌
	- #### Power & sleep
		- ##### Screen
			- ###### When plugged in, turn off after -> Never
		- ##### Sleep
			- ###### When plugged in, PC goes to sleet after -> Never
	- #### Clipboard
		- ##### Clipboard history ✔
- ### Devices
	- #### Bluetooth & other devices
		- ##### Bluetooth ❌
	- #### Typing
		- ##### Autocorrect misspelled word ❌
		- ##### Add a period after I double-tap the Spacebar ❌
- ### Personalization
	- #### Lock screen
		- ##### Background -> Picture
		- ##### Choose the picture of your choice
		- ##### Choose which apps show quick status on the lock screen -> Calendar, Weather
	- #### Themes
		- ##### [[##Windows Theme|Install a theme]] and select it
		- ##### Change the Mouse cursor by using the button above
		- ##### Change the Background by using the Background button
		- ##### Save the theme by using the Save theme button
	- #### Start
		- ##### Show suggestions occasionally in Start ❌
		- ##### Unpin all the apps from Start
	- #### Taskbar Settings
		- ##### Use Small Taskbar Buttons ✔
		- ##### Show News And Interests On The Taskbar ❌
		- ##### Taskbar location on screen -> Top
- ### Accounts
	- #### Sign-in options
		- ##### Restart apps ✔
- ### Time & Language
	- #### Region
		- ##### Change data formats
			- ###### Calendar -> Gregorian Calendar
			- ###### First day of week -> Monday
			- ###### Short date -> 05-Apr-17
			- ###### Long date -> Wednesday, 5 April, 2017
			- ###### Short time -> 09:40
			- ###### Long time -> 09:40:07
- ### Gaming
	- #### Xbox Game Bar ❌
## Task Manager:
- ### Startup
	- #### Microsoft Edge ❌
	- #### Microsoft One Drive ❌
## Taskbar -> Right Click:
- ### Search
	- #### Show Search Icon ✔
	- #### Show Search Highlights ❌
- ### News and Interests
	- #### Turn off ✔
- ### Show Touch Keyboard Button ✔
- ### [Taskbar X](https://chrisandriessen.nl/taskbarx)
	- #### Open the configurator
		- ##### Style -> Blur

<br>

# System Image
- ## Open Control Panel
- ## Backup and Restore (Windows 7)
- ## Create a system image

<br>

# Reboot