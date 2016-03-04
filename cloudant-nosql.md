---
layout: post
title: cloudant nosql db
permalink: /cloudant-nosql/
---

[Cloudant NoSQL Database](https://console.ng.bluemix.net/docs/services/Cloudant/index.html#Cloudant) is a NoSQL database as a service (DBaaS). It's built from the ground up to scale globally, run non-stop, and handle a wide variety of data types like JSON, full-text, and geospatial. Cloudant NoSQL DB is an operational data store optimized to handle concurrent reads and writes, and provide high availability and data durability.

In this tutorial you will learn how to use the Cloudant API for Java and the Cloudant's web console to manage your database entries.

<br>

####**Copy a GitHub Repository**

1. Open a browser and login to your [Github](https://github.com/) account. 

2. Go to the Github Repository [https://github.com/theAids/cloudant](https://github.com/theAids/cloudant) and fork the repository by clicking the  ` Fork` button.

<br>

####Create a Bluemix DevOps Project based on the GitHub Repository

1. Open another web browser tab and login to [Bluemix DevOps](https://hub.jazz.net/).

1. Click `CREATE PROJECT`.

1. Name your project `cloudant`.

1. Click `Link to an existing GitHub repository`.

1. Select the repository `<username>/cloudant`

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
	>Do steps 10 and 11 only if necessary
1. When the process is complete, click the `BUILD & DEPLOY` button. We will now create the `Devops Delivery Pipeline`.

<br>

####**Create a Build Stage**

1. Click the `ADD STAGE` button.  Change the stage name `MyStage` to `Build Stage`.

1. On the `INPUT` tab, set the following values:

	||||
	|---|---|---|
	| **Input Type** | SCM Repository |
	| **Git URL** | https://github.com/your_username/cloudant.git |
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
	| **Deploy Script** | `#!/bin/bash`<br>`cf push cloudant-<your_name> -m 256M -p build/libs/cloudant.war`  |	
	| **Stop running this stage if this job fails** | checked |
	
	<br>

4. Click the `SAVE` button.

	<br>
	
####Deploy the Application using Delivery Pipeline

1. On the `BUILD & DEPLOY` tab, click the `Run Stage` icon of the `Build Stage`.
	>Wait for the process to finish and make sure that it passed all the stages of the delivery pipeline.

2. Using your browser, log in to your [IBM Bluemix](https://ibm.biz/bluemixph) account.

3. Click the widget of your application `cloudant-<your name>`.

4. Click `ADD A SERVICE OR API`.

5. Look for `Cloudant NoSQL DB` under the `Data and Analytics` category.

6. Click `CREATE`. When asked to restage your application, click the `RESTAGE` button. Wait for your application to restage.

7. In your computer, open a text editor. Copy these lines and save them as follows:

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

9. Open a new tab and go to `http://cloudant-<your name>.mybluemix.net`.

10. Click `Choose File` and locate the saved file `gagamba.json`. Click `Open`. 

11. Click the `Upload` button.

12. To see the inserted entry, go back to [IBM Bluemix Dashboard](https://ibm.biz/bluemixph).

13. Under `Services`, click the created `Cloudant NoSQL DB` service. 

14. Click the `Launch button` at the upper right corner. It will redirect you to your `Cloudant Web Console`.

<br>

####Exploring the Cloudant Web Console

The `Cloudant Web Console` can help you manage your databases, database entries, views, indexes and other Cloudant features such as Replication, which will be discussed later. Looking at the Web Console, we can see that we already have one `database` and one `document`. This database is statically created when we uploaded the `gagamba.json` file.

	>`Document` in NoSQL context is the counterpart of row/field in RDBMS.

Let us now examine the created database.

1. Click the database name `books`.

2. At the right side of the screen, we can see an entry in a JSON style format. This is the document that we have uploaded earlier. To see the full document, click the `pencil`(edit) icon at the upper right corner. You can see a file similar to this:

	```text
	{
	  "_id": "e66edb37df544ad59b7596ce62a042d0",
	  "_rev": "1-0f050c951b9430fae0a7ee73f3095fc4",
	  "year": 1991,
	  "author": {
	    "fname": "f. sionil",
	    "lname": "jose"
	  },
	  "isbn": "978-971-8845-59-2",
	  "title": "Gagamba"
	}
	```
Since we did not provide the `ID(_id)` and `Revision(_rev)` number, it was automatically created for us.

3. Click `Cancel`.

We can also use this Console to perform actions such as adding  new `database` or `document`. Click the `gear` icon to see more options.

<br>

####Replication Feature
One feature of this Web Console is `Replication Management`.

1. Click the `Replication` button at the left side of the window.
	>We will create a new replication.

2.  In the `New Replication` tab, enter `books` as the name of our `Source Database`
	>Make sure that you are in the `My Databases` tab.

3. For the `Target Database`, click `New Database` and enter `books-replica` as the name of our local replication database.

4. Check the `Make this replication continious`
	>This means that all subsequent changes to the source database are transmitted to the target database in real-time.

5. Provide your `Cloudant password`. This can be found in the `VCAP_SERVICES` of your cloudant application (`cloudant-<your name>`).
	>To access this, open another browser then go to your [IBM Bluemix](https://ibm.biz/bluemixph) Dashboard. Click the widget of your `cloudant-<your name>` application and click `Environmental Variables` at the left side of the window. It will show you the `VCAP_SERVICES` content of your application. Look for `clouddantNoSQLDB > credentials > password`.

6. Copy and paste the cloudant password from the VCAP_SERVICES into the password field. Click ` Continue Replication`.

7. At the upper left side of the screen click `Databases`.
	>We can see now three databases: the `original database(books)`, the replication `database(books-replica)` and the `_replicator` database that contains information about the replication.

####Test Replication
1. Open another and go back again to your cloudant application (`cloudant-<yourname>.mybluemix.net`).

2. Click `Choose File` and look for the created file `mans_search.json`. Click Open.

3. Click `Upload`.

4. Go back to the CLoudant Web Console tab and refresh the page. We can now see two documents in both the `books` and `books-replica` databases.


####**Analyze how the Cloudant NoSQL DB works**

To connect to the Cloudant service, this application used the `Cloudant Client API` contianed in `com.cloudant:cloudant-client:2.3.0` library. Originally, CLoudant NoSQL uses `HTTP API or RESTful API` to connect to the service. Any language-specific libraries such as this one are just wrappers to easily use the service.

	public int addEntry(String jsonString) throws Exception
    {
        try{
            CloudantClient client = getClientConn();

            Database db = client.database("books", true);
            JSONParser parser = new JSONParser();
            
            try{
              JSONObject json = (JSONObject)parser.parse(jsonString);
              
              Response rs = db.save(json);

              return rs.getStatusCode();

            }catch (ParseException e) {
              System.out.println("Failed Parsing: " + e.getErrorType() + e.toString());
              e.printStackTrace();
            }

        }catch (Exception e) {
            System.out.println("Failed: " + e.getMessage());
            e.printStackTrace();
        }

        return 0;
    }

Same with MongoDB, Cloudant NoSQl can also use JSON style files to populate the database and it will automatically create the database if it's not yet created.

####**Delete the Application and Cloudant NoSQL DB Service**
1. Go to [IBM Bluemix](ibm.biz/bluemixph) Website and click the `Dashboard`.
2. From the `Applications` section, click the `gear` icon in the widget of the `cloudant-<your_name>` application.
3. Select the `Delete App` from the list.
4. In the `Services` tab, select the created `Cloudant NoSQL DB` service.
5. Click the `Delete` button.

####End of Tutorial
