Using Randori for ActionScript requires IntelliJ IDEA, the Randori Plugin and SDK. The goal of this lesson is to get the Randori environment properly configured in IntelliJ IDEA.

### 1) Download the Randori SDK from https://github.com/RandoriAS/randori-sdk/archive/v0.2.1.zip and save it on your drive in a known location. Unzip this SDK as you will need it in a later step. 

This is the SDK that provide the Randori framework code as well as several core library definitions.

2) Open up IntelliJ, and go to File -> Settings... -> Plugins.

In the right part of the screen, click Browse repositories... and enter the search term 'Randori' into the textbox in the right upper corner of the screen. This will, probably, yield just one result called something along the lines of 'Randori compiler vx.x.x', double click this entry and download the plugin, after that press close and then apply.

Now you'll need to restart IntelliJ, go ahead and do so.

3) Next, you will need to extract the contents of the RandoriSDK-*.zip to a known location on your drive.

4) Once the SDK is unzipped, return to IntelliJ. Click Create New Project.

![Create New Project](http://randoriframework.com/wp-content/uploads/2013/03/lesson1-1.png)

5) Setting up the project for the first time will require multiple steps. To save yourself some debugging, do not press Finish until we tell you. On the left side of the screen, choose Randori Module as the project type. On the right side, name your project SimpleTest and choose a location on your drive to store the associated files and assets.

![Select Randori Module](http://randoriframework.com/wp-content/uploads/2013/03/lesson1-2.png)

6) Next, under Project SDK on the right side of the screen, click New… and browse to the location where the RandoriSDK was extracted on your drive. It’s now safe to click Finish.

![Select Randori SDK](http://randoriframework.com/wp-content/uploads/2013/03/lesson1-3.png)

7) You should now see the SDK listed underneath the project name and location.

You now have a new project that has been properly configured with the Randori SDK. In the next section you will begin adding HTML, CSS and ActionScript to the project.