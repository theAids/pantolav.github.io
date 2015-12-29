---
layout: post
title: Bluemix Devops Services Delivery Pipeline
permalink: /devops-delivery-pipeline/
---
jetty tutorial:
-Dorg.apache.jasper.compiler.disablejsr199=true

##Application Development Tutorial

###Bluemix Devops Services Delivery Pipeline
[Bluemix Devops Services](https://hub.jazz.net) has a delivery pipeline that allows you to build, test, and deploy your web application.

In this tutorial you will learn to create the build stage, test stage, and deploy stage.  The build stage will use Gradle.  The test stage will use JUnit through Gradle.  The deploy stage will use the cf tool.


>**Prerequisite:**

>You are **required** to do the [Gradle's Unit Testing Tutorial](/gradle-unit-testing).

>- The sample code used in the current tutorial is based from the sample code used in [Gradle's Unit Testing Tutorial](/gradle-unit-testing). 

>You are not required (but **recommended**) to do the [Jetty Basics Tutorial](/jetty-basics).

>- **However**, ensure that your machine has the Jetty web server set-up as discussed in [Jetty Basics Tutorial](/jetty-basics).

>You are not required (but **recommended**) to do  the [Bluemix Basics Tutorial](/bluemix-basics).

>- **However**, ensure that you have a Bluemix account.  
>- Your account should have the space `dev` under the region `US-South`.  The creation of the space `dev` is discussed in [Bluemix Basics Tutorial](/bluemix-basics).
>- Make sure that the `cf` tool is installed.  The installation of `cf` tool is discussed in [Bluemix Basics Tutorial](/bluemix-basics).





<br>



####Copy Sample Codes from Git repository


1. Open a terminal window and create the directory `gradletemp` in the root directory.  Go to the created directory.

	```text		
	> mkdir gradletemp
	> cd gradletemp
	```

1. Clone the git repository `https://github.com/pong-pantola/gradle-web-application.git` and go to the created `gradle-web-application` directory.

	```text
	> git clone https://github.com/pong-pantola/gradle-web-application.git
	> cd gradle-web-application
	```
 
	The `gradle-web-application` directory has a subdirectory `src`.

	```text
	gradle-web-application/
	|
	|----build.gradle
	|
	|----src/
	     |
	     |----main/
	     |    |
	     |    |----java/net/tutorial/
	     |    |             |
	     |    |             |----Math.java
	     |    |             |----Calculator.java
	     |    |
	     |    |----resources/
	     |    |    |
	     |    |    |----log4j.properties        
	     |    |
	     |    |----webapp/
	     |         |
	     |         |----calculator.jsp
	     |	     
	     |----test/
	          |
	          |----java/net/tutorial/
	                        |
	                        |----MyTest.java
	                        |----TestRunner.java	
	``` 

	The subdirectories and files are exactly the same as the one you used and created in [Gradle's Unit Testing Tutorial](/gradle-unit-testing).  However, an additional file is added: `src/main/webapp/calculator.jsp`.

	`calculator.jsp` is the `.jsp` version of `Calculator.java`.

	The logical errors present in `Math.java` in the [Gradle's Unit Testing Tutorial](/gradle-unit-testing) are already corrected in this tutorial's version of `Math.java`.

<br>

####Review some of the Java classes and Build script


1. As mentioned, `Math.java`  logical errors in [Gradle's Unit Testing Tutorial](/gradle-unit-testing) are already corrected.

	**Source code** of	`src/main/java/net/tutorial/Math.java`:
 
	```java
	package net.tutorial;
	
	public class Math{
	
	  //will be used in the multiply method to simulate that
	  //the multiply method is taking too long to execute
	  private void delay(){
	    try{
	      Thread.sleep(3000);//3000 msec. or 3 sec. delay
	    } catch(InterruptedException ex) {
	      Thread.currentThread().interrupt();
	    }
	  }
	
	  public int add(int a, int b){
	    return a+b;
	  }
	  
	  public int sub(int a, int b){
	    return a-b;
	  }  
	
	  public int multiply(int a, int b){
	    return a*b;
	  }
	}
	```
 
	 Remember that in the [Gradle's Unit Testing Tutorial](/gradle-unit-testing), the method `add` had a logical error (i.e., instead of `a+b`;, the return statement is `a-b;`). 	In addition, a delay is inserted in the method `multiply` to demonstrate the timeout mechanism of JUnit.  These two issues are already resolved in this tutorial's version of `Math.java`. 
	
	<br>

1. `build.gradle` has the same content as the one you created in [Gradle's Unit Testing Tutorial](/gradle-unit-testing).

	Please review the text below for the current content of `build.gradle`:


	```text
	apply plugin: 'java'
	
	repositories {
	    mavenCentral()
	}
	 
	dependencies {
	    compile 'log4j:log4j:1.2.17'
	    testCompile 'junit:junit:4.12'
	}
	
	jar {
		from { configurations.compile.collect { it.isDirectory() ? it : zipTree(it) } }
	    manifest {
	        attributes 'Main-Class': 'net.tutorial.Calculator'
	    }
	}
	```

	Note that the contents of the above script are the same as the contents of `build.gradle` file that you completed in [Gradle's Unit Testing Tutorial](/gradle-unit-testing).  

	In this tutorial, entries related to web application (e.g., use of `.war` file) will be added to `build.gradle`.
	
	<br>
 
1. As mentioned, `calculator.jsp` is the `.jsp` version of `Calculator.java`.

	**Source code** of	`src/main/webapp/calculator.jsp`:
 
	```java
	<!DOCTYPE html>
	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<%@ page import="net.tutorial.Math" %>
	<html>
	<head>
	    <title>Calculator</title>
	</head>
	<body>
	<%
	Math m = new Math();
	%>
	
	<%="5 + 9 = " + m.add(5, 9)%>
	<br>
	<%="8 - 2 = " + m.sub(8, 2)%>
	<br>
	<%="4 x 7 = " + m.multiply(4, 7)%>
	<br>
	
	</body>
	</html>
	```

	Note that similar to `Calculator.java`, `calculator.jsp` uses `Math.java`:
	
	```java
	<%@ page import="net.tutorial.Math" %>
	```

####Update Build script to support Web Application
	
1. Update `build.gradle` to include `.war` related entries:

	```text
	apply plugin: 'java'
	
	apply plugin: 'war'
	
	war {
		archiveName 'calcuapp.war'
	}
	
	repositories {
	    mavenCentral()
	}
	 
	dependencies {
	    compile 'log4j:log4j:1.2.17'
	    testCompile 'junit:junit:4.12'
	}
	
	jar {
		from { configurations.compile.collect { it.isDirectory() ? it : zipTree(it) } }
	    manifest {
	        attributes 'Main-Class': 'net.tutorial.Calculator'
	    }
	}
	```

	Take note that the following were added in the script above:

	```text
	apply plugin: 'war'
	
	war {
		archiveName 'calcuapp.war'
	}
	```
	
	The `apply plugin: 'war'` entry will provide capability to Gradle to create a `war` file.

	The entry containing `archiveName 'calcuapp.war'` is added so that when the `.war` file is created, its name is `calcuapp.war`.  If you omit this entry, the name of the `.war` file is the name of the directory containing the project.  As an example, if `archiveName 'calcuapp.war'` is omitted, the `.war` file will be named `gradle-web-application.war`.

	<br>

1. Assemble the Gradle project.

	> Make sure that you are in the `gradle-web-application` directory before issuing the command below.

	```text
	> gradle assemble
	```

	**Output:**
		
	```text
	:compileJava
	:processResources
	:classes
	:war
	:assemble
	
	BUILD SUCCESSFUL
	
	Total time: 9.289 secs
	```
	
	The `build` directory and its subdirectories and files are created.

     	In this tutorial, the most important file that was created is the `build/libs/calcuapp.war` file.  This is the `.war` file containing the web application.


	<br>

####Deploy the Web Application in Jetty

1. Copy `calcuapp.war` to Jetty's `webapps` subdirectory.

	>Refer to [Jetty Basics Tutorial](/jetty-basics) if your machine do not have the Jetty web server.
	
1. Make sure that the Jetty web server is running.

	>To start the Jetty web server
	>- open another terminal window
	>- go to Jetty's home directory
	>- issue the command `java -jar start.jar`.

1. On a web browser, go to [`http://localhost:8080/calcuapp/calculator.jsp`](http://localhost:8080/calcuapp/calculator.jsp).

	**Output:**
		
	```text
	5 + 9 = 14
	8 - 2 = 6
	4 x 7 = 28 
	```

	You have successfully deployed the calculator application in Jetty.


	<br>

####Deploy the Web Application in Bluemix

1. Login to your Bluemix account using the `cf` tool.

	> Make sure that you are in the `gradle-web-application` directory before issuing the command below.

	```text
	> cf login -a https://api.ng.bluemix.net -s dev
	```

1. Upload the web application to your Bluemix account.

	```text
	> cf push calculator-<your_name> -p build/libs/calcuapp.war
	```

	**Example:**
		
	```text
	> cf push calculator-pong -p build/libs/calcuapp.war
	```

1. On a web browser, go to `http://calculator-<your_name>.mybluemix.net/calculator.jsp`.

	**Output:**
		
	```text
	5 + 9 = 14
	8 - 2 = 6
	4 x 7 = 28 
	```

	You have successfully deployed the calculator application in Bluemix.

	<br>

####End of Tutorial


####What's next?



