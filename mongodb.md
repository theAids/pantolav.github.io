---
layout: post
title: mongodb
permalink: /mongodb/
---

[MongoDB](https://console.ng.bluemix.net/docs/services/MongoDB/index.html#MongoDB) is an open source document, NoSQL database that is owned by MongoDB Inc. You can use MongoDB to quickly develop applications by using the features that are provided, such as data scaling, auto-sharding, and high availability. You can also use MongoDB to store different types of document objects, such as JSON files, with a key-value pair.

In this tutorial you will learn how to use the MongoDB driver for Java to connect to your MongoDB service in Bluemix.

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
	>Do steps 10 and 11 only if necessary.

1. When the process is complete, click the `BUILD & DEPLOY` button. We will now create the `Devops Delivery Pipeline`.

<br>

####**Create a Build Stage**

1. Click the `ADD STAGE` button.  Change the stage name `MyStage` to `Build Stage`.

1. On the `INPUT` tab, set the following values:

	||||
	|---|---|---|
	| **Input Type** | SCM Repository |
	| **Git URL** | https://github.com/your_username/mongodb.git |
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

2. On the `INPUT` tab, set the following values:

	||||
	|---|---|---|
	| **Input Type** | Build Artifacts |
	| **Stage** | Build Stage |
	| **Job** | Gradle Assemble |
	| **Stage Trigger** | Run jobs when the previous stage is completed |


3. On the `JOBS` tab, click the `ADD JOB` link and select `Deploy`.   Change the job name `Deploy` to `Cloud Foundry Push to Dev Space`.  Set the following values:

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

4. Click the `SAVE` button.

	<br>
	
####Deploy the Application using Delivery Pipeline

1. On the `BUILD & DEPLOY` tab, click the `Run Stage` icon of the `Build Stage`.
	>Wait for the process to finish and make sure that it passed all the stages of the delivery pipeline.

2. Using your browser, log in to your [IBM Bluemix](https://ibm.biz/bluemixph) account.

3. Click the widget of your application `mongodb-<your name>`.

4. Click `ADD A SERVICE OR API`.

5. Scroll down further in the `CATALOG` page until you see the `Bluemix Labs Catalog` link. Click this link.

6. Look for `mongodb` under the `Data and Analytics` category.

7. Click `CREATE`. When asked to restage your application, click the `RESTAGE` button. Wait for your application to restage.

8. In your computer, open a text editor. Copy these lines and save them as follows:

	gagamba.json
	
	```text
	{
		"title" : "Gagamba",
		"isbn"	: "978-971-8845-59-2",
		"year" : 1991,
		"author": {
			"fname" : "f. sionil",
			"lname" : "jose"
		}
	}
	```
	
	mans_search.json
	
	```text
	{
		"title" : "Mans Search for Meaning",
		"isbn" : "978-0-8070-1429-5",
		"year"	: 1992,
		"author": {
			"fname" : "viktor",
			"lname" : "frankl"
		},
		"foreword" : "harold kushner"
	}
	```
We are going to use these two files to populate our database.

9. Open a new tab and go to `http://mongodb-<your name>.mybluemix.net`.

10. Click `Choose File` and locate the saved file `gagamba.json`. Click `Open`. 

11. Click the `Upload` button. The inserted database entry is shown at the bottom.

12. Repeat steps 10 - 11 fo the file `mans_search.json`. 
	>Notice that even though the `mans_search.json` file has an extra field `foreword`, it is still inserted into the database. This is because mondodb is a `schema less` database. Meaning, the number of fields from one entry can differ from another.

<br>

####**Analyze how the MongoDB Application works**

To be able to perform MongoDB operations, the MongoDB API for Java is needed. So it is important to include the the `org.mongodb:mongodb-driver:3.2.2` in the `build.gradle` file.

To connect to the service, the service URI is needed. This application used the `Cloudfoundry API` to parse the `VCAP_SERVICES` and get the service URI. Thus the `org.cloudfoundry:cloudfoundry-runtime:0.8.4` library is needed to solve the dependency problem.

	protected static String getServiceURI() throws Exception
    	{
    		CloudEnvironment environment = new CloudEnvironment();
	        if ( environment.getServiceDataByLabels("mongodb").size() == 0 ) 
	        {
	            throw new Exception( "No MongoDB service is bound to this app!!" );
	        } 
	
	        Map credential = (Map)((Map)environment.getServiceDataByLabels("mongodb").get(0)).get( "credentials" );
	     
	        return (String)credential.get( "url" );
      	}

The service's URI/URL is needed to create a new `MongoDB Client`. Unlike SQL databases, `NoSQL` do not use the traditional SQL statements but instead, it uses objects or documents in `JSON style format` to perform database operations. In MongoDB's context, a `collection` is the counterpart of `RDBMS's table`. And as shown in the code below, a collection will be automatically created if it is used prior to its existence.

	 public boolean addEntry(String jsonString) throws Exception
    	{
	        BasicDBObject entry = (BasicDBObject)JSON.parse(jsonString);
	
	        try{
	            String connURL = getServiceURI();
	
	            MongoClient mongo = new MongoClient(new MongoClientURI(connURL));
	
	            //database name is defined by the service, check VCAP_SERVICES
	            DB db = mongo.getDB("db");
		
		    //MongoDB will create the collection 'books' if it does not yet exist in the database.
	            DBCollection table = db.getCollection("books");
	            WriteResult wr = table.insert(entry);
	
	            //Returns true if the write was acknowledged.
	            return wr.wasAcknowledged();
	        }
	        catch (Exception e) 
	        {
	            e.printStackTrace();
	        }
	
	        return false;
    	}

MongoDB provides, high performance, high availability, and easy scalability. It is often used in  Big Data, Content Management Delivery and Data Hub.

####**Delete the Application and MongoDB Service**
1. Go to [IBM Bluemix](ibm.biz/bluemixph) Website and click the `Dashboard`.
2. From the `Applications` section, click the `gear` icon in the widget of the `mongodb-<your_name>` application.
3. Select the `Delete App` from the list.
4. In the `Services` tab, select the created `mongodb` service.
5. Click the `Delete` button.

####End of Tutorial
