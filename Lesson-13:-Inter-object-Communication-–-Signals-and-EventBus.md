<b>Please Note:</b> There is a new plugin version available which requires an SDK change, [please read for more details](https://github.com/RandoriAS/randori-plugin-intellij/wiki/Instructions-for-updating-your-Plugin).

In this lesson, you will explore two decoupled methods of communication provided by the Randori framework. The two methods introduced in this lesson are generally a way to handle interoperability between your various cross-compiled objects. 

Remember that, ultimately, Randori is primarily glue that allows the interoperability of JS, cross-compiled AS and/or C# and HTML/CSS. Therefore you will also have at your disposal the methods provided by any other libraries you choose to use, e.g. you have already used jQuery and event listeners to handle some tasks in the application. 

1. Ensure you have the SimpleTest project open in IntelliJ

2. Right click on the src folder and create a new package named eventBus.

3. Right click on the eventBus package and create a new Randori Class named AppEventBus

4. Add a public variable named dataLoadStarted of type SimpleSignal to the AppEventBus class.
```
public var dataLoadStarted:SimpleSignal;
```
  A SimpleSignal is a class based on the concepts of AS3 Signals by Robert Penner (thanks Rob). It’s a callback mechanism designed to allow decoupling like events but with several advantages. 
This application is a tiny example of a few loosely related features. Does it need an eventbus? Likely no. However, the concepts here will scale well beyond this use.  
  

5. Add an [Inject] tag above the dataLoadStarted variable.
```
[Inject]
public var dataLoadStarted:SimpleSignal;
```

6. Add a second public variable named dataLoadCompleted of type SimpleSignal to the AppEventBus class and add an [Inject] tag above it.
```
[Inject]
public var dataLoadCompleted:SimpleSignal;
```
  This ‘event’ bus can contain any number of signals which can be individually listened for and dispatched.

7. Open PeopleMediator and add a new public var named appBus of type AppEventBus to the class and decorate it with an Inject annotation.
```
[Inject]
public var appBus:AppEventBus;
```
  The event bus will now be injected into this mediator.

8. Edit the onRegister() method. As the first line of the method you will call the dispatch() method of the dataLoadStarted property of the bus and pass it a new Date object.
```
    override protected function onRegister():void {
        appBus.dataLoadStarted.dispatch( new Date() );
		service.get().then( handleResult );
    }
```
  You injected the instance of AppEventBus into a variable named appBus. AppEventBus has a property that you added called dataLoadStarted which is a SimpleSignal. 
SimpleSignal is used throughout the framework for simple messaging needs. It exposes several methods that you will use throughout this lesson. The first is dispatch() which takes a variable number of arguments, but allows you to pass data to anyone listening for the signal.

9. Next Edit, the handleResult() method. This time call the dispatch() method of the dataLoadCompleted property instead and pass it a new Date object.

```
private function handleResult( result:Array ):void {
    appBus.dataLoadCompleted.dispatch( new Date() );
    names.data = result;
}
```
  Effectively, when you start loading the data, you will dispatch one signal on the bus and another when it is complete, each with a Date object representing the time.

10. Save your file and switch to the IndexMediator.

11. Add a new public var named bus of type AppEventBus to the class and decorate it with an Inject annotation.
```
[Inject]
public var appBus:AppEventBus;
```

12. At the bottom of the onRegister() method, add the following code snippet:
```
appBus.dataLoadStarted.add( handleLoadStart );
```
  Here you are using the add() method to add a listener to that Signal. 

13. In the onDeRegister() method, remove() the listener:
```
appBus.dataLoadStarted.remove( handleLoadStart );
```

14. Just below the onDeRegister() method, add a new method with the following signature:
```
private function handleLoadStart( date:Date ):void {
}
```

15. Inside the method, call the Window.console.log() method and output the date as shown below:
```
private function handleLoadStart( date:Date ):void {
    Window.console.log( "Load Started " + date );
}
```
  The static Window class holds many of the common objects, such as console, available in the browser.

16. Inside the onRegister() method, add the following code:
```
appBus.dataLoadCompleted.addOnce( handleLoadComplete );
```
  This time you are listening to the dataLoadComplete signal, but you are doing so with the addOnce() method. This method listens for the first signal to dispatch and then removes itself as a listener, so you will only receive one notification. 

17. Add the following code to the onDeRegister() method:
```
appBus.dataLoadCompleted.remove( handleLoadComplete );
```
  You might ask why this needs to be removed if it can only fire once. The answer is that you still need to clean up the listener in the event that it never fired. If you never received a dataLoadComplete, then this callback would continue to be registered and hence have garbage collection implications.

18. Add a new method called handleLoadComplete()
```
private function handleLoadComplete( date:Date ):void {
    Window.console.log( "Load Completed " + date );
} 

```
  At this point everything is wired up for the event bus, unfortunately the IndexMediator and PeopleMediator will each get their own copy of the AppEventBus. We need to specify it should be a singleton to ensure they receive the same copy.

19. Save this file and open your BoringContext.

20. Add this line inside of the configure() method
```
binder.bind( AppEventBus ).inScope( Scope.Singleton ).to( AppEventBus );
```
  In this code, you have instructed the dependency injection framework to create the AppEventBus but treat it as a Singleton. So, every object that requests the event bus will now receive the same version.

21. Save your file and launch the app in a browser. Make sure you can see the console view and click load people. You should see that the PeopleMediator communicated with the IndexMediator through the event bus.

