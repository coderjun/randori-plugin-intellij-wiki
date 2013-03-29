In this lesson, you will add the basic script tags needed for Randori along with the Randori bootstrap code. You will then create a behavior interact with the DOM.

1) Ensure you have the SimpleTest project we created in the last lesson open in IntelliJ

2) Open the index.html file in the editor.

3) Start by adding a script tag in the head of the HTML file which will load jQuery from google’s content host. Alternatively you are free to use a local copy of jQuery if you have one.

``<head>``

``<title></title>``

``<!-- JQuery -->``

``<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js"></script>``

``</head>``

Right now Randori has a light dependency on several jQuery methods used mostly as a convenience mechanism. It is likely that future versions of Randori will make this dependency optional.

4) Next, you will add two more script tags below this one. These script tags will cause the inclusion of the Randori dependency injection framework Guice and the Randori framework.

<head>
  <title></title>
  <!-- JQuery -->
  <script type="text/javascript"
    src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js">
  </script>

  <!-- Randori Dependencies -->
  <script type="text/javascript" 
src="generated/lib/RandoriGuice.js"></script>
  <script type="text/javascript" src="generated/lib/Randori.js"></script>
</head>

Right now your project doesn’t have a generated or libs folder. These will be added automatically after our first compile. The generated folder is the default location where Randori places cross-compiled code. The lib folder is where dependency libraries are added during compilation.
5) Next you will add the bootstrap code that launches Randori. Add another script block and add the code below, still inside the head.
<script type="text/javascript">
  //Start the randori framework
  jQuery(function () {
    var randoriApp = new randori.startup.RandoriBootstrap(document);
    randoriApp.launch();
  });
 </script>

The code above instructs Randori to parse the DOM starting at the requested point. In this case document. This is very important. Randori doesn’t need to manage an entire app. The framework itself doesn’t try to manage state of manage the application in any way. It is just glue that is used to facilitate the connection between the DOM and code using a mediator and set of decorator patterns. Any state, application or other aspects you choose to maintain are at your own discretion. 
6) All of the source that you write for Randori will exist inside the src folder of the project. Right click on the src folder. Choose New->Package. Name the package behaviors.
7) Right click on the behaviors package now. Choose New->Randori Behavior. Name the class MessageWindow. Press OK.
At this point you may be wondering why you are creating classes. It’s a fair question if you are. Randori is about mixing paradigms. Code can be in JavaScript or ActionScript, functional or object-oriented. The goal of Randori is to let you choose the right approach at different levels and for different team skills. Ultimately Randori is about integrating teams to build great apps.
This particular class extends AbstractBehavior. In Randori, behaviors are classes that can be written in JavaScript or ActionScript. They are responsible for decorating a DOM node, managing any visual aspects and providing an API to the rest of the application. This will be an extraordinarily simple behavior; however, behaviors can be very complex, such as Lists and Grids.
8) Before you begin coding, it’s a good idea to turn on the Randori Problems View. Navigate to View->Tool Windows -> Randori Problems.
This view will allow you to see output from the Randori Compiler as you are working with your code. The compiler builds your code whenever you save. However, it only actually generates JavaScript when requested.
9) Inside of the MessageWindow class, override the protected function named onRegister.
    override protected function onRegister():void {

    }
The onRegister() method is called by the Randori framework once the behavior has been created, any visual or object dependencies are injected and the root node is established. That will make more sense as we continue but for now consider it to be the first ‘safe’ time you are allowed to perform actions in a behavior.
10) Inside the onRegister() method, we will modify the node we decorate to display a simple message.
    override protected function onRegister():void {
        this.decoratedNode.html("I am registered");
    }
Every Randori behavior is provided a reference to the node it decorates wrapped in jQuery. In this case, we are setting the html of that node to a message so we can simply ensure the behavior is working. Presently you have a behavior but it is not used anywhere in the application. We will change that next.
11) Save this file. When you do, the Randori compiler will generate JavaScript code from your work so far. 
After doing so, a generated folder will appear in the project view to the left. If you expand the generated folder, you will find a lib folder with the two Randori libraries. You will also find a behaviors folder, and inside of it MessageWindow.js.
Randori uses the package to determine the directory structure of the output JavaScript. If you open MessageWindow.js you will see the generated code. During normal development, Randori generates very basic and readable code which can later be optimized for production.
You will see your onRegister() method in this JS along with a few other items. 
behaviors.MessageWindow.prototype.onRegister = function() {
	this.decoratedNode.html("I am registered");
};
Randori has generated some code indicating that you inherit from AbstractBehavior.
$inherit(behaviors.MessageWindow, randori.behaviors.AbstractBehavior);
It has generated a className because this piece of JS actually represents an object oriented class.
behaviors.MessageWindow.className = "behaviors.MessageWindow";

It generates a method named getClassDependencies, which is used by Randori to ensure any classes used in your MessageWindow class are dynamically loaded before they are needed. In this case, there are none.
behaviors.MessageWindow.getClassDependencies = function(t) {
	var p;
	return [];
};
Finally it generates a method which is used to determine injection points. This will become clearer as we grow this small demo and use the features of Randori.
behaviors.MessageWindow.injectionPoints = function(t) {
	var p;
	switch (t) {
		case 1:
			p = randori.behaviors.AbstractBehavior.injectionPoints(t);
			break;
		case 2:
			p = randori.behaviors.AbstractBehavior.injectionPoints(t);
			break;
		case 3:
			p = randori.behaviors.AbstractBehavior.injectionPoints(t);
			break;
		default:
			p = [];
			break;
	}
	return p;
};

Remember, the goal at this stage is readable, not optimized code.
12) Return to the index.html file. As a starting point, find your div with Simple Test text and add the following code:
<div data-behavior="behaviors.MessageWindow">Simple Test</div>

This code indicates that the MessageWindow behavior should decorate this node. Using HTML data attributes is a quick way to test, but it is more limiting and less elegant that other approaches which we will explore shortly.
13) Now click the green play button above to launch the application. The web browser will show your application with the message I am registered where Simple Test used to be.
Randori automatically loaded the MessageWindow behavior when it was needed, instantiated it and provided it with the decorateNode, which your behavior then modified.
This is an extremely simple example, but it is the basis for much more complicated constructs as your application evolves.