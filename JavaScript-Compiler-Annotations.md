### [JavaScript]
The [JavaScript] tag always decorates a class or package level function and has the following defined arguments:

**export : Boolean** (default true) – Controls the export of this particular JavaScript class. If the class is marked export="false" then the class itself is not exported into a separate file, or as a portion of a monolithic file. References to the class in other areas of the source persist and remain unchanged. The use case of this argument is to allow a compile time dependency on an object that will be fulfilled by externally available code at runtime. So, for example, I can mark HTMLDivElement as export=”false” as the JavaScript virtual machine defines this class at runtime.

**mode : String** (default proto, accepts proto, global or json) – Defines the way in which the class code is treated at runtime. 

* _proto_ - In proto mode, the class is exported as a prototype based representation of the class wherein the code exists in a simulated package structure constructed via an object tree. If needed, an $inherit() function call is created to express the class’s inheritance, the className function property is created, the getClassDependencies() and injectionPoints() functions are created.

 Please note the code below does not show possible optimizations. It is simply to express intent.

 **Source Definition**
```
package foo {

public class Bar {

  public var thing:String;

  public function get prop():String {
    return thing;
  }

  public function set prop( value:String ):void {
    thing = value;
  }

  public function pubMethod():void {
  }

  private function privMethod():void {
  }

  public static function staticMethod():void {
  }

  public function Bar( p1:String ) {
  }
}
}
```
**Exported Definition**
```
if (typeof foo == "undefined")
  var foo = {};

foo.Bar = function(p1) {
  this.thing = null;
};

foo.Bar.prototype.get_prop = function() {
  return this.thing;
};

foo.Bar.prototype.set_prop = function(value) {
  this.thing = value;
};

foo.Bar.prototype.pubMethod = function() {
};

foo.Bar.prototype.privMethod = function() {
};

foo.Bar.staticMethod = function() {
};

foo.Bar.className = "foo.Bar";

foo.Bar.getClassDependencies = function(t) {
	return [];
};

foo.Bar.injectionPoints = function(t) {
  var p;
  switch (t) {
    case 0:
      p = [];
      p.push({n:'p1', t:'String'});
    break;
  default:
      p = [];
    break;
  }
  return p;
};
```
**Source Use**
```
var x:Bar = new Bar( "thing" );
x.prop = "stuff";
Bar.staticMethod();	
```
**Exported Use**
```
var x = new foo.Bar("thing");
x.set_prop("stuff");
foo.Bar.staticMethod();
```

* _global_ - In global mode, the static methods and properties of the class become methods and properties of the window object itself without regard to the original package of the class. Non static methods and properties should be ignored and, although we may not be able to enforce at this time, this object should NOT be instantiated anywhere in the source. It is effectively a static class. So, foo.Bar.method1 exports simply as method1. Further, all references to foo.Bar.method1() now simply become method1()

 **Source Definition**
```
package foo {

[JavaScript(mode="global")]
public class Bar {

  public static var thing:String = "Mike";

  public static function staticMethod():void {
  }
}
}
```
**Exported Definition**
```	
thing = "Mike";

staticMethod = function() {
};
```
**Source Use**
```
Bar.staticMethod()
Bar.thing = "Bob";
```
**Exported Use**
```	
staticMethod()
thing = "Bob";
```

* _json_ - The main use for mode="json" is to create compile time value objects which are treated as generic JavaScript objects at runtime. Specifying mode="json" should implicitly mark the class as export="false". There are two cases under json, constructor setting and property setting.

  Constructor setting. Please note in the example below, it is up to the developer to ensure the properties match the constructor argument names.

  **Source Definition**
```
package foo {

[JavaScript(mode="json")]
public class Bar {
  public var thing:String;

  public var prop():String;

  public function Bar(prop:String,thing:String){
  }
}
}	
```
  **Source Use**
```
var x1:Bar = new Bar( "7", "8" );	
```
  **Exported Use**
```
var x1 = {prop:"7", thing:"8"};
```

  Variable and Property setting:

 **Source Definition**
```
package foo {

[JavaScript(mode="json")]
public class Bar {

  public var thing:String;

  public function get prop():String {
    return thing;
  }

  public function set prop( value:String ):void {
    thing = value;
  }

  public function Bar(){
  }
}
}	
```

  **Source Use**
```
var x2:Bar = new Bar();
x2.prop = "7";
x2.thing = "8";
```

  **Exported Use**
```
//In an ideal world
var x2 = {prop:"7", thing:"8"};

//Also acceptable
var x2 = {};
x2.prop = "7";
x2.thing = "8";
```

**name : String** (default null, accepts any string) – Overrides the default name of this particular class in the exported code. When the name argument is null, the compiler should use the fully qualified name of the class. When the argument is populated, the value of the name argument is used as a substitute for the fully qualified name of the class in all exported code. Presently, the name attribute is only supported in combination with export="false". While, this attribute could be used with any mode, it only makes a noticeable difference in proto. A class foo.Bar with the name argument set to "test.Case" will show the following functionality:

 **Source Definition**
```
package foo {

[JavaScript(name="test.Case")]
public class Bar {

  public var thing:String = "Mike";

  public function method():void {
  }

  public static function staticMethod():void {
  }

  public function Bar( prop1:int ) {
  }
}
}	
```
**Source Use**
```
var x:Bar = new Bar(1);
x.thing = "7";
x.method();
Bar.staticMethod(); 	
```
**Exported Use**
```
var x = new test.Case(1);
x.thing = "7";
x.method();
test.Case.staticMethod(); 
```

**omitconstructor: Boolean** (default false) – Maps to the webkit [OmitConstructor] tag indicating that a class cannot be instantiated. This may change in the future as it is under consideration for deprecation by WebKit.

**nativecondition: String** (default null) – Future use to indicate a group of features only available given a certain pre-condition

---

### [JavaScriptConstructor]
The [JavaScriptConstructor] tag always decorates a class as ActionScript classes have a single constructor:

**factoryMethod:String** (default null, accepts any string) – Used to override the object’s creation mechanism. A practical use case for this property is the construction of an HTMLDivElement. It may be natural for a developer in ActionScript to indicate that they wish a new HTMLDivElement with its constructor, however, the operation is performed differently in JavaScript. 

**Source Definition**
```
package randori.webkit.html {

  [JavaScript(export="false", name="HTMLDivElement")]
  [JavaScriptConstructor(factoryMethod="document.createElement('div')")]
  public class HTMLDivElement extends HTMLElement {
  }
}
```

**Source Use**

```
var x:HTMLDivElement = new HTMLDivElement();	
```

**Exported Use**
```
var x = document.createElement('div');
```

---
### [JavaScriptMethod]
The [JavaScriptMethod] tag decorates a method of an ActionScript class.

**name:String** (default null, accepts any string) – Much like the name attribute of the JavaScript annotation, this attribute and tag are used to override the name of a single method. A practical use case for this property is mapping multiple typed ActionScript methods to a single untyped JavaScript method. 

**Source Definition**

```
package randori.webkit.xml {
  [JavaScript(export="false", name="XMLHttpRequest")]
  public class XMLHttpRequest {

  [JavaScriptMethod(name="send")]
  public function sendArrayBuffer(data:ArrayBuffer):void{   
  }

  [JavaScriptMethod(name="send")]
  public function sendBlob(data:Blob):void {
  }

  [JavaScriptMethod(name="send")]
  public function sendDocument(data:Document):void {
  }

  [JavaScriptMethod(name="send")]
  public function sendString(data:String):void {
  }
}
}
```

**Source Use**
```
var x:XMLHttpRequest = new XMLHttpRequest();
x.sendArrayBuffer( null );
x.sendBlob ( null );
x.sendDocument ( null );
x.sendString ( null );
```

**Exported Use**
```	
var x:XMLHttpRequest = new XMLHttpRequest();
x.send(null);
x.send(null);
x.send(null);
x.send(null);
```

**mode:String** (default null, global) – Similar to the mode attribute of the JavaScript annotation, the global mode of the JavaScriptMethod annotation instructs the compiler to generate the code outside of any structure, such as a method or class block. This annotation should only be legal on private static functions of a class. The first use case is our testing integration. The only use case we currently have is in conjunction with the global mode of the JavaScript annotation.

**Source Definition**

```
package foo {
import randori.webkit.page.Window;

[JavaScript(mode="global")]
public class Bar {

	[JavaScriptMethod(mode="global")]
	private static function doItGlobal() {
		Window.console.log("I run immediately")
	}

	public static function doItNotQuite() {
		Window.console.log("You need to call me")
	}
}
}
```

**Exported Definition**
```
console.log("I run immediately");

doItNotQuite = function() {
  console.log("You need to call me");
};
```

---
### [JavaScriptProperty]
The [JavaScriptProperty] tag decorates a variable or property of an ActionScript class.

**name:String** (default null, accepts any string) – Much like the name attribute of the JavaScriptMethod annotation, this attribute and tag are used to override the name of a single property or variable. A practical use case for this property is mapping multiple typed ActionScript properties to a single untyped JavaScript property. 

**Source Definition**

```
package foo.List {
  [JavaScript(export="false", name="List")]
  public class List {

  [JavaScriptMethod(name="label")]
  public var labelFunction:Function;
  
  public var label:String;

  }
}
```

**Source Use**
```
var x:List = new List();
x.label = "Hello";
x.labelFunction = function(data:Object) {
                    return "Hi" 
                  };	
```

**Exported Use**
```
var x = new foo.List();
x.label = "Hello";
x.label = function(data) {
	    return "Hi";
	  };
```

---
### [JavaScriptCode]
The [JavaScriptCode] tag decorates a method of an ActionScript class.

**file:String** (default null, accepts any string) – Specifies an external filename which contains JavaScript code. The code found in the external file should completely replace the method body of the decorated method.
 
**Source Definition**
```
package guice.utilities {
  [JavaScript(mode="global")]
  public class GlobalUtilities {
	      
    [JavaScriptCode(file="_createDelegate.js")]
    public static function $createStaticDelegate(  
        scope:Object, func:Function ):Function {
      return null;
    }
  }
}
```
**Exported Definition**
```
$createStaticDelegate = function(scope, func) {
  if (scope == null || func == null)
    return func;
  var delegate = function () {
    return func.apply(scope, arguments);
  };
delegate.func = func;
delegate.scope = scope;

return delegate;
};
```