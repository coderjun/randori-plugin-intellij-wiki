<b>Please Note:</b> There is a new plugin version available which requires an SDK change, [please read for more details](https://github.com/RandoriAS/randori-plugin-intellij/wiki/Instructions-for-updating-your-Plugin).

**This lesson requires SDK version 0.2.5 or greater**

In this lesson, you will explore the service model frequently used in Randori applications. It’s primarily comprised of two types of classes, Services and Parsers.

However, everything in this lesson can be taken as a suggestion. Randori is an open, injectable framework where you can actually change the core of the framework itself, by simply changing the injection configuration. Everything, including the injector itself, is swappable.

The point is that you can implement any service pattern you choose within Randori; there are no dependencies on these suggestions but they are an effective approach to use when starting out.

1. Ensure you have the SimpleTest project open in IntelliJ

2. Right click on the assets directory and create a new directory named data.

3. Right click on the data folder and create a new file named committers.json
4. Copy the following data into your JSON file
```
[
    {
        "name": "Mike S",
        "repositories": "randori-compiler,randori-plugin-intellij"
    },
    {
        "name": "Roland",
        "repositories": "randori-plugin-intellij,randori-tools,randori-libraries"
    },
    {
        "name": "Fred",
        "repositories": "randori-plugin-intellij"
    },
    {
        "name": "Mike L",
        "repositories": "randori-guice-framework,randori-framework"
    }
]
```
5. Right click on the src folder and create a new package named services.

6. Right click on the services package and create a new Randori class named CommitterService.

7. Make the CommitterService extend AbstractService
```
public class CommitterService extends AbstractService {
```

   AbstractService provides a few methods useful in creating a basic service layer, including the ability to construct a uri, a hook for modifying headers, and send a simple request a uri. The AbstractService accepts an XMLHttpRequest in its constructor, so you will add that next.

8. Add a parameter to the constructor named xmlHttpRequest of type XMLHttpRequest.
```
public function CommitterService( xmlHttpRequest:XMLHttpRequest ) {
}
```

9. Pass the xmlHttpRequest into the call to super()
```
public function CommitterService( xmlHttpRequest:XMLHttpRequest ) {
	super( xmlHttpRequest );
}
```

10. Create a new method named get() that returns a Promise, specifically a randori.async.Promise.
```
public function get():Promise {
}
```
   The randori.async.Promise is a Promises/A+ implementation that is used throughout the framework as a way to handle asynchronous behavior.

11. Create a new local variable named promise of type Promise, assign it to the result of calling sendRequest. Pass the sendRequest the verb “GET” and the path to the json file. Then return that promise.
```
public function get():Promise {
	var promise:Promise = sendRequest("GET","assets/data/committers.json");
	return promise;
} 
```

   This is a first version of the service. Later this can be evolved to have a more dynamic way to provide the path.

   Right now calling this service would return a String with the JSON data. That’s less than ideal so next this will change to introduce a compile time model and a parser to hold this data shortly.

12. Right click on the src package and create a new package named models.

13. Right click on the models package and create a new Randori class named Committer.

14. Add a new public property to the class named name of type String
```
public class Committer {
	public var name:String;
}
```
15. Add a second new public property to the class named repositories of type Array
```
public class Committer {
	public var name:String;
	public var repositories:Array;
}
```

16. Add a [JavaScript] annotation above the class. Set export to false, name to Object and mode to json.
```
[JavaScript(export="false",name="Object",mode="json")]
public class Committer {
	public var name:String;
	public var repositories:Array;
}
```

   As a reminder, the export=”false” tells the compiler not to make a JavaScript file for this class. The name="Object" tells the compiler to use a generic Object instead if this class is ever referenced. The mode="json" tells the compiler just to generate JSON for any instantiations of this class.

17. Right click on the services package and create a new package named parsers.

18. Right click on the parsers package and create a new Randori class named CommitterParser.

   For this particular class, you will create a Parser to do some specific parsing. However, if all of your objects simply use standard JSON, it is possible to reuse a standard parser in many cases.

19. Create a new public method named parseResult, which accepts a generic object and returns an Array.
```
public function parseResult(result:Object):Array {
}
```
In this method, you transform the data from json to whatever structure your application needs.

20. Inside the method, create a new variable named json of type Array and set it to the result of calling JSON.parse() passing it the result, cast as a String. Cast the return as an Array.
```
var json:Array = JSON.parse( result as String ) as Array;
```

   If the JSON file were formed differently, you might be done at this point. However, this is a bit more complex in order to illustrate the idea of a parser. You are going to turn the comma separated list in the repositories field into an array.

21. Create a for loop, using i as an iterator that loops over the json array
```
public function parseResult(result:Object):Array {
	var json:Array = JSON.parse( result as String ) as Array;

	for ( var i:int=0; i<json.length; i++ ) {
	}
}
```

22. Inside the loop, create a variable named committer of type Committer and assign it to json[i];
```
for ( var i:int=0; i<json.length; i++ ) {
	var committer:Committer = json[ i ];
}
```

23. Next, create a var named list of type String and assign it to json[ i ].repositories
```
for ( var i:int=0; i<json.length; i++ ) {
	var committer:Committer = json[ i ];
	var list:String = json[ i ].repositories;
}
```

24. Finally, assign the result of calling the split() method on the list to committer.repositories
```
for ( var i:int=0; i<json.length; i++ ) {
	var committer:Committer = json[ i ];
	var list:String = json[ i ].repositories;
	committer.repositories = list.split();
}
```

   This code effectively rewrites the comma separated list of repositories with an Array

25. As the last step of this method, return json. Your final method should look like the following:
```
public function parseResult(result:Object):Array {
	var json:Array = JSON.parse( result as String ) as Array;

	for ( var i:int=0; i<json.length; i++ ) {
		var committer:Committer = json[ i ];
		var list:String = json[ i ].repositories;
		committer.repositories = list.split();
	}

	return json;
}
```

26. Return to the CommitterService and alter the constructor to accept the CommitterParser in the constructor.
```
public function CommitterService( xmlHttpRequest:XMLHttpRequest, parser:CommitterParser ) {
```

27. Add a private variable to the class named parser of type CommitterParser
```
private var parser:CommitterParser;
```

28. Finally, in the constructor, assign the instance variable to the argument of the constructor
```
public function CommitterService( xmlHttpRequest:XMLHttpRequest, parser:CommitterParser ) {
	super( xmlHttpRequest );
	this.parser = parser;
}
```

29. Inside the get() method, change the return statement to the result of calling promise.then() and passing the function parser.parseResult as the following code shows:

   ```
   public function get():Promise {
	   var promise:Promise = sendRequest("GET","assets/data/committers.json");
	   return promise.then( parser.parseResult );
   }
   ```

   This code creates a promise that passes the result of the service call through the parser’s parseResult method and then returns that value.

30. Open the PeopleMediator class create a new public variable named service of type CommitterService. Add an inject annotation above it.
```
[Inject]
public var service:CommitterService;
```

   This example is using property injection, however, in production constructor injection is preferred for anything that is absolutely required for a class to function properly.

31. Empty the onRegister() method of all the existing code.

32. Inside the onRegister() method, add the following code:
```
service.get().then( handleResult );
```

   This code calls your get() method and then calls the handleResult method (which you will create next) with the parsed result.

33. Create a handleResult() method which defines a parameter named results of type Array:
```
private function handleResult( result:Array ):void {
}
```
34. Inside of the handleResult() method, assign the results to the data property of the names SimpleList.
```
private function handleResult( result:Array ):void {
	names.data = result;
}
```

35. Save your code and run it. When you click the Push People button, you will see a list of committers. 

   If you watch the network requests coming from the application when you click the Button, you will see that the PeopleMediator is loaded, which requires the CommitterService, which requires CommitterParser. All are loaded, and then the code to load the committers.json file, parses it and passes it along to the SimpleList.

In the next lesson, you will [learn about decoupled inter-object communication.](https://github.com/RandoriAS/randori-plugin-intellij/wiki/Lesson-13:-Inter-object-Communication-%E2%80%93-Signals-and-EventBus).
