## System requirements
The plugin only works in the Pro or Ultimate editions of IntelliJ IDEA, so the community version is not supported at this moment.

## Step 1: download
Download the latest version of the SDK:
[Randori SDK](http://www.teotigraphix.com/labs/RandoriSDK-0.2.0.zip)

After download, unzip the SDK to a directory of your liking.

After that download the latest version of the plugin:
[Randori IntelliJ Plugin](http://www.teotigraphix.com/labs/RandoriPlugin-0.2.0.zip)

## Step 2: Installation
Open up IntelliJ, and go to File -> Settings -> Plugins.
In the right part of the screen, click 'Install plugin from disk...' and select the zip you just downloaded. After that you'll need to restart  IntelliJ.

Alright, and that's all for the plugin installation!

## Step 3: Creating a project
Next, create a new project and select 'Randori module', give it a name and click on the 'New...' button next to the SDK listcontrol and select the directory where you unzipped the SDK to.

Click 'Finish'.

That's it, you've created your first Randori project.

Next, check out some of the demo applications in this repository:
[Randori Demo App Repository](https://github.com/RandoriFrameworkAS/DemoApplications)
This can serve as a reference for your own ventures at first.

To add a class or interface there are 'Randori Class' and 'Randori Interface' options underneath the 'New' menu item. (Right-click on a directory for that)

To compile your project, use the key combo **CTRL+SHIFT+ALT+R**. Verify that the compilation worked by checking the existende of a folder called 'generated' in the root of your project.

For now, we refer you to the demo application to see how the HTML, CSS, etc works. If you have specific questions you can ask them on the [Randori Forum](http://randoriframework.com/forum/).

Only one thing is left to do now: You'll need to create a run configuration for the Randori app. To do so,
select Run -> Edit configurations...
In the resulting dialog, click on the green '+' in the top left corner and select 'Randori Application'.

First, give it a name and then select your Randori module from the drop downlist.

Now the remaining text boxes for 'Index' and 'Webroot' have these purposes:
By default the Randori plugin assumes that the webroot for your application is equal to your project root.
So, it assumes there is a file called 'index.html' directly in your root. Now, should you put everything underneath the 'src' folder, you'll have to enter 'src/index.html' in the 'Index' textbox.

Also by default, the plugin runs its own embedded webserver that uses your project root as its webroot,
so normally, when you run the application, a browser will open with a URL like this: _http://localhost:14237/index.html_
(Where the port number is any available port at that moment).

Now, if you have your own webserver already running locally, you can enter its root URL there. So if you have Apache (or IIS or whatever) running on the regular port 80 and your app is hosted in a subdirectory, you add this in the webroot box:
http://localhost/mysubdirectory.

After that you can click OK and your run target will have been created. Check this by using the combo **SHIFT-F10**: A browser should open pointing to the localhost URL just described.

And that's it! If you find any bugs, or have ideas for new functionality please get in touch with us by using the issues list here:
[Randori Plugin Issue List](https://github.com/RandoriFrameworkAS/Plugin/issues)

For ideas and bugs in the Randori framework itself, please use this list:
[Randori Framework Issue List](https://github.com/RandoriFrameworkAS/Randori/issues)