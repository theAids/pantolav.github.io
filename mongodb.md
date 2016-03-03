---
layout: post
title: mongodb
permalink: /mongodb/
---

>[MongoDB](https://console.ng.bluemix.net/docs/services/MongoDB/index.html#MongoDB) is an open source document, NoSQL database that is owned by MongoDB Inc. You can use MongoDB to quickly develop applications by using the features that are provided, such as data scaling, auto-sharding, and high availability. You can also use MongoDB to store different types of document objects, such as JSON files, with a key-value pair.

>In this tutorial you will learn how to use the MongoDB driver for Java to connect to your MongoDB service in Bluemix.

<br>

####**Copy a GitHub Repository**

1. Open a browser and login to your [Github](https://github.com/) account. 

2. Go to the Github Repository [https://github.com/theAids/mongodb](https://github.com/theAids/mongodb) and fork the repository by clicking the  ` Fork` button.

<br>

####Create a Bluemix DevOps Project based on the GitHub Repository

1. Open another web browser tab and login to [Bluemix DevOps](https://hub.jazz.net/).

1. Click `CREATE PROJECT`.

1. Name your project `mongodb`.

1. Click `Link to an existing GitHub repository`.

1. Select the repository `<username>/mongodb`

1. Ensure the following options are chosen:

	||||
	|---|---|---|
	| **Private Project** | checked |
	| **Add features for Scrum development** | checked |
	| **Make this a Bluemix Project** | checked |
	| **Region** | IBM Bluemix US South |
	| **Organization** | you may leave the default selection |		
	| **Space** | dev |
	
1. Click the `CREATE` button and  wait for your project to be created.

1. Click the `EDIT CODE` button.  You will be redirected to Bluemix DevOps' editor

1. Click the `Git Repository` icon found on the left side of the screen. It will redirect you to another page.

1. On the `Working Directory` section (right side of the page) Set the following values:

	||||
	|---|---|---|
	| **Select All** | checked |
	| **Commit message** | files created when Bluemix DevOps project was created |

	<br>

1. Click the `Commit` button followed by the `Push` button to sync your working directory with the github repository.

1. When the process is complete, click the `BUILD & DEPLOY` button. We will now create the `Devops Delivery Pipeline`.

<br>

####Create a Build Stage

1.Click the `ADD STAGE` button.  Change the stage name `MyStage` to `Build Stage`.

1.On the `INPUT` tab, set the following values:

	||||
	|---|---|---|
	| **Input Type** | SCM Repository |
	| **Git URL** | https://github.com/<username>/mongodb.git |
	| **Branch** | master |
	| **Stage Trigger** | Run jobs whenever a change is pushed to Git |

	<br>

1. On the `JOBS` tab, click the `ADD JOB` link and select `Build`.   Change the job name `Build` to `Gradle Assemble`.  Set the following values:

	||||
	|---|---|---|
	| **Builder Type** | Gradle |		
	| **Build Shell Command** | `#!/bin/bash`<br>`gradle assemble`  |	
	| **Stop running this stage if this job fails** | checked |

	<br>

1. Click the `SAVE` button.

	<br>

####Create a Deploy Stage

1. Click the `ADD STAGE` button.  Change the stage name `MyStage` to `Dev Deploy Stage`.

	<br>

1. On the `DEVOPS-DELIVERY-PIPELINE TAB`: On the `INPUT` tab, set the following values:

	||||
	|---|---|---|
	| **Input Type** | Build Artifacts |
	| **Stage** | Build Stage |
	| **Job** | Gradle Assemble |
	| **Stage Trigger** | Run jobs when the previous stage is completed |

	<br>

1. On the `JOBS` tab, click the `ADD JOB` link and select `Deploy`.   Change the job name `Deploy` to `Cloud Foundry Push to Dev Space`.  Set the following values:

	||||
	|---|---|---|
	| **Deployer Type** | Cloud Foundry |		
	| **Target** | IBM Bluemix US South - https://api.ng.bluemix.net |		
	| **Organization** | you may leave the default selection |		
	| **Space** | dev |	
	| **Application Name** | blank |		
	| **Deploy Script** | `#!/bin/bash`<br>`cf push mongodb-<your_name> -m 256M -p build/libs/mongodb.war`  |	
	| **Stop running this stage if this job fails** | checked |
	
	<br>

1. Click the `SAVE` button.

	<br>
####Deploy the Application using Delivery Pipeline
1. On the `BUILD & DEPLOY` tab, click the `Run Stage` icon of the `Build Stage`.
	>Wait for the process to finish and make sure that it passed all the stages of the delivery pipeline.

2. Using your browser, log in to your [IBM Bluemix](ibm.biz/bluemixph) account.

3. Click the widget of your application `mongodb-<your name>`.

4. Click `ADD A SERVICE OR API`.

5. Scroll down further in the `CATALOG` page until you see the `Bluemix Labs Catalog` link. Click this link.

6. Look for `mongodb` under the `Data and Analytics` category.

7. Click `CREATE`. When asked to restage your application, click the `RESTAGE` button. Wait for your application to restage.

8. Open a new tab and go to `http://mongodb-<your name>.mybluemix.net`.

9. Choose any file you want to upload by clicking the `Choose File` button.

10. Click the `Upload File` button.


