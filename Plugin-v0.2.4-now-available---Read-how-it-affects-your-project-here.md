Randori Plugin v0.2.4 is now available for download. 

This Plugin contains significant compiler and IDE updates. However, it also requires an SDK update. 

1. Update your plugin, then download the latest Randori SDK from https://github.com/RandoriAS/randori-sdk/archive/master.zip and save it on your drive in a known location. 

2. Unzip the SDK

3. Right click on your project in IntelliJ and choose Open Module Settings

4. Choose Platform Settings/SDKs

5. Remove the old SDK

6. Click the plus key and add a new Randori SDK and navigate to where you extracted the latest SDK. Click OK

7. Click on Project Settings/Project

8. Change the Project SDK to the new SDK you just created. Click Apply.

9. Click on Project Settings/Modules.

10. Click the Dependencies Tab

11. For the Module SDK, ensure Project SDK is selected

12. Click OK

13. The project should now compile.

Version v0.2.4 of the plugin changes the names of the Randori and Randori Guice libraries. Therefore if you were developing with an older version of the plugin, you must update your existing HTML files.

Pre-v0.2.4 they would have read:
```
<script type="text/javascript" src="generated/lib/RandoriGuice.js"></script>
<script type="text/javascript" src="generated/lib/Randori.js"></script>
```

Please update these references to read:
```
<script type="text/javascript" src="generated/lib/randori-guice-framework.js"></script>
<script type="text/javascript" src="generated/lib/randori-framework.js"></script>
```

After making these updates, your project should compile correctly and run with the latest plugin.

