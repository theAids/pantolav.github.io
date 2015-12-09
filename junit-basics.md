##Application Development Tutorial

###JUnit Basics
JUnit is a simple framework to write repeatable tests. You may go to http://junit.org/ for additional information regarding JUnit.

In this tutorial we will learn how to create a simple test class that is used to test the methods of a Java class.

>**Prerequisite:**

>Having a good understanding of Java programming is required to do this tutorial.


####Copy Sample Codes from Git repository
1. Open a terminal window and create the directory `junittemp` in the root directory.  Go to the created directory.
 ```bash
    >mkdir junittemp
    >cd junittemp
    
 ```

1. Clone the git repository `https://hub.jazz.net/git/pantolav/junit-basics` and go to the created `junit-basics` directory.
 ```bash
    >git clone https://hub.jazz.net/git/pantolav/junit-basics
    >cd junit-basics
    
 ```
 The `junit-basics` directory has two subdirectories: `src` and `build`.
 
 `src` has two subdirectories: `main` and `test`. 

 `src/main` contains the Java class `src/main/java/net/tutorial/Math.java` which we will be tested later using JUnit.  In addition, it contains the `src/main/java/net/tutorial/Calculator.java` which is a sample Java application that uses `Math.java`. 

 `src/test` contains the Java class `src/test/java/net/tutorial/MyTest.java` which is the test class that will be used to test `Math.java`.  In addition, it contains the `src/test/java/net/tutorial/TestRunner.java` which is a Java application that will run the test. 
 
 `build` has two subdirectories: `classes` and `libs`. 

 `build/classes` is used to hold the the `.class` files that will be created later when we compile our `.java` files.

 `build/libs` is used to contain the JUnit libraries (i.e, `.jar` files) that you will download later.

####Download the JUnit libraries
1. Go to https://github.com/junit-team/junit/wiki/Download-and-Install.
 >Just in case the URL is broken.  You may go to http://junit.org/ and find the download link.

2. Download the latest version of `junit.jar` and `hamcrest-core.jar` and save them in the subdirectory `build/libs`.

####Examine the Java class to be tested


1. Let's examine the sample class `Math.java` which we will test later with JUnit.

 >`src/main/java/net/tutorial/Math.java`:

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
 
 `Math.java` contains the methods we want to test: `add`, `sub`, and `multiply`.  
 
 The `add` method is intentionally made incorrect by using `return a-b;` instead of `return a+b;` to demonstrate errors that may be detected by JUnit.

 The `multiply` method contains the line `delay();` to force the `multiply` method to execute for more than 3 secs.  This is useful when we demonstrate the concept of timeout in JUnit.

 The `sub` method is included to serve as a control.  Since the implementation of `sub` is correct, JUnit should not report any error involving `sub`.
 <br>
 
1. The sample code above may be used by other Java classes to create an application. An example of this is `Calculator.java`.

 >`src/main/java/net/tutorial/Calculator.java`:
 
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

 `Calculator.java` represents a sample application that uses the `Math.java` class we discussed above.  
 <br>
 
1. Compile `Math.java` and `Calculator.java`.
 > Make sure that you are in the `junit-basics` directory before issuing the command below.
 
 ```bash
    >javac -d build/classes/main src/main/java/net/tutorial/*.java
 ```

1. Run the `Calculator` application.
 ```bash
    >java -classpath build/classes/main net/tutorial/Calculator
 ```
  **Output:**
 ```bash    
    5 + 9 = -4
    8 - 2 = 6
    4 x 7 = 28
 ```

 As expected, the output `5 + 9 = -4` is wrong.  In addition, it took approximately 3 secs. before the line `4 x 7 = 28` appeared.
 

####Test the Java class

1. Let's examine the code `MyTest.java` which will serve as the test class to test the methods of `Math.java`.

 >`src/main/test/net/tutorial/MyTest.java`:

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

 Static import allows us to access static items (e.g., static methods) in a class without specifying the name of the class.  As an example, JUnit has a class `Assert` that has a static method `assertEquals`.  If a non-static import (i.e., the typical way of importing a class) is used:
 
 ```java
    import org.junit.Assert;
 ```

 we need to specify the `Assert` class when calling the `assertEquals` method:

 ```java
        Assert.assertEquals("3 + 7 should be 10", 10, m.add(3, 7));
 ```

 Since `MyTest.java` utilizes static import, we may omit the class `Assert`:

 ```java
        assertEquals("3 + 7 should be 10", 10, m.add(3, 7));
 ```
 This makes the code shorter especially if we plan to use the `assertEquals` method several times.

 Aside from a non-static import, `MyTest.java` utilizes several JUnit annotations : 

 ```java
      @Test(timeout=1000)
    
      @Before
 ```

 Before we discuss `@Test(timeout=1000)` let's discuss `@Test` first.  Placing the annotation `@Test` before a method specifies that a method is used as a test method.  Therefore, `MyTest.java` has three test methods: `addShouldReturnSum`, `subShouldReturnDifference`, and `multiplyShouldReturnProduct`.

 `@Test(timeout=1000)` has the same effect as `@Test` but performs an additional test by monitoring the execution time of a test method.  If a test method executes beyond a specified timeout then it will produce an error.  The timeout is in msec.  This means `@Test(timeout=1000)` will wait for 1000 msecs. or 1 sec. for a test method to complete.  If after 1 sec. a test method has not finished executing,  a timeout error is reported.  

 In `MyTest.java`, the three methods of`Math.java` (i.e., `add`, `sub`, and `multiply`) are tested with a timeout of 1 sec.  Recall that we intentionally placed a 3 secs. delay in the `multiply` method.  We expect a timeout error reported when we run the test later.

 `@Before` indicates that a method needs to be executed before each test method is executed.  In `MyTest.java` we use `@Before` in the following:

 ```java
      @Before
      public void initializeMath(){
        m = new Math();
      }
 ```

 Given this, the `initializeMath` method gets executed before each of the three test methods get executed.  In `MyTest.java`, we may opt to omit the `@Before` annotation as well as the `initializeMath` method.  However, we need to insert the instantiation of `m` in each test method.  An example is shown below:

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

 >`src/test/java/net/tutorial/TestRunner.java`:
 
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
   
 ```bash
    >javac -classpath build/libs/*;build/classes/main -d build/classes/test src/test/java/net/tutorial/*.java
 ```
 Notice that the classpath includes `build/libs/*`.  Recall that we downloaded and saved the two JUnit `jar` files in this directory.
 
1. Run the `TestRunner` application.
 ```bash
    >java -classpath build/libs/*;build/classes/main;build/classes/test net/tutorial/TestRunner
 ``` 
  **Output:**
 ```bash  
    Error #:1
    multiplyShouldReturnProduct(net.tutorial.MyTest): test timed out after 1000 milliseconds
    
    Error #:2
    addShouldReturnSum(net.tutorial.MyTest): 3 + 7 should be 10 expected:<10> but was:<-4>

 ```
 
 As expected, the `multiplyShouldReturnProduct` test method resulted to an error since the `multiply` method of `Math.java` has a call to the `delay` method which produces a 3 sec. delay.  This is way longer than the 1 sec. timeout that is indicated in the annotation in the test method (i.e., `@Test(timeout=1000)`).

 In addition, the `addShouldReturnSum` test method also failed since we intentionally made the `sum` method of `Math.java` incorrect.  Recall that we used `return a-b;` instead of `return a+b;` in the `sum` method of `Math.java`.
 

####End of Tutorial

