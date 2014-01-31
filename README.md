#Tishadow installation guide and usage tips & tricks 

This is a guide on how to install [TiShadow](https://github.com/dbankier/TiShadow) and various tips and tricks that help in TiShadow usage. 
Initial idea was to have a point of reference for myself. But it grew and grew to the point it might be quite useful to more people. If you would like to add anything to this document please feel free. If you have a question or find something difficult to follow - please let me know. 

## <a name='TOC'>Table of Contents</a>
1. [Super quick deployment](#quick-deployment)
1. [Full install](#full-install)
	1. [Install Node.js and npm](#node-install)
	1. [Install TiShadow](#tishadow-install)
	1. Setup with Titanium studio
	1. [Setup TiShadow app project in terminal (CLI)](#tishadowapp-setup)
	1. [Build TiShadow app for the platform of your choice](#build-tishadow-app)
	1. [Start TiShadow app and TiShadow server](#tishadow-run)
1. Using with single emulator/simulator
1. Using with multiple emulators/simulators
1. Using Controller
1. Automation with Grunt/Supervisor
1. [Troubleshooting](#troubleshooting)
1. [Resources](#resources)



##<a name='quick-deployment'>Super quick deployment</a>

Super quick deployment procedure for the itchy fingers aka tl;dr (requires at least TiShadow v2.4.3). If you are not interested in the full process but just want to quickly check it out, please follow instructions below:

If you already **have Node.js** and npm installed on your system you are minted. Follow steps below.

If you **do not have Node.js**, you have to install it (see the section [Install Node.js and npm](#node-install)).

###1. Install TiShadow. 
In terminal window type:
		
	npm install -g tishadow
		
or, if your system complains about access rights, type
	
	sudo npm install -g tishadow
	
You will be asked for your password, then TiShadow should be installed on your machine.
To verify the installation type: 
	
	tishadow -v

Your version should be at least: `2.4.3`

###2. Use express mode to checkout your app
Inside terminal, navigate to Titanium project folder you would like to test, then type:
	
**for Android emulator**
	
	titanium build -p android -T emulator --shadow 
	
**for iOS simulator**
	
	titanium build -p ios -T simulator --shadow 

This should compile your app, run the TiShadow server in the background, create the appified version of your app and push it to the emulator.

###3. Edit source code.
Make changes in your source code and save them. Go to the emulator and see the app changing in front of your eyes. Enjoy.

**End of tl;dr**

##<a name='full-install'>Full install</a>

###<a name='node-install'>Install Node.js and npm</a>

**OSX and Ubuntu**
Go to [http://nodejs.org/](http://nodejs.org/)
Click big green 'Install' button, download package. 

**OSX**
Install it by double clicking.

**Ubuntu 12.04**
Extract it to a folder somewhere. Open that folder in the terminal and type:
		
	sudo apt-get update
	sudo apt-get install -y python-software-properties python g++ make

These two above will update packages info for your system and install necessary packages for node install.
Now compile and install NodeJS in your home folder. 
		
	./configure --prefix=$HOME
	make
	make install

**OSX and Ubuntu**
Once the installation finishes, open terminal window and verify node installation by typing `node -v` (you should see node number like `v0.10.21`), type `npm -v` (again you should see the numer like `1.3.11`). 

If both commands gave you numbers everything is ok, go to point 3. If not, something went wrong. Check the [Troubleshooting](#troubleshooting) section of this document.


###<a name='tishadow-install'>Install TiShadow</a>

**OSX and Ubuntu**
using npm 

	sudo npm install -g tishadow

From git *(not tested on Ubuntu)*

	sudo npm install -g git+ssh://git@github.com/dbankier/TiShadow.git

Test the install

	tishadow -v


###Setup with Titanium studio

Disclaimer - my Ti Studio 3.2.0 did not allow me to build the TiShadow app, so this part is theoretical - YMMV
So before I write something sensible here - you might want to check second part of this tutorial:
[TiShadow – Getting Started pt.2](http://www.stephenfeather.com/blog/tishadow-getting-started-part-2/)

**Awaiting content**

###<a name='tishadowapp-setup'>Setup TiShadow app project in terminal (CLI)</a>

Create a project folder for your TiShadow app (like a regular Titanium project) using your favourite file manager or terminal.
Mine folder is in:	`$HOME/Titanium_Studio_Workspace/tishadowapp`

Navigate to your newly created folder (e.g `cd $HOME/Titanium_Studio_Workspace/tishadowapp)`
Inside that folder type:

	tishadow app -d ./ 

The `-d ./` means create the app project in the current folder.

You should be asked about the app id (just like when you create your normal Ti project). Type something like this:

	Enter app id [com.yydigital.tishadowapp]: com.test.tishadowapp

After pressing enter/return you should see:
	
	[INFO] Creating new app...

And once the project files have been created you should be back at the prompt. 

*If you see red error message like this:*
	
	[ERROR] You really don't want to write to the current directory.

*That means you probably forgot to add `-d ./` or `-d nameofthedirectory` to your `tishadow app` command.*

To verify that everything went fine check the contents of the folder either in you file manager or typing `ls`. You should see folder structure similiar to this:

	Resources	manifest	modules		platform	tiapp.xml

###<a name='build-tishadow-app'>Build TiShadow app for the platform of your choice</a>

Open terminal window if it is not already opened. Navigate to the folder with your TiShadow project (e.g. `$HOME/Titanium_Studio_Workspace/tishadowapp`) and using `titanium` command line tool compile it.  
**OSX and Ubuntu** type (instead of `titanium` you can also use short version `ti`: 

**IOS**

	titanium build -p ios -T simulator
	
Optionally you might also add `-Y iphone` or `-Y ipad` to target specific simulator. By default it should build to iPhone simulator.

**Android**

	titanium build -p android -T emulator
	
If you have more than one Android emulator configured you might add `-C nameOfYourEmulator`
If you are stuck at `[INFO]  Waiting for emulator to become ready` you might try [manual method](#emu-problems) from the [Troubleshooting](#troubleshooting) section.

If you would like to know more about building options type `titanium build -h` in terminal. 

The above should compile and build your TiShadow app and put it on the emulator of your choice.


##<a name='tishadow-run'>Start TiShadow app and TiShadow server</a>

**Start TiShadow server**
This could not be easier. In the terminal window type:

	tishadow server

You should be greeted with something like `[DEBUG] TiShadow server started. Go to http://localhost:3000`. We will talk about debug Controller in a moment. 
**Start TiShadow app**
TiShadow server is running, and presuming you have [successfully build](#build-tishadow-app) your TiShadow app and deployed it to the emulator of your choice, all you need is to touch the TiShadow app icon to start it. 

The app will ask you about the connection to the server.
**IOS**
You can type your computer IP (e.g. `192.168.1.10` or localhost address `127.0.0.1`), IOS simulator should work fine with both. 

**Android**
You need to give it a real IP address (e.g. `192.168.1.10`), otherwise the app will not see the server. 

**Both**
Once you provide the app with the server address and click `connect`, the app should connect to the server and on the bottom of the screen there should be `connected` message.
To be absolutely sure, switch to the terminal window with the TiShadow server running, and you should see info message:

	[INFO] [iphone, 7.0.3, xx.xx.xx.xx] Connected
		
Where `xx.xx.xx.xx` will be your emulator/simulator IP address. 

Well, now you are ready to push your test app to the TiShadow app. 

##Using with single emulator
**Awaiting content**

##Using with multiple emulators
**Awaiting content**
 
##Using Controller
**Awaiting content**

##Automation with Grunt/Supervisor
**Awaiting content**

##<a name='troubleshooting'>Troubleshooting</a>

*Ubuntu 12.04 Node install problem*
If 'make install' fails with an error like this: 

	OSError: [Errno 13] Permission denied: '/usr/local/bin/node'

use:

	./configure --prefix=$HOME
or
	sudo make install

**TiShadow Ubuntu install problem**
TiShadow was installed properly by npm but typing it in terminal shows only:
	
	-bash: tishadow: command not found

Check in your `$HOME/local/bin`, `$HOME/bin` or in `/usr/local/bin` if there is a link to tishadow module.
If not, locate your tishadow install files (usually in `/usr/local/lib/node_modules/tishadow/cli/tishadow` or ...)

and create a link 

	ln /usr/local/lib/node_modules/tishadow/cli/tishadow  /usr/local/bin/tishadow
	
<a name='emu-problems'>**Emulator problems**</a>
If you are not able to put your compiled TiShadow app on the emulator because the emulator starts too slowly and you see the following message:
	[INFO]  Waiting for emulator to become ready
	[ERROR] Emulator failed to start in a timely manner

	The current timeout is set to 120000 ms
	You can increase this timeout by running: titanium config android.emulatorStartTimeout <timeout ms>

If you have not already changed the value of `android.emulatorStartTimeout` please do it, e.g
    
    titanium config android.emulatorStartTimeout 300000

The line above should change the waiting time to 5 minutes

**If the problem persists, and is with Android emulator try:**

Go to your android sdk folder `tools/` subfolder and type: 

	android avd
	
This should launch Android Virtual Device Manager (fancy name for emulator management). Start (create/configure) your emulator and wait until it is ready.

In a separate terminal window compile your TiShadow app for Android (`-b` means 'build only', without deploying it to the emulator)
	
	titanium build -p android -b

If you have only one emulator running, type:

	adb install /pathtoyourapp/build/android/bin/yourappname.apk

If you have more emulators running at once check for their names. Navigate to the android sdk folder `platform-tools` subfolder and type:

	adb devices
	
That should give you a list of currently connected Android devices/emulators then use the name of the emulator you would like to target

	adb -s emulator-5554 install /pathtoyourapp/build/android/bin/yourappname.apk

Do not close emulator. Start TiShadow server

	tishadow server
	
Go to the emulator and start your just installed TiShadow app.

**adb problems**

If `adb` cannot find devices, or does not install an app on the emulator spitting out error message:

	error: device not found
	- waiting for device -

and the emulator *is* running, probably `adb` daemon died (remember kids a daemon is for life, not just for christmas). Try these two commands with a couple of seconds between them:

	adb kill-server
	adb start-server
	
If the resurrection was successful you should see something like the following message:

	* daemon not running. starting it now on port 5037 *
	* daemon started successfully *

##<a name='resources'>Resources</a> in no particular order.
[Titanium Appcelerator](http://www.appcelerator.com/) great mobile development platform.

[TiShadow](https://github.com/dbankier/TiShadow) Github page. You guys are stars! 

[Node.js](http://nodejs.org/)


[Installing Node.js via package manager](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint)

[Installing node.js from binary on Ubuntu](http://anirudhaj.wordpress.com/2013/02/03/installing-node-js-from-binary-on-ubuntu-precise/)

[Automatic TiShadow Installs](https://gist.github.com/kwhinnery/5565515)

[grunt-tishadow](https://npmjs.org/package/grunt-tishadow)

Stephen Feather [TiShadow – Getting Started pt.1](http://www.stephenfeather.com/blog/tishadow-getting-started-part-1/)
[TiShadow – Getting Started pt.2](http://www.stephenfeather.com/blog/tishadow-getting-started-part-2/)

[Titanium API Documentation](http://docs.appcelerator.com/titanium/3.0/#!/api) mountains of knowledge.



[back to top](#TOC)
