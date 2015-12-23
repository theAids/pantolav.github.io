---
layout: post
title: Gradle Dependency Management
permalink: /gradle-dependency-management/
---

##Application Development Tutorial

###Gradle's Dependency Management
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

1. Clone the git repository `https://github.com/pong-pantola/gradle-dependency-management.git` and go to the created `gradle-dependency-management` directory.

	```text
	> git clone https://github.com/pong-pantola/gradle-dependency-management.git
	> cd gradle-dependency-management
	```
 
	The `gradle-dependency-management` directory has two subdirectories: `src` and `build`.

	```text
	gradle-dependency-management/
	|
	|----src/main/
	|        |
	|        |----java/net/tutorial/
	|        |             |
	|        |             |----Math.java
	|        |             |----Calculator.java
	|        |
	|        |----resources/
	|             |
	|             |----log4j.properties        
	|
	|----build/
	     |
	     |----classes/
	     |----libs/
	``` 

	`src` has a subdirectory `main`. 

	`src/main` contains the Java class `src/main/java/net/tutorial/Math.java` which contains methods performing mathematical functions (i.e., add, sub, mulitiply).  In addition, it contains the `src/main/java/net/tutorial/Calculator.java` which is a sample Java application that uses `Math.java`. 

	`src/main` also has a subdirectory `resources`.  Any resource (e.g., configuration or properties file) needed by the Java classes can be placed here.  For this tutorial, the subdirectory contains the log4j.properties file.  It is not essential in this tutorial to know the purpose of this file aside from it is used by one of the Java classes.
 
	`build` has two subdirectories: `classes` and `libs`. 

	`build/classes` is used to hold the the `.class` files that will be created later when you compile your `.java` files.

	`build/libs` is used for the libraries (i.e, `.jar` files) that you will download later.  These libraries are needed to compile the Java classes later.

<br>


####Examine the Java classes


1. `Math.java`  contains the methods `add`, `sub`, and `multiply`.   This is exactly the same file that was discussed in [JUnit Basics Tutorial](/junit-basics). 
 
	Since the focus of this tutorial is on Gradle's dependency management, you don't need to analyze at this point the contents of `Math.java`.  This class was discussed in the [JUnit Basics Tutorial](/junit-basics) and will be revisited in [Gradle's Test Task Tutorial](/gradle-test-task).
 
	 >Note that the method `add` has a logical error (i.e., instead of `a+b`;, the return statement is `a-b;`).  This error will be discussed further in the [Gradle's Test Task Tutorial](/gradle-test-task).  No need to fix this error at this point.

	>In addition, a delay is inserted in the method `multiply`.  This will also be discussed in the [Gradle's Test Task Tutorial](/gradle-test-task).
	
	<br>

1. `Calculator.java` is a Java application that uses `Math.java`.

	**Source code** of	`src/main/java/net/tutorial/Calculator.java`:
 
	```java
	package net.tutorial;
	
	import org.apache.log4j.Logger;
	
	public class Calculator{
		
	  private static final Logger LOGGER = Logger.getLogger(Calculator.class);
	
	  public static void main (String args[]){
	    Math m = new Math();
		
		System.out.println("5 + 9 = " + m.add(5, 9));
		
		System.out.println("8 - 2 = " + m.sub(8, 2));	
		
		System.out.println("4 x 7 = " + m.multiply(4, 7));
		
		LOGGER.info("Calculation completed.");
	  }
	}
	```

	`Calculator.java` is very similar to the one discussed in [JUnit Basics Tutorial](/junit-basics).  However, in this tutorials these three extra lines are included:

	```java
	import org.apache.log4j.Logger;
	```


	```java
	  private static final Logger LOGGER = Logger.getLogger(Calculator.class);
	```
	```java
		LOGGER.info("Calculation completed.");
	```

	Log4j is a Java-based logging library.  For this tutorial, you don't need to know the details regarding this library.  The three Log4j-related lines above are included in `Calculator.java` just to demonstrate dependency resolution.  When `Calculator.java` is compiled later, you will encounter an error due to Log4j library dependency.  You will resolve this dependency using the manual approach as well as Gradle's dependency management.

	>It was mentioned earlier that the `src/main` directory has a subdirectory `resources` that contains the file `log4j.properties`.  This file is needed by Log4j.  However, in this tutorial it is not important to examine the contents of the file.  	

	<br>
 

####Review the Java classes and Build script


1. `Math.java`  contains the methods `add`, `sub`, and `multiply`.   This is exactly the same file that was discussed in [JUnit Basics Tutorial](/junit-basics). 
 
	 >Remember that the method `add` has a logical error (i.e., instead of `a+b`;, the return statement is `a-b;`). 

	>In addition, a delay is inserted in the method `multiply` to demonstrate the timeout mechanism of JUnit.
	
	<br>

1. `Calculator.java` is exactly the same file that was discussed in [Gradle's Dependency Management Tutorial](/gradle-dependency-management).   It is a Java application that uses `Math.java`.  

1. `build.gradle` has the same content as the one you created in [Gradle's Dependency Management Tutorial](/gradle-dependency-management).

	Please review the text below for the current contents of `build.gradle`:

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

	Note that the contents are related to repositories and dependencies since the `build.gradle` file was used in the [Gradle's Dependency Management Tutorial](/gradle-dependency-management).  

	In this tutorial, entries related to testing will be added to `build.gradle'.
	
	<br>
 
1. Compile `Math.java` and `Calculator.java`.

	> Make sure that you are in the `gradle-dependency-management` directory before issuing the command below.
 
	```text
	> javac -d build/classes/main src/main/java/net/tutorial/*.java
	```


	**Output:**

	```text
	src\main\java\net\tutorial\Calculator.java:3: error: package org.apache.log4j does not exist
	import org.apache.log4j.Logger;
	                       ^
	src\main\java\net\tutorial\Calculator.java:7: error: cannot find symbol
	  private static final Logger LOGGER = Logger.getLogger(Calculator.class);
	                       ^
	  symbol:   class Logger
	  location: class Calculator
	src\main\java\net\tutorial\Calculator.java:7: error: cannot find symbol
	  private static final Logger LOGGER = Logger.getLogger(Calculator.class);
	                                       ^
	  symbol:   variable Logger
	  location: class Calculator
	```

	As expected, a compile-time error is encountered due to the dependency to the Log4j library.   You will solve this problem by resolving the Log4j library dependency.  

	<br>
	
####Manually Resolve the Library Dependency Problem

> In order to appreciate Gradle, let's try resolving library dependency without using Gradle.  After this, you will use Gradle's dependency management to see how dependency resolution becomes simple with the use of Gradle.

1. Go to [http://logging.apache.org/log4j/2.x/download.html]([http://logging.apache.org/log4j/2.x/download.html]).
 
	>Just in case the URL is broken,  you may go to [Apache Log4j](http://logging.apache.org/log4j) and find the download link.
 
1. Download the latest version of Apache Log4j library (i.e., apache-log4j-x.x-bin.zip OR apache-log4j-x.x-bin.tar.gz) in a temporary directory.

1. Extract the contents of the zip (or gz) file.  You will see several Log4j `.jar` files.  Copy all the `.jar` files in the subdirectory `build/libs`.

	>Note that for the compilation to work, you don't need to copy all the `.jar` files.  Only one to three `.jar` files are needed.  However, since it may take sometime to identify which `.jar` files are needed, it is quicker just to copy all the files.  This is one problem with manually resolving dependencies.  You need to identify the necessary `.jar` files.

	<br>

1. Compile again `Math.java` and `Calculator.java`.

	> Note that the command below includes the `-classpath build/libs/*` option.  This is needed so that the compilation will use the `.jar` files that you copied earlier.
 
	```text
	> javac -classpath build/libs/* -d build/classes/main src/main/java/net/tutorial/*.java
	```

	The compilation is successful.  The library dependency is manually resolved.  

	>Take note that in manual library dependency resolution you need to know:

	> a. where to download the library

	> b. which `.jar` files are needed
		

1. Run the `Calculator` application.

	> Note that the command below includes the `-Dlog4j.configurationFile=file:src//main/resources/log4j.properties` option.  This is needed so that Log4j knows the location of its `.properties` file.

	```text
	> java -classpath build/libs/*;build/classes/main -Dlog4j.configurationFile=file:src//main/resources/log4j.properties net/tutorial/Calculator
	```

	**Output:**

	```text
	5 + 9 = -4
	8 - 2 = 6
	4 x 7 = 28
	INFO  - Calculator                 - Calculation completed.
	```

	As mentioned earlier the method `sum` has a logical error (i.e., sum should be 14 and not -4).  In addition, the method `multiply` has a delay that is why you have observed a 3 secs. delay before `4 x 7 = 28` appeared.  The logical error and the delay are not important in this tutorial.  These will be useful in [Gradle's Test Task Tutorial](/gradle-test-task).

	At this point, what is important to note is the compilation (and execution) became successful due to the manual dependency resolution.
 
<br>

####Resolve the Library Dependency Problem using Gradle's Dependency Management

> One of the features of Gradle is dependency management.  By just specifying the libraries needed in a Gradle build script file (`build.gradle`), dependency resolution becomes faster.

1. To undo the files created during the manual dependency resolution (e.g., the `.jar` files you copied earlier), delete the entire `build` subdirectory.

	>In Gradle, the `build` subdirectory and its subdirectories (e.g., `classes`) need not exist for compilation to work.

1. In the `gradle-dependency-management` directory, create a text file with a filename `build.gradle`.

1. Place the following line in `build.gradle`:

	```text
	apply plugin: 'java'
	```

	The line `apply plugin: 'java'` adds Java compilation along with testing and bundling capabilities in the Gradle project.

	At this point, Gradle's dependency management is not yet utilized in `build.gradle`.  Let's try to compile the `.java` files using Gradle and see what errors will be produced.

1. To compile the `.java` files using Gradle's `assemble` task:

	> Make sure that you are in the `gradle-dependency-management` directory before issuing the command below.
 
	```text
	> gradle assemble
	```

	**Output:**

	```text
	:compileJava
	/gradle-dependency-management/src/main/java/net/tutorial/Calculator.java:3: error: package org.apache.log4j does not exist
	import org.apache.log4j.Logger;
			       ^
	/gradle-dependency-management/src/main/java/net/tutorial/Calculator.java:7: error: cannot find symbol
	  private static final Logger LOGGER = Logger.getLogger(Calculator.class);
			       ^
	  symbol:   class Logger
	  location: class Calculator
	/gradle-dependency-management/src/main/java/net/tutorial/Calculator.java:7: error: cannot find symbol
	  private static final Logger LOGGER = Logger.getLogger(Calculator.class);
					       ^
	  symbol:   variable Logger
	  location: class Calculator
	3 errors
	:compileJava FAILED

	FAILURE: Build failed with an exception.

	* What went wrong:
	Execution failed for task ':compileJava'.
	> Compilation failed; see the compiler error output for details.

	* Try:
	Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

	BUILD FAILED

	Total time: 4.751 secs
	```

	As expected compilation error due to Log4j dependency is encountered.  Let's fix this problem by utlizing the dependency management of Gradle in `build.gradle`.

	<br>

1. Specify the library repository to be used in `build.gradle` by updating the file to the following:

	```text
	apply plugin: 'java'

	repositories {
	    mavenCentral()
	}
	```

	Repositories are dependency containers.  It has a collection of libraries that your code may need.  You may specify zero or more repositories in a `build.gradle` file.

	In this tutorial, you need one repository to resolve the dependency problem caused by the use of the Log4j library.

	Repositories can be local (e.g., a local directory in your hard drive) or remote (e.g., Maven or Ivy repository).  This tutorial does not cover the details on how to use local repositories as well as the difference between Maven and Ivy repositories.  You may check [Gradle's website](https://docs.gradle.org/current/userguide/artifact_dependencies_tutorial.html). for additional information on repositores.

	In `build.gradle`, you added the following lines earlier:

	```text
	repositories {
	    mavenCentral()
	}
	```

	This means that you will be using Maven's central repository to get the necessary library.

	<br>

1. Specify the needed library in `build.gradle` by updating the file to the following:

	```text
	apply plugin: 'java'
	
	repositories {
	    mavenCentral()
	}
	
	dependencies {
	    compile 'log4j:log4j:1.2.17'
	}
	```

	The entry `compile 'log4j:log4j:1.2.17'` means that the indicated library (`'log4j:log4j:1.2.17'`) is needed during compilation. 

	`'log4j:log4j:1.2.17'` uses the following format: `'group:name:version'`.

	To know the group, name, and version of the library you may go to [Maven Central Repository](http://search.maven.org/) and search for the library you need.  

	As an example, you may go to the [Maven Central Repository](http://search.maven.org/).  In the search box, type `log4j`.  The search result will be a table with its first 3 columns labeled as: `GroupId`, `ArtifiactId`, and `Latest Version`, which is basically the `group`, `name`, and `version` mentioned in the format above.

	You may choose the most appropriate row in the search result.  For this tutorial, the row that was selected is the one with the following values:

	GroupId | ArtifactId | Latest Vesion
	-|-|-
	log4j | log4j | 1.2.17

	This is the reason why in `build.gradle` the dependency is specified as `compile 'log4j:log4j:1.2.17'`.

	>Note that when you try this tutorial the available library in Maven may  have changed.  Adjust the entry in `build.gradle` if necessary (e.g., version higher than `1.2.17` may be available already).

	<br>

1. Compile again the `.java` files using Gradle's `assemble` task.

	```text
	> gradle assemble
	```

	**Output:**

	```text
	:compileJava
	:processResources
	:classes
	:jar
	:assemble
	
	BUILD SUCCESSFUL
	
	Total time: 8.029 secs
	```

	Since dependency management of Gradle is already specified in `build.gradle`, the compilation error encountered earlier is resolved.
	
	Notice that the subdirectory `build` is automatically created.  Below are some of the subdirectories and files that are inside `build`.

	```text
	gradle-dependency-management/
	|
	|----build/
	     |
	     |----classes/main
	     |           |
	     |           |----net/tutorial/
	     |                    |
	     |                    |----Math.class
	     |                    |----Calculator.class
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

	More importantly, the `assemble` task generated the `gradle-dependency-management.jar` under the `libs` subdirectory.  This `.jar` file will be executed later.  In addition, the log4j.properties file is copied inside a subdirectory of `build`.  This is needed since the application uses the Log4j library.

	>Instead of using the `assemble` task (i.e., by issuing the command `gradle assemble`) you may use the `classes` task (i.e., by issuing the command `gradle classes`) to compile the `.java` followed by the `jar` task (i.e., by issuing the command `gradle jar`) .

	>In other words, `assemble` task = `classes` task + `jar` task.

	<br>
	
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

