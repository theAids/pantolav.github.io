---
layout: post
title: Bluemix Basics
permalink: /bluemix-basics/
---

##Application Development Tutorial

###Bluemix Basics
IBM [Bluemix](https://ibm.biz/bluemixph) is a platform as a service (PaaS) cloud technology of IBM.  You may develop applications and deploy them in Bluemix.

In this tutorial you will learn how to deploy a sample JSP application in Bluemix.  In addition, you will also learn how to create a PostgreSQL database service that will be used by the sample application.

>**Prerequisite:**
xxxx
>Having a good understanding of Java programming is required to do this tutorial.

<br>

####Create a Bluemix Account
1. Go to [Bluemix](https://ibm.biz/bluemixph) and click the `SIGN UP` button.

1. Fill-up and submit the registration form.

1. Wait for a confirmation e-mail and follow the instructions in the e-mail to validate your Bluemix registration.

<br>

####Explore your Bluemix Account

1. Open a web browser and login to your [Bluemix](https://ibm.biz/bluemixph) account.

1. You will be redirected to your dashboard.  Your dashboard:
	- provides some information regarding your account (e.g., organization, spaces, etc.)
	- summarizes the amount of resources you have consumed 
	- enumerates the applications, services, containers, and virtual machines you have created.

1. You may have one ore more organizations.  By default, you only have one organization.  The name of this organization is the same as your Bluemix account.

	Additional organizations may become available in your Bluemix account when other Bluemix users share his/her organization to you.  The procedure in sharing an organization is not covered in this tutorial.
	
1. Each organization has an allotted set of resources.  As an example, your dashboard shows the following:

	Resource | Consumed | Total Allocation
	-|-|-
	Cloud Foundry Apps | 0GB | 2GB
	Services and APIs | 0 | 10

	Note that there are other resources in your dashboard not listed above.

	Since you have a total allocation of 2GB for Cloud Foundry apps, it means that you can have one or more running applications in your account that has a total memory consumption of 2GB.  As an example, if you deploy Application A in Bluemix with a memory allocation of 1GB, you still have 1GB left that can be used for another application (or applications).

	For services and APIs, you are given an allocation of 10.  An example of a service is a PostgreSQL database service which you will create later. 

1. The physical location (e.g., location of the data center) of the Bluemix servers that will host your application is referred to as Bluemix regions.  Currently there are three Bluemix regions: `United Kingdom`, `Sydney,` and `US South`.

	In this tutorial, you will be using the `US South` region.  However, in an actual deployment, you may choose any of the available regions.  If you will be deploying several applications, these applications may be deployed in different region (e.g., your first application is in `United Kingdom`, while the second one is in `US South`).  However, the total consumed resources in these regions should not exceed the allocation of your organization.

1. Make sure that the current region used by your Bluemix account is `US South` by  clicking the `Person` icon on the upper-right corner of your account.  

	>Once you select `US South` it is possible that you will be prompted to create a space.  If you are prompted, enter a space named `dev`.  This purpose of a space is explained in the next step.

1. Under the `US South` region, you may deploy one ore more applications.  As the number of applications you deploy increases, the harder it is to manage your applications.  To help manage them, applications are grouped together in spaces.  As an example, you may create spaces which groups applications belonging to the same phase.  For example, you may create a space called `dev` and deploy all of your applications that are still under development under this phase.  You may create a second space called `prod` for applications that are already in production.

	Another way to utilize spaces is to group applications based on projects.  For example, you may create a space called `proj1` for all projecs belonging to project 1.

1. Verify if you have a `dev` space at the left side of your dashboard.  If there is no `dev` space, create one by clicking the `Create a Space` link.

<br>

####Explore the Bluemix Catalog

1. In the menu, click `CATALOG`.  The Bluemix Catalog shows the different services and APIs, as well as runtimes and containers that you may create.

1. Scroll down until you see `PostgreSQL by Compose`.  PostgreSQL is a type of relational database.  

	In Bluemix, there are services that are very similar.  As an example, in this tutorial, you will be creating a PostgreSQL service.  However, the PostgreSQL service that you will be using is NOT `PostgreSQL by Compose`.

1. Scroll down further in the `CATALOG` page until you see the `Bluemix Labs Catalog` link.  Click this link.

1. Scroll down until you see `postgresql`.   The `postgresql` service and the `PostgreSQL by Compose` service are very similar (i.e., both are PostgreSQL services).  In this tutorial, you will be using `postgresql`.  When you are asked to create a PostgreSQL service in the succeeding step, make sure to use `postgresql` and not `PostgreSQL by Compose`.

1. Leave your Bluemix account open on the browser.  You will use this again later.
	<br>

###Install the Cloud Foundry (`cf`) tool.

Cloud Foundry is an open-source platform as a service cloud technology.  Bluemix is based from Cloud Foundry.  Cloud Foundry has a command-line tool called `cf` that is used to deploy applications in cloud foundry-based environment.  Since Bluemix is based on Cloud Foundry, the same `cf` tool is used to deploy your application in Bluemix.


1. Go to the Cloud Foundry `cf` tool [repository](https://github.com/cloudfoundry/cli/releases).

1. Download the appropriate installer (not the binary).

1. Install using the default options.

1. To test if installation is successful, open a terminal window and issue the following command:

	```text		
	> cf
	```

	**Output:**
		
	```
	NAME:
	   cf - A command line tool to interact with Cloud Foundry
	
	USAGE:
	   [environment variables] cf [global options] command [arguments...] [command options]
	   :
	   :
	```
	
	You should see the help screen of `cf` (see above sample).


####Copy Sample Application
You will download a copy of a sample application that you will deploy in your Bluemix account.


1. Create the directory `bluemixtemp` in the root directory.  Create a subdirectory `myfirstapp` in `bluemixtemp`.

1. Download [PostgreSQLUpload.war](https://github.com/ibmjstart/bluemix-java-postgresql-uploader/releases/download/v1.1/PostgreSQLUpload.war) and save it in the `myfirstapp` subdirectory.

<br>


####Deploy Sample Application in Bluemix using the `cf` tool.

1. Open a terminal window and go to the `myfirstapp` subdirectory.

1. Login to your Bluemix account using the `cf` tool.

	```text
	> cf login -a https://api.ng.bluemix.net -s dev
	```
	
	>When asked for a username (e-mail address) and password, enter the username and password of your Bluemix account.

	**Output:**

	```
	API endpoint: https://api.ng.bluemix.net
	
	Username> -----
	
	Password>
	Authenticating...
	OK
	
	Targeted space dev
	
	API endpoint: https://api.ng.bluemix.net (API version: 2.40.0)
	User:         -----
	Org:          -----
	Space:        dev
	```
	
	The `-a` switch allows you to specify the URL of the Bluemix region.  In this tutorial, you are using the `US South` region.  The URL of this region is `https://api.ng.bluemix.net`.

	The `-s` switch allows you to specify the space where you will deploy the application.  In this tutorial, you will be deploying the application in the `dev` space you created earlier.

1. Upload the sample application to your Bluemix account.

	```text
	> cf push myfirstapp-<your_name> -m 256M -p PostgreSQLUpload.war
	```

	**Example:**
		
	```text
	> cf push myfirstapp-pong -m 256M -p PostgreSQLUpload.war
	```

	**Output:**
		
	```text
	:
	:
	     state     since                    cpu    memory           disk
	#0   running   2016-01-14 08:00:15 AM   0.8%   185.1M of 256M   180.9M	
	```

	In the example above, the name of the application is `myfirstapp-pong`.  Replace `pong` with your name.  The name of your application will be concatenated with string `.mybluemix.net` and this will be the URL of your application.  As an example, the application `myfirstapp-pong` is accessible using the URL http://myfirstapp-pong.mybluemix.net.

	>**IMPORTANT:** If you encounter an error message `host is taken`, it means that the application name you specified has already been used by another Bluemix user.  Issue the `cf push` command again but change the name of your application.  In the example above, instead of `myfirstapp-pong`, it can be `myfirstapp-pong2`.

	The `-m` switch allows you to specify the memory allocation of your application.

	The `-p` switch allows you to specify the location of the file containing the sample application.

1. Open another browser tab (do not close the browser tab containing your Bluemix account).  Go to `http://myfirstapp-<your_name>.mybluemix.net` to verify that the sample application is successfully deployed.

	> If you encounter a `404 Not Found: Requested route ('-----.mybluemix.net') does not exist`, it may mean any of the following:
		a. you typed the wrong URL (**solution:** double check the URL)
		b. your application is not yet running (**solution:** wait for your application to run, refer to the sample output above)
		c. your application failed to run (**solution:** look at the error message and issue again the `cf push` command)

	The `PostgreSQL Upload` sample application is shown on the screen.  Take note that aside from logging in using the `cf` tool, the only other command you issued earlier is `cf push` which deployed your application in your Bluemix account.  You NEVER explicitly created/set-up a web server to host your application.  When you issued the `cf push` command, Bluemix determined that you want to deploy a Java-based web application and automatically created the necessary web server (which we refer to in Bluemix as runtime) to host your application.  You will see later the specific type of runtime that was created to host your application. 

	The sample application allows you to upload a text file in a PostgreSQL database.  You will test if this sample application is correctly running.

1. Click the `Browse` button of the sample application and choose any text file
	> Make sure it is a text file and not a binary file.
	> If you don't have any text file, just create one and place at least 3 lines of text.



	
	adsf
	adf

xxx
1. In the menu, click `CATALOG`.  The Bluemix Catalog shows the different services and APIs, as well as runtimes and containers that you may create.


	```text
	> git clone https://github.com/pong-pantola/junit-basics.git
	> cd junit-basics
	```


1. Clone the git repository `https://github.com/pong-pantola/junit-basics.git` and go to the created `junit-basics` directory.

	```text
	> git clone https://github.com/pong-pantola/junit-basics.git
	> cd junit-basics
	```
 
	The `junit-basics` directory has two subdirectories: `src` and `build`.

	```text
	junit-basics/
	|
	|----src/
	|    |
	|    |----main/java/net/tutorial/
	|    |                  |
	|    |                  |----Math.java
	|    |                  |----Calculator.java
	|    |
	|    |----test/java/net/tutorial
	|                       |
	|                       |----MyTest.java
	|                       |----TestRunner.java
	|
	|----build/
	     |
	     |----classes/
	     |    |
	     |    |----main/
	     |    |----test/
	     |    
	     |----libs/
	``` 
 
	`src` has two subdirectories: `main` and `test`. 

	`src/main` contains the Java class `src/main/java/net/tutorial/Math.java` which you will test later using JUnit.  In addition, it contains the `src/main/java/net/tutorial/Calculator.java` which is a sample Java application that uses `Math.java`. 

	`src/test` contains the Java class `src/test/java/net/tutorial/MyTest.java` which is the test class that will be used to test `Math.java`.  In addition, it contains the `src/test/java/net/tutorial/TestRunner.java` which is a Java application that will run the test. 
 
	`build` has two subdirectories: `classes` and `libs`. 

	`build/classes` is used to hold the the `.class` files that will be created later when you compile your `.java` files.

	`build/libs` is used for the JUnit libraries (i.e, `.jar` files) that you will download later.

<br>
####Download the JUnit libraries
1. Go to [https://github.com/junit-team/junit/wiki/Download-and-Install](https://github.com/junit-team/junit/wiki/Download-and-Install).
 
	>Just in case the URL is broken, you may go to [http://junit.org/](http://junit.org/) and find the download link.
 
2. Download the latest version of `junit.jar` and `hamcrest-core.jar` and save them in the subdirectory `build/libs`.

<br>

####Examine the Java class to be tested


1. Let's examine the sample class `Math.java` which you will test later with JUnit.
 
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
	    //a-b is used instead of a+b to simulate
	    //a possible error in the source code
	    return a-b;
	  }
	
	  public int sub(int a, int b){
	    return a-b;
	  }
	
	  public int multiply(int a, int b){
		//added delay to simulate that this method is 
	    //taking too long to execute	
		delay();
	
	    return a*b;
	  }
	}
	```
 
	`Math.java` contains the methods you want to test: `add`, `sub`, and `multiply`.  
 
	The `add` method is intentionally made incorrect by using `return a-b;` instead of `return a+b;` to demonstrate errors that may be detected by JUnit.

	The `multiply` method contains the line `delay();` to force the `multiply` method to execute for more than 3 secs.  This is useful when you demonstrate the concept of timeout in JUnit.

	The `sub` method is included to serve as a control.  Since the implementation of `sub` is correct, JUnit should not report any error involving `sub`.
 
	<br>

1. `Math.java` may be used by other Java classes to create an application. An example of this is `Calculator.java`.

	**Source code** of	`src/main/java/net/tutorial/Calculator.java`:
 
	```java
	package net.tutorial;
	
	public class Calculator{
	
	  public static void main (String args[]){
	    Math m = new Math();
		
		System.out.println("5 + 9 = " + m.add(5, 3));
		
		System.out.println("8 - 2 = " + m.sub(8, 2));	
		
		System.out.println("4 x 7 = " + m.multiply(4, 7));
	  }
	}
	```

	`Calculator.java` represents a sample application that uses the `Math.java` class.  

	<br>
 
1. Compile `Math.java` and `Calculator.java`.

	```text
	> javac -d build/classes/main src/main/java/net/tutorial/*.java
	```
	
	> Make sure that you are in the `junit-basics` directory before issuing the command above.
	
	>It is worth noting that the subdirectory `build/classes/main` already exists prior to compilation.  This is essential since the `-d build/classes/main` option in the command above means that the subdirectory `build/classes/main` will be used as the base directory of the `.class` files that will be created during compilation.  If the subdirectory does not exist, the command above will produce an error.

	<br>
	

1. Run the `Calculator` application.

	```text
	> java -classpath build/classes/main net/tutorial/Calculator
	```

	**Output:**

	```text
	5 + 9 = -4
	8 - 2 = 6
	4 x 7 = 28
	```

	As expected, the output `5 + 9 = -4` is wrong.  In addition, it took approximately 3 secs. before the line `4 x 7 = 28` appeared.
 
<br>
####Test the Java class

1. Let's examine the code `MyTest.java` which will serve as the test class to test the methods of `Math.java`.

	**Source code** of	`src/main/test/net/tutorial/MyTest.java`:

	```java
	package net.tutorial;
	
	import static org.junit.Assert.assertEquals;
	import org.junit.Before;
	import org.junit.Test;
	
	public class MyTest{
	  private Math m;
	  
	  @Before
	  public void initializeMath(){
	    m = new Math();
	  }
	  
	  @Test(timeout=1000)
	  public void addShouldReturnSum() {
	    assertEquals("3 + 7 should be 10", 10, m.add(3, 7));
	  }
	  
	  @Test(timeout=1000)
	  public void subShouldReturnDifference() {
	    assertEquals("5 - 9 should be -4", -4, m.sub(5, 9));
	  }  
	  
	  @Test(timeout=1000)
	  public void multiplyShouldReturnProduct() {
	    assertEquals("8 * 4 should be 32", 32, m.multiply(8, 4));
	  }
	} 
	```

	Let's look at some code segments in `MyTest.java` that are not typically found in a Java code.

	First of all `MyTest.java` contains a static import: 

	```java
	import static org.junit.Assert.assertEquals;
	```

	Static import allows you to access static items (e.g., static methods) in a class without specifying the name of the class.  As an example, JUnit has a class `Assert` that has a static method `assertEquals`.  If a non-static import (i.e., the typical way of importing a class) is used:
 
	```java
	import org.junit.Assert;
	```

	you need to specify the `Assert` class when calling the `assertEquals` method:

	```java
	    Assert.assertEquals("3 + 7 should be 10", 10, m.add(3, 7));
	```

	Since `MyTest.java` utilizes static import, you may omit the class `Assert`:

	```java
	    assertEquals("3 + 7 should be 10", 10, m.add(3, 7));
	```

	This makes the code shorter especially if you plan to use the `assertEquals` method several times.

	Aside from a non-static import, `MyTest.java` utilizes several JUnit annotations : 

	```java
	  @Test(timeout=1000)
	   
	  @Before
	```

	Before we discuss `@Test(timeout=1000)` let's discuss `@Test` first.  Placing the annotation `@Test` before a method specifies that a method is used as a test method.  Therefore, `MyTest.java` has three test methods: `addShouldReturnSum`, `subShouldReturnDifference`, and `multiplyShouldReturnProduct`.

	`@Test(timeout=1000)` has the same effect as `@Test` but performs an additional test by monitoring the execution time of a test method.  If a test method executes beyond a specified timeout then it will produce an error.  The timeout is in msec.  This means `@Test(timeout=1000)` will wait for 1000 msecs. or 1 sec. for a test method to complete.  If after 1 sec. a test method has not finished executing,  a timeout error is reported.  

	In `MyTest.java`, the three methods of`Math.java` (i.e., `add`, `sub`, and `multiply`) are tested with a timeout of 1 sec.  Recall that we intentionally placed a 3 secs. delay in the `multiply` method.  We expect a timeout error reported when you run the test later.

	`@Before` indicates that a method needs to be executed before each test method is executed.  In `MyTest.java` we use `@Before` in the following:

	```java
	  @Before
	  public void initializeMath(){
	    m = new Math();
	  }
	```

	 Given this, the `initializeMath` method gets executed before each of the three test methods get executed.  In `MyTest.java`, you may opt to omit the `@Before` annotation as well as the `initializeMath` method.  However, you need to insert the instantiation of `m` in each test method.  An example is shown below:

	```java
	    @Test(timeout=1000)
	    public void addShouldReturnSum() {
	      m = new Math();
	      assertEquals("3 + 7 should be 10", 10, m.add(3, 7));
	    }
	    
	    @Test(timeout=1000)
	    public void subShouldReturnDifference() {
	      m = new Math();      
	      assertEquals("5 - 9 should be -4", -4, m.sub(5, 9));
	    }  
	    
	    @Test(timeout=1000)
	    public void multiplyShouldReturnProduct() {
	      m = new Math();      
	      assertEquals("8 * 4 should be 32", 32, m.multiply(8, 4));
	    }
	  } 
	```

	There are other JUnit annotations aside from `@Test`, `@Test(timeout=1000)`, and `@Before`.  You may check the webpage [Unit Testing with JUnit - Tutorial](http://www.vogella.com/tutorials/JUnit/article.html) for a discussion of other JUnit annotations.

	It can be observed that the names of the test methods in `MyTest.java` (i.e., `addShouldReturnSum`, `subShouldReturnDifference`, and `multiplyShouldReturnProduct`) are relatively long.  This is essential to make the report generated by JUnit more intuitive.  When a test method encounters an error, the name of the test method is included in the report.  Having very descriptive method names allows developers to debug the codes faster.

	The last thing that is worth examining in `MyTest.java` is the `assertEquals` method.  The `assertEquals` method accepts the following parameters: `message`, `expected`, and `actual`.

	If `expected` is not equal to `actual`, the `assertEquals` method throws an `AssertException` that contains a message that is based on the `message` parameter.

	Aside from `assertEquals`, there are other methods that can be used for testing.  You may check the webpage [Unit Testing with JUnit - Tutorial](http://www.vogella.com/tutorials/JUnit/article.html) for additional methods that can be used for testing.

	<br>
 
1. Let's now see how the test class `MyTest.java` is executed.   `TestRunner.java` is an application that runs the test methods found in `MyTest.java`.

	**Source code** of	`src/test/java/net/tutorial/TestRunner.java`:
 
	```java
	package net.tutorial;
	
	import org.junit.runner.JUnitCore;
	import org.junit.runner.Result;
	import org.junit.runner.notification.Failure;
	
	public class TestRunner{
	  public static void main(String args[]){
	    Result result = JUnitCore.runClasses(MyTest.class);
		int errorCtr = 0;
	    for (Failure failure : result.getFailures()) {
		  errorCtr++;
		  System.out.println("Error #:"+ errorCtr);
	      System.out.println(failure.toString());
		  System.out.println();
	    }
		
		if (errorCtr == 0)
		  System.out.println("Congratulations!  There are no errors.");
	  }
	}
	```

	Notice that `TestRunner.java` never explicitly called the test methods `addShouldReturnSum`, `subShouldReturnDifference`, and `multiplyShouldReturnProduct` found in `MyTest.java`.

	Instead, it simply calls the `runClasses` method in `JUnitCore`:
 
	```java
	      Result result = JUnitCore.runClasses(MyTest.class); 
	``` 

	The `runClasses` method has a parameter `MyTest.class`.  It is able to identify the test methods to execute in `MyTest.java` because of the `@Test` annotations.  The result of all test methods (i.e., succeeded or failed) are saved in `result` which is an instance of `Result`.

	`Result` has a `getFailures` method which returns a `List` of `Failure`.

	<br>

1. Compile `MyTest.java` and `TestRunner.java`.

	> Make sure that you are in the `junit-basics` directory before issuing the command below.
   
	```text
	> javac -classpath build/libs/*;build/classes/main -d build/classes/test src/test/java/net/tutorial/*.java
	```

	Notice that the classpath includes `build/libs/*`.  Recall that you downloaded and saved the two JUnit `jar` files in this directory.
 
 
1. Run the `TestRunner` application.

	```text
	> java -classpath build/libs/*;build/classes/main;build/classes/test net/tutorial/TestRunner
	```
	
	**Output:**

	```text  
	Error #:1
	multiplyShouldReturnProduct(net.tutorial.MyTest): test timed out after 1000 milliseconds
	    
	Error #:2
	addShouldReturnSum(net.tutorial.MyTest): 3 + 7 should be 10 expected:<10> but was:<-4>
	```
 
	As expected, the `multiplyShouldReturnProduct` test method resulted to an error since the `multiply` method of `Math.java` has a call to the `delay` method which produces a 3 sec. delay.  This is way longer than the 1 sec. timeout that is indicated in the annotation in the test method (i.e., `@Test(timeout=1000)`).

	In addition, the `addShouldReturnSum` test method also failed since we intentionally made the `sum` method of `Math.java` incorrect.  Recall that we used `return a-b;` instead of `return a+b;` in the `sum` method of `Math.java`.
 
<br>
####End of Tutorial


