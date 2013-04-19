<b>Please Note:</b> There is a new plugin version available which requires an SDK change, {please read for more details:}[https://github.com/RandoriAS/randori-plugin-intellij/wiki/Instructions-for-updating-your-Plugin].

In the last lessons, you explored the ViewStack which is a great way to manage regions of the application which will display swappable content. Items in the ViewStack are expected to be complete HTML pages which could be edited as independent documents.

However, there are times that you simply wish to include a fragment of HTML, with or without behaviors attached, into your application. This short lesson will explore including fragments, which is particularly useful when one starts building components.

1. Ensure you have the SimpleTest project open in IntelliJ

2. Right-click on the SimpleTest project and create a new directory named fragments

   You may notice that all HTML, CSS and assets are generally placed in folders at the root of the project, whereas the source code is placed under the src folders and in packages. This is intentional. The code in the src folder is transformed into js, which then resides in a generated folder at the project root. Code in the src folder will not be deployed into production; however, your assets, html and the generated folder constitutes a deployable project

3. Right click on fragments and create a new HTML file called testFragment.html

4. Delete the entire contents of this file and replace it with the following HTML:
```
<div>
    This is a simple fragment of HTML being loaded at runtime.
</div>
```
5. Save this file and open behaviors.css

6. Add a new class named testFragment as show below:
```
.testFragment {
    -randori-fragment: "fragments/testFragment.html";
}
```

   Just to illustrate the point, we will add a few visual styles into the definition next.

7. Add a height of 150px, a width of 200px, and a position relative to the testFragment
```
.testFragment {
    -randori-fragment: "fragments/testFragment.html";
    height: 150px;
    width: 200px;
    position: relative;
}
```

8. Save your CSS file and open index.html

9. In your index.html file, add a new div directly below the body. Assign the div the class testFragment.
```
<body class="simpleApp">
    <div class="testFragment"></div>
```

10. Save the index.html and run it

    You will see your message on the screen. Randori imported the fragment. Right now this is a simple fragment as it does not contain further behaviors. You will change that next.

11. Open your index.html and cut the following lines from the index.html. 
```
<div id="messageWindow" class="messageWindow">
    <div id="title">Simple Test</div>
    <div id="message">My Message</div>
</div>
```
12. Paste these lines into the testFragment.html.
```
<div>
    This is a simple fragment of HTML being loaded at runtime.
</div>

<div id="messageWindow" class="messageWindow">
    <div id="title">Simple Test</div>
    <div id="message">My Message</div>
</div>
```

13. Ensure that the body of your index.html looks like the following code:
```
<body class="simpleApp">
    <div class="testFragment"></div>

    <input id="pushPeople" value="Push People" type="button"/>
    <input id="pushThings" value="Push Things" type="button"/>
    <input id="popView" value="Pop View" type="button"/>

    <div id="myViewStack" class="viewStack" style="position:relative"/>
</body>
```

14. Save and run the application.

    The screen display should be very similar, with a few positioning differences. However, Randori has now loaded the fragment from an external file. That fragment also contained behaviors. 

Those behaviors were instantiated and injected into the mediator as though they had always existed on the page. Everything, including the message substitution you created with Guice earlier, was still applied.