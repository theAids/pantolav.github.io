---
layout: post
title: Bluemix Devops Services Delivery Pipeline
permalink: /devops-delivery-pipeline/
---

##Application Development Tutorial

###Bluemix DevOps Services Delivery Pipeline
[Bluemix DevOps Services](https://hub.jazz.net) has a delivery pipeline that allows you to build, test, and deploy your web application.

In this tutorial you will learn to set-up a delivery pipeline by creating a build stage, a test stage, and a deploy stage.  The build stage will use Gradle.  The test stage will use JUnit through Gradle.  The deploy stage will use the cf tool.


>**Prerequisite:**

>You are **required** to do the [Creating a Web Application using Gradle Tutorial](/gradle-web-application).

>- The sample code used in the current tutorial is based from the sample code used in [Creating a Web Application using Gradle Tutorial](/gradle-web-application). 

>You are not required (but **recommended**) to do  the [Bluemix Basics Tutorial](/bluemix-basics).

>- **However**, ensure that you have a Bluemix account.  
>- Your account should have the space `dev` under the region `US-South`.  The creation of the space `dev` is discussed in [Bluemix Basics Tutorial](/bluemix-basics).

>You are not required (but **recommended**) to do  the [Bluemix DevOps Services Basics Tutorial](/devops-basics).

>- **However**, ensure that you have a Bluemix DevOps Services account.

>You are not required (but **recommended**) to do  the [GitHub Basics Tutorial](/github-basics).

>- **However**, ensure that you have a GitHub account.


<br>



####Copy a GitHub Repository


1. Open a web browser tab and login to [GitHub](https://github.com/).  In this tutorial, we will refer to this browser tab as `GITHUB TAB`.

1. Using the same web browser tab, go to the GitHub repository [`https://github.com/pong-pantola/devops-delivery-pipeline`](https://github.com/pong-pantola/devops-delivery-pipeline).

1. Fork the repository by clicking the `Fork` button. 

1. Verify that you have successfully forked the repository by checking its name:

	>**VERY IMPORTANT:**
	>In this tutorial, we will use the username `juandelacruz`.  Kindly use your username in place of `juandelacruz` when you do the tutorial.

	**Name of Repository:**

	```text		
	juandelacruz/devops-delivery-pipeline
	```

1. Take note of the URL of your Git repository:

	**Git URL**

	```text		
	https://github.com/juandelacruz/devops-delivery-pipeline.git
	```


####Create a Bluemix DevOps Project based on the GitHub Repository

1. Open another web browser tab and login to [Bluemix DevOps](https://hub.jazz.net/).  In this tutorial, we will refer to this browser tab as `DEVOPS TAB`.

1. Click `CREATE PROJECT`.

1. Name your project `devops-delivery-pipeline`.

1. Click `Link to an existing GitHub repository`.

	>If this is the first time you will link a Bluemix DevOps project to a GitHub repository, you will be asked to authorize your Bluemix DevOps account to access your GitHub account.  Proceed with confirming access.

1. Select the repository `juandelacruz/devops-delivery-pipeline`

1. Ensure the following options are chosen:

	||||
	|-|-|-|
	| **Private Project** | checked |
	| **Add features for Scrum development** | checked |
	| **Make this a Bluemix Project** | checked |
	| **Region** | IBM Bluemix US South |
	| **Organization** | you may leave the default selection |		
	| **Space** | dev |

	<br>
	
1. Click the `CREATE` button.  Wait for your project to be created.

1. Click the `EDIT CODE` button.  You will be redirected to Bluemix DevOps' editor.  In this tutorial, we will refer to this browser tab as `DEVOPS-EDITOR TAB`.

	The editor shows the working directory (and not the GitHub repository you forked earlier).  Since the Bluemix DevOps project is liked to the GitHub repository `juandelacruz/devops-delivery-pipeline`, the contents of the working directory is based from the repository. 

	However, notice that there are additional files/subdirectories (e.g., `.cfignore` and `launchConfigurations`) that were added in the working directory.  These were added automatically when the Bluemix DevOps project was created.  To sync the working directory with the GitHub repository `juandelacruz/devops-delivery-pipeline`, these files/directories need to be pushed to the repository.

1. On the `DEVOPS-EDITOR TAB`: Click (open in another browser tab) the `Git Repository` icon found on the left side of the screen.  We will refer to this browser tab as `DEVOPS-GIT TAB`.

1. On the `DEVOPS-GIT TAB`: Set the following values:

	||||
	|-|-|-|
	| **Select All** | checked |
	| **Commit message** | files created when Bluemix DevOps project was created |

	<br>

1. On the `DEVOPS-GIT TAB`: Click the `Commit` button.


1. On the `DEVOPS-GIT TAB`: Click the `Push` button.

	Your working directory and GitHub repository are now the same.

1. On the `GITHUB TAB`: Refresh the page and verify that `.cfignore` and `launchConfigurations` are added.

	You are now ready to create the delivery pipeline (i.e., build stage, test stage, deploy stage).
	
1. On the `DEVOPS-GIT TAB`: Click (open in another browser tab) the `BUILD & DEPLOY` button.  We will refer to this browser tab as `DEVOPS-DELIVERY-PIPELINE TAB`.





####Create a Build Stage

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: Click the `ADD STAGE` button.  Change `MyStage` to `Build Stage`.

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `INPUT` tab, set the following values:

	||||
	|-|-|-|
	| **Input Type** | SCM Repository |
	| **Git URL** | https://github.com/juandelacruz/devops-delivery-pipeline.git |
	| **Branch** | master |
	| **Stage Trigger** | Run jobs whenever a change is pushed to Git |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `JOB` tab, click the `ADD JOB` link and select `Build`.   Change `Build` to `Gradle Build`.  Set the following values:

	||||
	|-|-|-|
	| **Builder Type** | Gradle |		
	| **Build Shell Command** | `#!/bin/bash`<br>`gradle asemble`  |	
	| **Stop running this stage if this job fails** | checked |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click the `SAVE` button.

	<br>
	
####Create a Test Stage

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: Click the `ADD STAGE` button.  Change `MyStage` to `Test Stage`.

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `INPUT` tab, set the following values:

	||||
	|-|-|-|
	| **Input Type** | Build Artifacts |
	| **Git URL** | https://github.com/juandelacruz/devops-delivery-pipeline.git |
	| **Branch** | master |
	| **Stage Trigger** | Run jobs whenever a change is pushed to Git |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `JOB` tab, click the `ADD JOB` link and select `Build`.   Change `Build` to `Gradle Build`.  Set the following values:

	||||
	|-|-|-|
	| **Builder Type** | Gradle |		
	| **Build Shell Command** | `#!/bin/bash`<br>`gradle asemble`  |	
	| **Stop running this stage if this job fails** | checked |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click the `SAVE` button.

xxxxxxxxxxxxxxxxxxxxxxxx


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



