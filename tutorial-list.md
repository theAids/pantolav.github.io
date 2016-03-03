---
layout: post
title: Tutorial List
permalink: /tutorial-list/
---

[MongoDB](/mongodb)

>[MongoDB](https://console.ng.bluemix.net/docs/services/MongoDB/index.html#MongoDB) is an open source document, NoSQL database that is owned by MongoDB Inc. You can use MongoDB to quickly develop applications by using the features that are provided, such as data scaling, auto-sharding, and high availability. You can also use MongoDB to store different types of document objects, such as JSON files, with a key-value pair.

>In this tutorial you will learn how to use the MongoDB driver for Java to connect to your MongoDB service in Bluemix.

<br>

[Git Basics Tutorial](/git-basics)

>[Git](https://git-scm.com) is a version control system for software development.

>There are two ways to manage versions using Git.  Either through a series of commits or through the use of branches.

>In this tutorial you will learn how to use the version control of Git using commit and go back to a previous commit.

<br>

[Jetty Basics Tutorial](/jetty-basics)

>Jetty is a Java HTTP (Web) server and Java Servlet container. 

>In this tutorial you will learn how to deploy a web application packaged as a `.war` file in Jetty.

>**Prerequisite:**
>[Git Basics Tutorial](/git-basics) (recommended but not required)

<br>



[GitHub Basics Tutorial](/github-basics)

>[GitHub](https://github.com) is a Git repository hosting system that can be used as a remote Git repository.

>In the [Git Basics Tutorial](/git-basics) , you learned to do version control in a local Git repository (i.e., repository in your hard drive).  This allows you to go back to a previous version of your work.  However, using only a local repository prevents you to collaborate with a teammate efficiently.  For example, if you are working on  a Java code `Module1.java` and your teammate is working on `Module2.java`, the typical way of consolidating your work is copying through a USB drive or sending your code as an e-mail.

>In this tutorial, you will learn how to use GitHub as a remote Git repository in order for you to share your work with a teammate.

>**Prerequisite:**
>[Git Basics Tutorial](/git-basics)

<br>



[JUnit Basics Tutorial](/junit-basics)

>[JUnit](http://junit.org/) is a simple framework to write repeatable tests.

>In this tutorial you will learn how to create a simple test class that is used to test the methods of a Java class.

<br>



[Gradle Basics Tutorial](/gradle-basics)

>[Gradle](http://gradle.org/) is an open source build automation system.  It has a Java plugin to allow building Java applications and running tests for them.

>In this tutorial you will explore the different tasks available in Gradle.

<br>



[Gradle's Dependency Management Tutorial](/gradle-dependency-management)

>Gradle's dependency management allows quick resolution of library dependency.

>In this tutorial you will learn how to resolve library dependency using Gradle's dependency management.  In order to appreciate this feature of Gradle, you will first resolve the dependency problem using the manual approach.

>**Prerequisite:**
>[Git Basics Tutorial](/git-basics) (recommended but not required)
>[Gradle Basics Tutorial](/gradle-basics)

<br>


[Gradle's Unit Testing Tutorial](/gradle-unit-testing)

>Gradle's unit testing allows execution of test classes (e.g., those created using the JUnit library).

>In this tutorial you will learn how to run a test class created using a JUnit library in Gradle.

>**Prerequisite:**
>[JUnit Basics Tutorial](/junit-basics)
>[Gradle's Dependency Management Tutorial](/gradle-dependency-management)

<br>



[Creating a Web Application using Gradle Tutorial](/gradle-web-application)

>Gradle has an available `war` plugin that allows you to package your Java web application into a `.war` file.

>In this tutorial you will learn how to create a `.war` file.  The `.war` file will be deployed twice: locally (through a Jetty web server) and remotely (through [IBM Bluemix](https://ibm.biz/bluemixph)).

>**Prerequisite:**
>[Jetty Basics Tutorial](/jetty-basics) (recommended but not required)
>[Bluemix Basics Tutorial](/bluemix-basics) (recommended but not required)
>[Gradle's Unit Testing Tutorial](/gradle-unit-testing)

<br>


[Bluemix DevOps Services Basics Tutorial](/devops-basics)

>[Bluemix DevOps Services](https://hub.jazz.net) or simply Bluemix DevOps is a software as a service (SaaS) cloud technology from IBM that supports development, tracking and planning, and continuous delivery of software.

>In this tutorial you will explore the different features of Bluemix DevOps.

>**Prerequisite:**
>[Bluemix Basics Tutorial](/bluemix-basics) (recommended but not required)
>[GitHub Basics Tutorial](/github-basics)

<br>


[Bluemix DevOps Services Delivery Pipeline Tutorial](/devops-delivery-pipeline)

>[Bluemix DevOps Services](https://hub.jazz.net) has a delivery pipeline that allows you to build, test, and deploy your web application.

>In this tutorial you will learn to set-up a delivery pipeline by creating a build stage, a test stage, and a deploy stage.  The build stage will use Gradle.  The test stage will use JUnit through Gradle.  The deploy stage will use the cf tool.

>**Prerequisite:**
>[Bluemix Basics Tutorial](/bluemix-basics) (recommended but not required)
>[Bluemix DevOps Services Basics](/devops-basics) (recommended but not required)
>[GitHub Basics Tutorial](/github-basics) (recommended but not required)
>[Creating a Web Application using Gradle Tutorial](/gradle-web-application)

<br>
	

[Bluemix DevOps Services Track and Plan Tutorial](/devops-track-plan)

>[Bluemix DevOps Services](https://hub.jazz.net) has a track and plan feature to support Agile planning.

>In this tutorial you will learn how to use track and plan to create and monitor stories and defects in a web application project.

>**Prerequisite:**
>[Bluemix Basics Tutorial](/bluemix-basics) (recommended but not required)
>[GitHub Basics Tutorial](/github-basics) (recommended but not required)
>[Bluemix DevOps Services Delivery Pipeline Tutorial](/devops-delivery-pipeline)

<br>




