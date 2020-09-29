Scala JS Tutorial====

This is a step-by-step tutorial where we start with the setup of a Scala.js sbt project and end up having some user interaction. 

## Setup

First create a new folder where your sbt project will go.

### sbt Setup
To setup Scala.js in a new sbt project, we need to do two things:
* Add the Scala.js sbt plugin to the build
* Enable the plugin in the project

Adding the Scala.js sbt plugin is a one-liner in project/plugins.sbt (All file names we write in this tutorial are relative to the project root):
    
```
addSbtPlugin("org.scala-js" % "sbt-scalajs" % "1.2.0")
```

We also setup basic project settings and enable this plugin in the sbt build file (build.sbt, in the project root directory):
```
enablePlugins(ScalaJSPlugin)

name := "Scala.js Tutorial"
scalaVersion := "2.13.1" // or any other Scala version >= 2.11.12

// This is an application with a main method
scalaJSUseMainModuleInitializer := true

```

Last, we need a project/build.properties to specify the sbt version.
```
sbt.version=1.3.7
```

That is all we need to configure the build.

### HelloWorld application : 
For starters, we add a very simple Main object. Create the file **src/main/scala/Main.scala** : 
 
```scala
object Main {
  def main(args: Array[String]): Unit = {
    println("Hello world!")
  }
}
```

As you expect, this will simply print “HelloWorld” when run. To run this, simply launch sbt and invoke the run task:

## Integrating with HTML
Now that we have a simple JavaScript application, we would like to use it in an HTML page. To do this, we need two steps:
* Generate a single JavaScript file out of our compiled code
* Create an HTML page which includes that file

### Generate JavaScript
To generate a single JavaScript file using sbt, just use the fastOptJS task:

```
> fastOptJS
```

This will perform some fast optimizations and generate the target/scala-2.13/scala-js-tutorial-fastopt.js file containing the JavaScript code.

### Create the HTML Page
To load and launch the created JavaScript, you will need an HTML file. Create the file index.html in the project root with the following content : 
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>The Scala.js Tutorial</title>
  </head>
  <body>
    <!-- Include Scala.js compiled code -->
    <script type="text/javascript" src="./target/scala-2.13/scala-js-tutorial-fastopt.js"></script>
  </body>
</html>
```

## Using the DOM

### Adding the DOM Library
To use the DOM, it is best to use the statically typed Scala.js DOM library. To add it to your sbt project, add the following line to your build.sbt:
```
libraryDependencies += "org.scala-js" %%% "scalajs-dom" % "1.1.0"
```

You will notice the %%% instead of the usual %%. It means we are using a Scala.js library and not a normal Scala library

Now that we added the DOM library, let’s adapt our HelloWorld example to add a tag to the body of the page, rather than printing to the console.

First of all, we import a couple of things:
```
import org.scalajs.dom
import org.scalajs.dom.document
```

We now create a method that allows us to append a <p> tag with a given text to a given node:
```
def appendPar(targetNode: dom.Node, text: String): Unit = {
  val parNode = document.createElement("p")
  parNode.textContent = text
  targetNode.appendChild(parNode)
}

```

Replace the call to println with a call to appendPar in the main method:

```
def main(args: Array[String]): Unit = {
  appendPar(document.body, "Hello World")
}
```

### Rebuild the JavaScript

To rebuild the JavaScript, simply invoke fastOptJS again:

You can now reload the HTML in your browser and you should see a nice “Hello World” message.


### Adding React support in scala : 
We are using Slinky framework for writing React apps in Scala. 

Add the below library into build.sbt file to add a support for slinky.

```
libraryDependencies += "me.shadaj" %%% "slinky-core" % "0.6.6" // core React functionality, no React DOM
libraryDependencies += "me.shadaj" %%% "slinky-web" % "0.6.6" // React DOM
```


 