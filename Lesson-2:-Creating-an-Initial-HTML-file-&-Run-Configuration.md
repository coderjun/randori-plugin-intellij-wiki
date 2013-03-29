In this lesson, you will create a basic HTML file to serve as the root of your project. You will also create a Run Configuration which will allow the Randori Plugin to launch a development web server on your behalf. 
This latter step is not strictly necessary, but rather a convenience allowing you to quickly debug multiple projects. If you have a web server installed that you would rather use, you simply need to point the Document Root to the root of the project you just created.

**1)** Ensure you have the SimpleTest project we created in the last lesson open in IntelliJ

**2)** Right click on SimpleTest

**3)** Navigate to New->HTML File

**4)** Name the HTML file index.html and click OK. A very simple HTML file is created on your behalf.

**5)** Inside of the Body of the file, add a div. Inside of the div, add the text Simple Test as shown in the following code. Save the file.

``<body>
    <div>Simple Test</div>
</body>``

As you will see shortly, Randori uses ids as object identifiers.

**6)** Next, navigate to the Run menu and choose Run. You will be presented with a small dialog box asking you to Edit Configurations. Select Edit Configurations.

![Edit Configuration](http://randoriframework.com/wp-content/uploads/2013/03/lesson2-1.png)

**7)** From the resulting dialog, press the + symbol in the upper left corner.

![Add New Configuration](http://randoriframework.com/wp-content/uploads/2013/03/lesson2-2.png)

**8)** A pop-down list will appear. Choose Randori Application.

![Add New Configuration](http://randoriframework.com/wp-content/uploads/2013/03/lesson2-3.png)

**9)** For this demo, just name the configuration Basic. Set the Index field to index.html, referencing the file you created above. From the module drop down, choose SimpleTest. Leave the Web Root field blank for this configuration.

![Edit Configuration](http://randoriframework.com/wp-content/uploads/2013/03/lesson2-4.png)

**10)** Click the Run button below. If all was configured properly, this Run action started the internal web server, found a free port and launched your index.html file in the browser. 

**11)** You can close your browser and now repeat the launch of your project by simply clicking the green play button just below the menu of your project.

![Runnable Configuration](http://randoriframework.com/wp-content/uploads/2013/03/lesson2-5.png)

That completes the goals for this lesson and all project setup. In the next section, you will begin to add functionality to the application by using the Randori Bootstrapper, then adding behaviors.