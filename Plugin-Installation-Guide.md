## System requirements
The plugin only works in the Ultimate edition of IntelliJ IDEA, the community edition is _not_ supported at this moment.

## Step 1: Download SDK
Download the latest version of the SDK:
[Randori SDK](https://github.com/RandoriAS/randori-sdk/archive/v0.2.1.zip)

After download, unzip the SDK to a directory of your liking.

## Step 2: Plugin installation
Open up IntelliJ, and go to _File -> Settings... -> Plugins_.  
In the right part of the screen, click _Browse repositories..._ and enter the search term 'Randori' into the textbox in the right upper corner of the screen.
This will, probably, yield just one result called something along the lines of 'Randori compiler vx.x.x',
double click this entry and download the plugin, after that press _close_ and then _apply_.  
Now you'll need to restart IntelliJ, go ahead and do so.

That's all for the plugin installation!

## Step 3: Creating a project
Next, create a new project and select _Randori module_, give it a name and click on the _New..._ button next to the SDK listcontrol and select the directory where you unzipped the SDK to.

Click _Finish_.

That's it, you've created your first Randori project.

Next, check out some of the demo applications in this repository:
[Randori Demo App Repository](https://github.com/RandoriAS/randori-demos-bundle)

This can serve as a reference for your own ventures at first.

To add a class or interface there are _Randori Class_ and _Randori Interface_ options underneath the _New_ menu item. (Right-click on a directory for that)

To compile your project, use the key combo **CTRL+SHIFT+ALT+R**. Verify that the compilation worked by checking the existence of a folder called 'generated' in the root of your project.

For now, we refer you to the demo application to see how the HTML, CSS, etc works. If you have specific questions you can ask them on the [Randori Forum](http://randoriframework.com/forum/).

## Step 4: Running the app
Only one thing is left to do now: You'll need to create a run configuration for the Randori app. To do so,
select _Run -> Edit configurations..._
In the resulting dialog, click on the green '+' in the top left corner and select _Randori Application_.

First, give it a name and then select your Randori module from the drop downlist.

Now the remaining text boxes for _Index_ and _Webroot_ have these purposes:
By default the Randori plugin assumes that the webroot for your application is equal to your project root.
So, it assumes there is a file called 'index.html' directly in your root. Now, should you put everything underneath the 'src' folder, you'll have to enter '_src/index.html_' in the _Index_ textbox.

Also by default, the plugin runs its own embedded webserver that uses your project root as its webroot,
so normally, when you run the application, a browser will open with a URL like this: _http://localhost:14237/index.html_
(Where the port number is any available port at that moment).

Now, if you have your own webserver already running locally, you can enter its root URL there. So if you have Apache (or IIS or whatever) running on the regular port 80 and your app is hosted in a subdirectory, you add this in the _webroot_ box:
http://localhost/mysubdirectory.

After that you can click OK and your run target will have been created. Check this by using the combo **SHIFT-F10**: A browser should open pointing to the localhost URL just described.

### In closing
And that's it! If you find any bugs, or have ideas for new functionality please get in touch with us by using the issues list here:
[Randori Plugin Issue List](https://github.com/RandoriAS/randori-plugin-intellij/issues)

For ideas and bugs in the Randori framework itself, please use this list:
[Randori Framework Issue List](https://github.com/RandoriAS/randori-framework/issues)