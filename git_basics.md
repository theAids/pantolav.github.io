---
layout: post
title: Git Basics
permalink: /git_basics/
---

##Application Development Tutorial

###Git Basics
[Git](https://hub.jazz.net) is a version control system for software development.

In this tutorial you will learn how to use the version control of Git to revert from a current version to a previous version.


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



####Install a Git Client


1. Go to [Git](https://git-scm.com/).

1. Download the Git client installer and install.

1. Test if the Git client is installed successfully.  Open a terminal window.

	```text
	> git
	```
	**Output:**

	```text
	usage: git [--version] [--help] [-C <path>] [-c name=value]
	           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
	           [-p|--paginate|--no-pager] [--no-replace-objects] [--bare]
	           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
	           <command> [<args>]
	
	The most commonly used git commands are:
	   add        Add file contents to the index
	   bisect     Find by binary search the change that introduced a bug
	   :
	   :
	```

	You should see the help screen of the Git client.

	<br>
	
####Create several text files and use Git to manage the versions of the files

In this tutorial, you will be creating several `.txt` files that will contain a list of words representing each letter of the alphabet.  The use of this approach will help you understand how Git works.  Once you are familiar with Git, you may use Git with your source code (e.g., `.java` file).

1. Create the directory `gittemp` in the root directory.  Go to the created directory.

	```text
	> mkdir gittemp
	> cd gittemp
	```

1. Create the subdirectory `alphabet` in the `gittemp` directory.  Go to the created directory.

	```text
	> mkdir alphabet
	> cd alphabet
	```

	The `alphabet` directory represents a working directory containing your source code.  As mentioned earlier, instead of source code, text files will be used in this tutorial.

1. Make the `alphabet` directory a local Git repository.

	```text
	> git init
	```

	**Output:**

	```text
	Initialized empty Git repository in D:/gittemp/alphabet/.git/
	```

	A hidden directory named `.git` is created after you issued the `git init` command.  You should not modify the contents of the `.git` hidden directory.  This contains information that will track the changes you made inside the `alphabet` directory.

	<br>


1. Check the status of your local Git repository.

	```text
	> git status
	```

	**Output:**

	```text
	On branch master
	
	Initial commit
	
	nothing to commit (create/copy files and use "git add" to track)
	```

		As stated in the status, there is `nothing to commit`.  This will change once you start creating files.


1. Create a file `animal.txt` with the following contents:

```text


```

1. Using the same web browser tab, go to the GitHub repository [`https://github.com/pong-pantola/devops-delivery-pipeline`](https://github.com/pong-pantola/devops-delivery-pipeline).

1. Fork the repository by clicking the `Fork` button. 

1. Verify that you have successfully forked the repository by checking its name:

	>**VERY IMPORTANT:**
	>In this tutorial, we will use the username `juandelacruz`.  Kindly use your username in place of `juandelacruz` when you do the tutorial.

	**Name of Repository:**

	```text		
	juandelacruz/devops-delivery-pipeline
	```

		The repository contains the following.

	```text
	devops-delivery-pipeline/
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

	The subdirectories and files are exactly the same as the one you used and created in [Creating a Web Application using Gradle Tutorial](/gradle-web-application). 

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

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: Click the `ADD STAGE` button.  Change the stage name `MyStage` to `Build Stage`.

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `INPUT` tab, set the following values:

	||||
	|-|-|-|
	| **Input Type** | SCM Repository |
	| **Git URL** | https://github.com/juandelacruz/devops-delivery-pipeline.git |
	| **Branch** | master |
	| **Stage Trigger** | Run jobs whenever a change is pushed to Git |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `JOBS` tab, click the `ADD JOB` link and select `Build`.   Change the job name `Build` to `Gradle Assemble`.  Set the following values:

	||||
	|-|-|-|
	| **Builder Type** | Gradle |		
	| **Build Shell Command** | `#!/bin/bash`<br>`gradle asemble`  |	
	| **Stop running this stage if this job fails** | checked |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click the `SAVE` button.

	<br>
	
####Create a Test Stage

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: Click the `ADD STAGE` button.  Change the stage name `MyStage` to `Test Stage`.

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `INPUT` tab, set the following values:

	||||
	|-|-|-|
	| **Input Type** | Build Artifacts |
	| **Stage** | Build Stage |
	| **Job** | Gradle Assemble |
	| **Stage Trigger** | Run jobs when the previous stage is completed |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `JOBS` tab, click the `ADD JOB` link and select `Test`.   Change the job name `Test` to `JUnit Test through Gradle`.  Set the following values:

	||||
	|-|-|-|
	| **Tester Type** | Simple |		
	| **Test Command** | `#!/bin/bash`<br>`gradle test`  |	
	| **Stop running this stage if this job fails** | checked |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click the `SAVE` button.

	<br>

####Create a Deploy Stage

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: Click the `ADD STAGE` button.  Change the stage name `MyStage` to `Dev Deploy Stage`.

	>Unlike the build and test stages which are named `Build Stage` and `Test Stage`, respectively, the name of the deploy stage you are about to create is `Dev Deploy Stage` to denote that that the application will be deployed in the `dev` space of your Bluemix account.  Another deploy stage will be created later.


1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `INPUT` tab, set the following values:

	||||
	|-|-|-|
	| **Input Type** | Build Artifacts |
	| **Stage** | Build Stage |
	| **Job** | Gradle Assemble |
	| **Stage Trigger** | Run jobs when the previous stage is completed |

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `JOBS` tab, click the `ADD JOB` link and select `Deploy`.   Change the job name `Deploy` to `Cloud Foundry Push to Dev Space`.  Set the following values:

	||||
	|-|-|-|
	| **Deployer Type** | Cloud Foundry |		
	| **Target** | IBM Bluemix US South - https://api.ng.bluemix.net |		
	| **Organization** | you may leave the default selection |		
	| **Space** | dev |	
	| **Application Name** | blank |		
	| **Deploy Script** | `#!/bin/bash`<br>`cf push calculator-<your_name> -m 256M -p build/libs/calcuapp.war`  |	
	| **Stop running this stage if this job fails** | checked |

	<br>

	

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click the `SAVE` button.

	You have created a delivery pipeline. 

	<br>

#### Deploy the Application through the Delivery Pipeline

1.  On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click (open in another browser tab) the `View logs and history ` link of the `Build Stage`.  We will refer to this browser tab as `DEVOPS-BUILD-STAGE-LOGS TAB`.

1.  On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click (open in another browser tab) the `View logs and history ` link of the `Test Stage`.  We will refer to this browser tab as `DEVOPS-TEST-STAGE-LOGS TAB`.

1.  On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click (open in another browser tab) the `View logs and history ` link of the `Dev Deploy Stage`.  We will refer to this browser tab as `DEVOPS-DEV-DEPLOY-STAGE-LOGS TAB`.

	The `DEVOPS-BUILD-STAGE-LOGS TAB`, `DEVOPS-TEST-STAGE-LOGS TAB`, `DEVOPS-DEV-DEPLOY-STAGE-LOGS TAB` will allow you to monitor the status of the delivery pipeline in each stage.

	Recall that in the [Creating a Web Application using Gradle Tutorial](/gradle-web-application) the following commands are used:

	Command | Purpose
	-|-
	`gradle assemble` | build `calcuapp.war`
	`gradle test` | run the JUnit test
	`cf push` | deploy the web application in Bluemix

	These three commands are exactly the same commands that the three stages will do.


1.  On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click the `Run Stage` icon of the `Build Stage`.

	Notice that the status o fthe `Build Stage` changes to `STAGE RUNNING`.  W

	Once the status `Build Stage` changed to `STAGE PASSED`, the `Test Stage` will automatically start.

	Similarly, when the status `Test Stage` changed to `STAGE PASSED`, the `Dev Deploy Stage` will automatically start.

	Wait for the status of the `Dev Deploy Stage` to change to `STAGE PASSED`.

	You may view the `DEVOPS-BUILD-STAGE-LOGS TAB`, `DEVOPS-TEST-STAGE-LOGS TAB`, `DEVOPS-DEV-DEPLOY-STAGE-LOGS TAB` to see the logs related to the execution of the three stages.

1. Open another web browser tab.  We will refer to this browser tab as `CALCULATOR-APP TAB`.

1.  On the `CALCULATOR-APP TAB`:  Go to `http://calculator-<your_name>.mybluemix.net/calculator.jsp`.

	**Output:**
	
	```text
	5 + 9 = 14
	8 - 2 = 6
	4 x 7 = 28 
	```

	<br>
#### Automatically start the Delivery Pipeline

In the previous steps, you manually started the delivery pipeline by clicking the `Run Stage` icon of the `Build Stage`.  

Since the `Build Stage` is configured to `Run jobs whenever a change is pushed to Git`, the delivery pipeline can be started whenever changes occur in the GitHub repository.

1.  On the `GITHUB TAB`:  Open the file `src/main/webapp/calculator.jsp` for editing.

1. On the `GITHUB TAB`:  Add the following lines at the end just before the `</body>` tag:

	```java
	<%="2 + 2 = " + m.add(2, 2)%>
	<br>
	```
1. On the `GITHUB TAB`:  Click the `Commit changes` button.

1. Quickly switch to the `DEVOPS-DELIVERY-PIPELINE TAB` and verify that the `Build Stage` automatically started due to the changes made in the GitHub repository.

	Wait until the three stages are complete.

1.  On the `CALCULATOR-APP TAB`:  Refresh the page to see the changes made in the calculator application.

	**Output:**
	
	```text
	5 + 9 = 14
	8 - 2 = 6
	4 x 7 = 28 
	2 + 2 = 4 
	```

	As expected, `2 + 2 = 4` is included in the output.

	Let's modify again the  file `src/main/webapp/calculator.jsp` but this time using the Bluemix DevOps editor and see if the delivery pipeline is automatically triggered.

1.  On the `DEVOPS-EDITOR TAB`:  Open the file `src/main/webapp/calculator.jsp`.  

	Notice that the lines you added in the `calculator.jsp`(i.e., `<%="2 + 2 = " + m.add(2, 2)%>` and `<br>`) in your GitHub repository does not appear in `calculator.jsp` of your working directory.

	You need first to sync the working directory in your Bluemix DevOps project before you start editing another file.

1.  On the `DEVOPS-GIT TAB`:  Refresh the page.

1.  On the `DEVOPS-GIT TAB`:  Notice that there is an `Incoming` change due to the modification of `calculator.jsp`.  Click the `Sync` button to update the copy of `calculator.jsp` in the working directory in your Bluemix DevOps project.

1.  On the `DEVOPS-EDITOR TAB`:  Refresh the page and reopen the file `src/main/webapp/calculator.jsp`.  Notice that the following lines now exist:

	```java
	<%="2 + 2 = " + m.add(2, 2)%>
	<br>
	```

1. On the `DEVOPS-EDITOR TAB`:  Add the following lines at the end just before the `</body>` tag:

	```java
	<%="3 - 3 = " + m.sub(3, 3)%>
	<br>
	```
1. On the `DEVOPS-EDITOR TAB`:  Make sure to save the changes made.

1. Quickly switch to the `DEVOPS-DELIVERY-PIPELINE TAB`.  

	Notice that the `Build Stage` did not start automatically.  You only changed the `calculator.jsp` that is in the working directory and not the one in the GitHub repository.   You need to push the changes made in the working directory to the GitHub repository for the `Build Stage` to start.



1. On the `DEVOPS-GIT TAB`: Refresh the page.

1. On the `DEVOPS-GIT TAB`: Set the following values:

	||||
	|-|-|-|
	| **Select All** | checked |
	| **Commit message** | added another computation |

	<br>

1. On the `DEVOPS-GIT TAB`: Click the `Commit` button.


1. On the `DEVOPS-GIT TAB`: Click the `Push` button.

	Your GitHub repository is now updated with the new version of `calculator.jsp`.

1. Quickly switch to the `DEVOPS-DELIVERY-PIPELINE TAB` and verify that the `Build Stage` automatically started due to the changes made in the GitHub repository.

####See the Effect if an Error is encountered in the Delivery Piipeline

You will intentionally introduce errors in `src/main/java/net/tutorial/Math.java` so that you can verify if the `Test Stage` will detect the errors.

1.  On the `GITHUB TAB`:  Open the file `src/main/java/net/tutorial/Math.` for editing.

1. On the `GITHUB TAB`:  Change the method `add` to the following:

	```java
	  public int add(int a, int b){
	    return a-b;
	  }
	```
	Since `a+b` is changed to `a-b`, we expect an error to be reported related to the `add` method.
	
	This error is discussed in detail in the [JUnit Basics Tutorial](/junit-basic) and revisited in [Gradle's Unit Testing Tutorial](/gradle-unit-testing).

	<br>
	
1. On the `GITHUB TAB`:  Change the method `multiply` to the following:

	```java
	  public int multiply(int a, int b){
	    delay();
	    return a*b;
	  }
	```

	Since `delay()` is added, we expect that `multiply` method will execute for at least 3 secs..  This will cause a timeout error.

	This error is also discussed in detail in the [JUnit Basics Tutorial](/junit-basic) and revisited in [Gradle's Unit Testing Tutorial](/gradle-unit-testing).

	<br>

1. On the `GITHUB TAB`:  Click the `Commit changes` button.

1. Quickly switch to the `DEVOPS-DELIVERY-PIPELINE TAB`.  

	As expected,  the `Build Stage` started automatically. 

	After the `Build Stage` is complete, the `Test Stage` started as well.  Due to the errors you introduced in the previous steps, the status of the `Test Stage` becomes `STAGE FAILED`.

	<br>

1. On the `DEVOPS-TEST-STAGE-LOGS TAB`: View the logs to verify that both `add` and `multiply` methods encounter problems.


1.  On the `GITHUB TAB`:  Open again the file `src/main/java/net/tutorial/Math.` for editing.  Correct the errors you introduced earlier (i.e., bring back `a+b` in the `add` method and remove the  `delay()` in the `multiply` method).  Don't forget to click the `Commit changes` button.

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: Ensure that all the stages are executed successfully.

	<br>
	
#### Create another Deploy Stage

The `Dev Deploy Stage` created earlier deploys the web application in the `dev` space in your Bluemix account.

You will create another deploy stage called `Prod Deploy Stage` which will redeploy the same application in the `prod` space in your Bluemix account.

Having separate deploy stages for development and production is essential to ensure that features/functionalities that are added to a web application is verified first in the `dev` space.  Once the features/functionalities are verified to be working properly, this is the only time the updated web application is redeployed to the `prod` space through the `Prod Deploy Stage`.

1. Make sure that you have a `prod` space under the region `US-South` in your Bluemix account. 

	>If you don't have a `prod` space,  you may use the [Bluemix Basics Tutorial](/bluemix-basics) as a guide in creating a space.


1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: Click the `ADD STAGE` button.  Change the stage name `MyStage` to `Prod Deploy Stage`.


1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `INPUT` tab, set the following values:

	||||
	|-|-|-|
	| **Input Type** | Build Artifacts |
	| **Stage** | Build Stage |
	| **Job** | Gradle Assemble |
	| **Stage Trigger** | Run jobs only when this stage is run manually |

	It should be noted that the **Stage Trigger** for this stage is `Run jobs only when this stage is run manually`.  This is different from the `Dev Deploy Stage` that uses the **Stage Trigger** `Run jobs when the previous stage is completed`.

	Unlike the web application in the `dev` space, the web application deployed in the `prod` space should be free from errors.  If you use the **Stage Trigger** `Run jobs when the previous stage is completed`, it is possible that a an application developer may push changes to the GitHub repository which will eventually trigger the `Prod Deploy Stage` to run even if the changes made by the developer are unverified.
	
	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `JOBS` tab, click the `ADD JOB` link and select `Deploy`.   Change the job name `Deploy` to `Cloud Foundry Push to Prod Space`.  Set the following values:

	||||
	|-|-|-|
	| **Deployer Type** | Cloud Foundry |		
	| **Target** | IBM Bluemix US South - https://api.ng.bluemix.net |		
	| **Organization** | you may leave the default selection |		
	| **Space** | prod |	
	| **Application Name** | blank |		
	| **Deploy Script** | `#!/bin/bash`<br>`cf push calculator-prod-<your_name> -m 256M -p build/libs/calcuapp.war`  |	
	| **Stop running this stage if this job fails** | checked |

	<br>

	

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`:  Click the `SAVE` button.

	You have completed the `Prod Deploy Stage`.  You can now test the **Stage Trigger** of this stage.

1.  On the `GITHUB TAB`:  Open the file `src/main/webapp/calculator.jsp` for editing.

1. On the `GITHUB TAB`:  Add the following lines at the end just before the `</body>` tag:

	```java
	<%="4 x 4 = " + m.add(4, 4)%>
	<br>
	```
1. On the `GITHUB TAB`:  Click the `Commit changes` button.

1. Quickly switch to the `DEVOPS-DELIVERY-PIPELINE TAB` and verify that the `Build Stage` is triggered automatically, followed by the `Test Stage`, and followed by the `Dev Deploy Stage`.

	Notice that the `Prod Deploy Stage` does not start automatically after the `Dev Deploy Stage` is complete.  You need to manually start the `Prod Deploy Stage`.

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: Click the `Run Stage` icon of the `Prod Deploy Stage`.

	Wait for this stage to be completed.

1. Open another web browser tab.  We will refer to this browser tab as `CALCULATOR-PROD-APP TAB`.

1.  On the `CALCULATOR-PROD-APP TAB`:  Go to `http://calculator-prod-<your_name>.mybluemix.net/calculator.jsp`.

	**Output:**
	
	```text
	5 + 9 = 14
	8 - 2 = 6
	4 x 7 = 28 
	2 + 2 = 4
	3 - 3 = 0
	4 x 4 = 16 	
	```

	Note that what you have viewed is the production version of the calculator application (`http://calculator-prod-<your_name>.mybluemix.net/calculator.jsp`)

	The development version of the application is accessible through `http://calculator-<your_name>.mybluemix.net/calculator.jsp`.

	<br>

####End of Tutorial


####What's next?




