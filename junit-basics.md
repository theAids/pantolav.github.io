---
layout: post
title: Gradle's Unit Testing
permalink: /gradle-unit-testing/
---

##Application Development Tutorial

###Gradle's Unit Testing
Gradle's dependency management allows quick resolution of library dependency.

In this tutorial you will learn how to resolve library dependency using Gradle's dependency management.  In order to appreciate this feature of Gradle, you will first resolve the dependency problem using the manual approach.

>**Prerequisite:**

>It is **required** that you have performed the [Gradle Basics Tutorial](/gradle-basics).

<br>



####Copy Sample Codes from Git repository


1. Open a terminal window and create the directory `gradletemp` in the root directory.  Go to the created directory.

	```text		
	> mkdir gradletemp
	> cd gradletemp
	```

1. Clone the git repository `https://github.com/pong-pantola/gradle-unit-testing.git` and go to the created `gradle-unit-testing` directory.

	```text
	> git clone https://github.com/pong-pantola/gradle-dependency-management.git
	> cd gradle-unit-testing
	```
 
	The `gradle-unit-testing` directory has a subdirectory `src`.

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
	     |         |
	     |         |----log4j.properties        
	     |
	     |----test/
	     |    |
	     |    |----java/net/tutorial/
	                        |
	                        |----MyTest.java
	                        |----TestRunner.java	
	``` 

	`src` has subdirectories `main` and `test`. 

	`src/main` contains exactly the same files and subdirectories as the one in [Gradle's Dependency Management Tutorial](/gradle-dependency-management). 

	`src/test` contains exactly the same files and subidrectories as the one in [JUnit Basics Tutorial](/junit-basics).

	`gradle-unit-testing` has a Gradle build script `build.gradle`.  Its contents are those that were discussed in [Gradle's Dependency Management Tutorial](/gradle-dependency-management).  Specifically, it defines Maven as a repository as well as the Log4j library dependency.
 
	In this tutorial, `build.gradle`will be updated to include entries related to unit testing.

<br>


####Review the Java classes and Build script


1. `Math.java`  contains the methods `add`, `sub`, and `multiply`.   This is exactly the same file that was discussed in [JUnit Basics Tutorial](/junit-basics). 
 
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
	     |           |
	     |           |----java/net/tutorial/
	     |                        |
	     |                        |----Math.class
	     |                        |----Calculator.class
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

1. To compile the `.java` files under the subdirectory `src/test`, use the `testClasses' task

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

1. Run the `.jar` file.

	```text
	> java -jar build/libs/gradle-dependency-management.jar
	```

	**Output:**

	```text
	no main manifest attribute, in build/libs/gradle-dependency-management.jar
	```
	
	The `no main manifest attribute` error is encountered because you did not specify the entry point of your Java application.  The entry point is your `.class` file that contains the `main` method.  In this tutorial, the entry point is the `Calculator` class.

	<br>

1. Specify the entry point of your application in `build.gradle` by updating the file to the following:

	```text
	apply plugin: 'java'
	
	repositories {
	    mavenCentral()
	}
	
	dependencies {
	    compile 'log4j:log4j:1.2.17'
	}

	jar {
	    manifest {
	        attributes 'Main-Class': 'net.tutorial.Calculator'
	    }
	}
	```

1. Reassemble and try to run again the `.jar` file.

	```text
	> gradle assemble
	> java -jar build/libs/gradle-dependency-management.jar
	```

	**Output:**

	```text
	Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/log4j/Logger
	        at net.tutorial.Calculator.<clinit>(Calculator.java:7)
	Caused by: java.lang.ClassNotFoundException: org.apache.log4j.Logger
	        at java.net.URLClassLoader$1.run(Unknown Source)
	        at java.net.URLClassLoader$1.run(Unknown Source)
	        at java.security.AccessController.doPrivileged(Native Method)
	        at java.net.URLClassLoader.findClass(Unknown Source)
	        at java.lang.ClassLoader.loadClass(Unknown Source)
	        at sun.misc.Launcher$AppClassLoader.loadClass(Unknown Source)
	        at java.lang.ClassLoader.loadClass(Unknown Source)
	        ... 1 more
	```

	The error states that `org.apache.log4j.Logger` is not found.  This is due to the Log4j library is not included in `gradle-dependency-management.jar`.  

	<br>
	
1. Inspect the contents of the `.jar` file to verify that the Log4j library is not included:

	```text
	> jar tf build/libs/gradle-dependency-management.jar
	```

	>If you have an archiving software like `7Zip`, you may use this instead to inspect the contents of the `.jar` file.

	`gradle-dependency-management.jar` contains the following:

	```text
	gradle-dependency-management.jar
	|
	|----META-INF/
	|    |
	|    |----MANIFEST.MF
	|
	|----net/tutorial/
	|        |
	|        |----Calculator.class
	|        |----Math.class
	|
	|----log4j.properties
	``` 

	There are no classes related to Log4j that are included in the `.jar` file.

1. Specify that Log4j library should be included in the `.jar` file by updating the `build.gradle` file to the following:

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

1. Reassemble and try to run again the `.jar` file.

	```text
	> gradle assemble
	> java -jar build/libs/gradle-dependency-management.jar
	```

	**Output:**

	```text
	5 + 9 = -4
	8 - 2 = 6
	4 x 7 = 28
	INFO  - Calculator                 - Calculation completed.
	```

	As expected, the Java application executed successfully.  

	The Log4j library is included in `gradle-dependency-management.jar`.  

	<br>
	
1. Verify that the Log4j library is already included in the  `.jar` file:

	```text
	> jar tf build/libs/gradle-dependency-management.jar
	```

	`gradle-dependency-management.jar` now contains the following:

	```text

	gradle-dependency-management.jar
	|
	|----META-INF/
	|    |
	|    |----MANIFEST.MF
	|
	|----net/tutorial/
	|        |
	|        |----Calculator.class
	|        |----Math.class
	|
	|----org/apache/log4j/...
	|
	|----log4j.properties
	``` 

	The subfolder `org/apache/log4j/` and additional subfolders and files are included in `gradle-dependency-management.jar`.

<br>

####End of Tutorial


####What's next?

