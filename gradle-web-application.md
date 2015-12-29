---
layout: post
title: Gradle Web Application
permalink: /gradle-web-application/
---
jetty tutorial:
-Dorg.apache.jasper.compiler.disablejsr199=true

##Application Development Tutorial

###Creating a Web Application using Gradle
Gradle has an available `war` plugin that allows you to package your Java web application into a `.war` file.

In this tutorial you will learn how to package into `.war` file a Java web application.  The application will be deployed twice: locally (through Jetty) and remotely (through [IBM Bluemix](https://ibm.biz/bluemixph)).

>**Prerequisite:**

>You are **required** to do the [Gradle's Unit Testing Tutorial](/gradle-unit-testing).

>- The sample code used in the current tutorial is based from the sample code used in [Gradle's Unit Testing Tutorial](/gradle-unit-testing). 

>You are not required (but **recommended**) to do the [Jetty Basics Tutorial](/jetty-basics).

>- **However**, ensure that your machine has the Jetty web server set-up as discussed in [Jetty Basics Tutorial](/jetty-basics).

>You are not required (but **recommended**) to do  the [Bluemix Basics Tutorial](/bluemix-basics).

>- **However**, ensure that you have a Bluemix account.  Your account should have the space `dev` under the region `US-South`.  The creation of the space `dev` is discussed in [Bluemix Basics Tutorial](/bluemix-basics).





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
	gradle-unit-testing/
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

####Review the Java classes and Build script


1. As mentioned, `Math.java`  logical errors in [Gradle's Unit Testing Tutorial](/gradle-unit-testing) are already corrected.
 
	 Remember that the method `add` has a logical error (i.e., instead of `a+b`;, the return statement is `a-b;`). 

	In addition, a delay is inserted in the method `multiply` to demonstrate the timeout mechanism of JUnit.
	
	<br>

1. `Calculator.java` is exactly the same file that was discussed in [Gradle's Dependency Management Tutorial](/gradle-dependency-management).   It is a Java application that uses `Math.java`.  

	Recall that `Caculator.java` uses the Log4j library:
	
	```test
	import org.apache.log4j.Logger;
	```

1. `build.gradle` has the same content as the one you created in [Gradle's Dependency Management Tutorial](/gradle-dependency-management).

	Please review the text below for the current content of `build.gradle`:

	```text
	apply plugin: 'java'
	
	repositories {
	    mavenCentral()
	}
	 
	dependencies {
	    compile 'log4j:log4j:1.2.17'
	}
	
	jar {
		from { configurations.compile.collect { it.isDirectory() ? it : zipTree(it) } }
	    manifest {
	        attributes 'Main-Class': 'net.tutorial.Calculator'
	    }
	}
	```

	Note that the contents are related to repositories and dependencies related Log4j library since the `build.gradle` file was used in the [Gradle's Dependency Management Tutorial](/gradle-dependency-management).  

	In this tutorial, entries related to testing will be added to `build.gradle`.
	
	<br>
 
1. `MyTest.java` is exactly the same file that was discussed in [JUnit Basics Tutorial](/junit-basics).   It serve as the test class to test the methods of `Math.java`.

	Remember that `MyTest.java` uses the JUnit library:

	```text
	import static org.junit.Assert.assertEquals;
	import org.junit.Before;
	import org.junit.Test;
	```

	You will observe later if the use of the JUnit libary has an effect in Gradle later.


1. `TestRunner.java` is an application that runs the test methods found in `MyTest.java`.  

	Like `MyTest.java`, `TestRunner.java` uses the JUnit library:

	```text
	import org.junit.runner.JUnitCore;
	import org.junit.runner.Result;
	import org.junit.runner.notification.Failure;
	```


1. Compile `.java` files using Gradle's `assemble` task.


	```text
	> gradle assemble
	```

	>Recall that in the [Gradle's Dependency Management Tutorial](/gradle-dependency-management), it was mentioned that instead of `assemble` task, you may use the `classes` task (i.e., `gradle classes`) to compile the `.java` classes followed by the `jar` task (`gradle jar`)  to create the `.jar` file.

	>`assemble task` = `classes` task + `jar` task


	**Output:**

	```text
	:compileJava
	:processResources
	:classes
	:jar
	:assemble
	
	BUILD SUCCESSFUL
	
	Total time: 8.285 secs
	```

	As expected, compilation is successful.
	
	The subdirectory `build` is automatically created.  Below are some of the subdirectories and files that are inside `build`.

	```text
	gradle-unit-testing/
	|
	|----build/
	     |
	     |----classes/main
	     |            |
	     |            |----net/tutorial/
	     |                     |
	     |                     |----Math.class
	     |                     |----Calculator.class
	     |
	     |----libs/
	     |    |
	     |    |----gradle-dependency-management.jar
	     |
	     |----resources/main/
	                    |
	                    |----log4j.properties
	``` 

	Notice that the `Math.class` and `Calculator.class` were created after the `assemble` task.  

	However the corresponding `.class` files of `MyTest.java` and `TestRunner.java` are not created.

	When the `assemble` task is used, only the files under the subdirectory `src/main` are considered.  Both `Math.java` and `Calculator.java` are under this subdirectory.

	However, `MyTest.java` and `TestRunner.java` are under the subdirectory `src/test`.  Therefore, these files are excluded from the `assemble` task.
	<br>

####Compile classes under the Subdirectory `src/test`

1. To compile the `.java` files under the subdirectory `src/test`, use the `testClasses' task.

	>Make sure that you are in the gradle-unit-testing directory before issuing the command below.

	```text
	> gradle testClasses
	```

	**Output:**

	```text
	/gradletemp/experiment/gradle-unit-testing/src/test/java/net/tutorial/MyTest.java:3: error: package org.junit does not exist
	import static org.junit.Assert.assertEquals;
	                       ^
	
	TUTORIAL NOTE: Other lines in this error message are omitted
	
	/gradletemp/experiment/gradle-unit-testing/src/test/java/net/tutorial/TestRunner.java:3: error: package org.junit.runner does not exist
	import org.junit.runner.JUnitCore;
	                       ^
	
	TUTORIAL NOTE: Other lines in this error message are omitted
	
	/gradletemp/experiment/gradle-unit-testing/src/test/java/net/tutorial/MyTest.java:10: error: cannot find symbol
	  @Before
	   ^
	  symbol:   class Before
	  location: class MyTest
	
	TUTORIAL NOTE: Other lines in this error message are omitted
	
	/gradletemp/experiment/gradle-unit-testing/src/test/java/net/tutorial/TestRunner.java:9: error: cannot find symbol
	    Result result = JUnitCore.runClasses(MyTest.class);
	    ^
	  symbol:   class Result
	  location: class TestRunner
	
	TUTORIAL NOTE: Other lines in this error message are omitted
	
	17 errors
	:compileTestJava FAILED
	
	FAILURE: Build failed with an exception.
	
	* What went wrong:
	Execution failed for task ':compileTestJava'.
	> Compilation failed; see the compiler error output for details.
	
	* Try:
	Run with --stacktrace option to get the stack trace. Run with --info or --debug
	option to get more log output.
	
	BUILD FAILED
	
	Total time: 6.224 secs
	```
	
	A compilation error due to JUnit dependency is encountered. Let's fix this problem by utlizing the dependency management you learned in [Gradle's Dependency Management Tutorial](/gradle-dependency-management).

	<br>
	
1. Specify the JUnit library in build.gradle by adding the `testCompile 'junit:junit:4.11'` line as shown below:

	```
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

	Take note that the `dependencies` section of `build.gradle` is updated to the following:

	```text
	dependencies {
	    compile 'log4j:log4j:1.2.17'
	    testCompile 'junit:junit:4.12'
		}
	```
	
	and NOT to the following approach:

	```text
	dependencies {
	    compile 'log4j:log4j:1.2.17', 'junit:junit:4.12'
		}
	```
	If the latter approach is used (i.e., `compile 'log4j:log4j:1.2.17', 'junit:junit:4.12'`), the JUnit library becomes available to `Math.java` and `Calculator.java` (i.e., files in the subdirectory `src/main`).  Since JUnit is not used by `Math.java` and `Calculator.java`, the `testCompile 'junit:junit:4.12'` is better.

	In this tutorial, we used `'junit:junit:4.12'`.  In [Gradle's Dependency Management Tutorial](/gradle-dependency-management), it was discusssed that the dependency entry uses the following format: `'group:name:version'`.

	To know how `'junit:junit:4.12'` was derived, you may go to the [Maven Central Repository](http://search.maven.org/).  In the search box, type `junit`.  For this tutorial, the row that was selected is the one with the following values:

	GroupId | ArtifactId | Latest Vesion
	-|-|-
	junit | junit | 4.12

	<br>

1. Compile again the `.java` files under the subdirectory `src/test`, use the `testClasses' task.

	```text
	> gradle testClasses
	```

	**Output:**
	
	```text
	:compileJava UP-TO-DATE
	:processResources UP-TO-DATE
	:classes UP-TO-DATE
	:compileTestJava
	Download https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.pom
	Download https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.jar
	:processTestResources UP-TO-DATE
	:testClasses
	
	BUILD SUCCESSFUL
	
	Total time: 1 mins 38.426 secs
	```
	
	As expected, the compilation error due to JUnit dependency is resolved.

	The subdirectory `build/classes` now contains the subdirectory `test`:

	```text
	gradle-unit-testing/
	|
	|----build/classes/
	           |
	           |----main/ 
	           |    |
	           |    |----net/tutorial/
	           |         |
	           |         |----Math.class
	           |         |----Calculator.class
	           |
	           |----test/ 
	                |
	                |----net/tutorial/
	                     |
	                     |----MyTest.class
	                     |----TestRunner.class	     
	``` 


	<br>

####Run the Test

1. To run the test, use the `test' task.

	>Make sure that you are in the gradle-unit-testing directory before issuing the command below.

	```text
	> gradle test
	```

	**Output:**
	
	```text
	:compileJava UP-TO-DATE
	:processResources UP-TO-DATE
	:classes UP-TO-DATE
	:compileTestJava UP-TO-DATE
	:processTestResources UP-TO-DATE
	:testClasses UP-TO-DATE
	:test
	
	net.tutorial.MyTest > multiplyShouldReturnProduct FAILED
	    org.junit.runners.model.TestTimedOutException at MyTest.java:27
	
	net.tutorial.MyTest > addShouldReturnSum FAILED
	    java.lang.AssertionError at MyTest.java:17
	
	3 tests completed, 2 failed
	:test FAILED
	
	FAILURE: Build failed with an exception.
	
	* What went wrong:
	Execution failed for task ':test'.
	> There were failing tests. See the report at: file:///D:/gradletemp/experiment/gradle-unit-testing/build/reports/tests/index.html
	
	* Try:
	Run with --stacktrace option to get the stack trace. Run with --info o option to get more log output.
	
	BUILD FAILED
	
	Total time: 7.645 secs
	```
	
	As expected, the `multiplyShouldReturnProduct` test method resulted to an error since the `multiply` method of `Math.java` has a call to the `delay` method which produces a 3 sec. delay.  This is way longer than the 1 sec. timeout that is indicated in the annotation in the test method (i.e., `@Test(timeout=1000)`).

	In addition, the `addShouldReturnSum` test method also failed since we intentionally made the `sum` method of `Math.java` incorrect.  Recall that we used `return a-b;` instead of `return a+b;` in the `sum` method of `Math.java`.

	<br>
	
1. Fix the error in `Math.java` by updating the `add` and `multiply` methods.

	Change the `add` method by changing `a-b` to `a+b`:

	```java
	  public int add(int a, int b){
	    return a+b;
	  }
	```

	Change the `multiply` method by commenting out `delay()`:

	```java
	  public int multiply(int a, int b){
	    //added to simulate that this method is 
	    //taking too long to execute	
	    //delay();
	
	    return a*b;
	  }
	```

	<br>
	
1. Run the test again.

	```text
	> gradle test
	```

	**Output:**
		
	```text
	:compileJava
	:processResources UP-TO-DATE
	:classes
	:compileTestJava
	:processTestResources UP-TO-DATE
	:testClasses
	:test
	
	BUILD SUCCESSFUL
	
	Total time: 7.322 secs
	```

	The errors are now fixed.

<br>

####End of Tutorial


####What's next?


