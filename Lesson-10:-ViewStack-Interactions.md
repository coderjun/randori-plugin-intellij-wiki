
<b>Please Note:</b> There is a new plugin version available which requires an SDK change, {please read for more details}[https://github.com/RandoriAS/randori-plugin-intellij/wiki/Instructions-for-updating-your-Plugin].

In this lesson, you will continue to use the ViewStack to load multiple views and create a simple behavior with tabs to manage it. As you may remember, the ViewStack understands how to perform the dynamic loading of your view’s html, and then asks Randori to perform the same steps outlined in previous lessons to build the newly loaded content.

With Randori, everything starts with a URL to an HTML file. After that HTML file is loaded, Randori decorates with the requisite functionality allowing it to become interactive.

1. Ensure you have the SimpleTest project open in IntelliJ

2. Right click on the views folder, and create a new HTML file named things.html

3. Inside the body tag of things.html, add the following text:
```
<body>
    This is the things page
</body>
```
4. Save your HTML file and open index.html

5. In your index.html file, rename the existing input (currently named loadMore) to pushPeople and provide an appropriate value:
```
<input id="pushPeople" value="Push People" type="button"/>
```

6. Below that, add a second input with an id of pushThings and an appropriate value:
```
<input id="pushThings" value="Push Things" type="button"/>
```

7. Next, add a button input with an id of popView and a value of Pop View. Your code should look like the following:
```
    <input id="pushPeople" value="Push People" type="button"/>
    <input id="pushThings" value="Push Things" type="button"/>
    <input id="popView" value="Pop View" type="button"/>

    <div id="myViewStack" class="viewStack" style="position:relative"/>
```
8. Save your html file and run the application. You will receive an error. This is intentional.

   Right now you have a required view injection for an element with an id of loadMore that isn’t being fulfilled. Randori throws errors when this happens to simplify debugging and prevent silent failures. Randori will never run the onRegister() method if a required view injection isn’t present.

   You can add a (required="false") attribute to any [View] annotations

   ```
   [View(required="false")]
   public var myViewElement: JQuery;
   ```

   Marking an element this way will stop Randori from validating its existence when the mediator is created. This is very useful for creating behaviors and mediators which have optional view elements; however, it is then your responsibility to check for their existence in any code before their use.

9. Right click on the loadMore variable, choose Refactor->Rename. Rename the variable to pushPeople. 

10. Right click on the handleLoadMore method, choose Refactor->Rename. Rename the method to handleLoadPeople. 

11. Add a new public var named pushThings and add the View annotation above it:
```
[View]
public var pushThings:JQuery;
```

12. Add a new public var named popView and add the View annotation above it:
```
[View]
public var popView:JQuery;
```

13. As the last line of the onRegister() method, add a new variable named viewStack of type ViewStack and assign it to myViewStack.
```
override protected function onRegister():void {
	var messageObj:Message = messageGenerator.newMessage();
	messageWindow.showMessage(messageObj.title, messageObj.message );

	pushPeople.click( handleLoadPeople );

	var viewStack:ViewStack = myViewStack;
}
```
You are creating a local variable which points to your instance of the ViewStack. This will be used in the anonymous functions you will create next.

14. Just below this variable, call the pushThings.click() method and pass it an anonymous function that accepts an event of type Event.
```
pushThings.click( function( event:Event ):void {
} );
```
15. Inside the function push views/things.html onto the ViewStack. Be sure to use the viewStack variable you declared locally.
```
pushThings.click( function( event:Event ):void {
	viewStack.pushView("views/things.html");
} );
```
16. Just below this function, call the popView.click() method and pass it an anonymous function that accepts an event of type Event.
```
popView.click( function( event:Event ):void {
} );
```

17. Inside the function call the popView() method of the ViewStack. Be sure to use the viewStack variable you declared locally.
```
popView.click( function( event:Event ):void {
	viewStack.popView();
} );
```
18. Run the application. Open the debug tools and look at the div named viewStack. Push people onto the viewStack followed by things. You will see these being added to the div.

    You can push multiple copies of any view onto the stack. It truly works as a stack, with multiple copies existing, each with their own mediators where applicable.

19. Click the pop view button to remove a view from the stack. 
You will only ever see the top item on the stack. And you can only pop the top item off of the stack.

    The ViewStack class also has a selectView() method which accepts a url. This method causes the ViewStack to be resorted, moving the view specified by the url to the top of the stack. One caveat. In the event that multiple versions of the same url have been added to the ViewStack, the first one found will be moved. 