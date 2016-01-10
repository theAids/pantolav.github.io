---
layout: post
title: Git Basics
permalink: /git_basics/
---

##Application Development Tutorial

###Git Basics
[Git](https://git-scm.com) is a version control system for software development.

There are two ways to manage versions using Git.  Either through a series of commits or through the use of branches.

In this tutorial you will learn how to use the version control of Git using commit and revert to a previous commit.


>**Prerequisite:**

>You are **required** to do the [Creating a Web Application using Gradle Tutorial](/gradle-web-application).

>- The sample code used in the current tutorial is based from the sample code used in [Creating a Web Application using Gradle Tutorial](/gradle-web-application). 

>You are not required (but **recommended**) to do  the [Bluemix Basics Tutorial](/bluemix-basics).

>- **However**, ensure that you have a Bluemix account.  
>- Your account should have the space `dev` under the region `US-South`.  The creation of the space `dev` is discussed in [Bluemix Basics Tutorial](/bluemix-basics).

>XXXYou are not required (but **recommended**) to do  the [Bluemix DevOps Services Basics Tutorial](/devops-basics).

>- **However**, ensure that you have a Bluemix DevOps Services account.

>XXXYou are not required (but **recommended**) to do  the [GitHub Basics Tutorial](/github-basics).

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

	<br>
	
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

	<br>
	
1. Create a file `animal.txt` with the following contents:

	```text
	ant
	bat
	cat
	dog
	eagle
	fox
	goat
	horse
	```

	<br>
	
1. Check the status of your local Git repository.

	```text
	> git status
	```

	**Output:**

	```text
	On branch master
	
	Initial commit
	
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)
	
	        animal.txt
	
	nothing added to commit but untracked files present (use "git add" to track)
	```

	Git detected the `animal.txt` file that you created.  However, it is currently untracked by Git.

	Git does not track all the files you created since it is possible that some of the files are non-essential.  You need to explicitly tell Git to track a file.

	<br>
	
1. Let Git track `animal.txt`.

	```text
	> git add animal.txt
	```

	If there are multiple files that you want Git to track, you may use the same `git add` command and just separate the files with a `space`.

	<br>
	
1. Check the status of your local Git repository.

	```text
	> git status
	```

	**Output:**

	```text
	On branch master
	
	Initial commit
	
	Changes to be committed:
	  (use "git rm --cached <file>..." to unstage)
	
	        new file:   animal.txt
	```

	At this point, `animal.txt` is being tracked by Git.  However, changes made in `animal.txt` is not yet commited.

	<br>

1. Commit the changes made in `animal.txt`.

	```text
	> git commit -m "added words from ant to horse"
	```

	**Output:**

	```text
	[master (root-commit) 9a29367] added words from ant to horse
	 1 file changed, 8 insertions(+)
	 create mode 100644 animal.txt
	```
	
	<br>

	Note that if there are other files that are being tracked aside from `animal.txt`, the changes made in those files will also be committed.

	<br>
	
1. Check the status of your local Git repository.

	```text
	> git status
	```

	**Output:**

	```text
	On branch master
	nothing to commit, working directory clean
	```

	At this point,  changes made in `animal.txt` is committed.  In the `git commit` command, you included a message `added words from ant to horse` to give a short description on what changes were committed.  This is very useful when you perform several commits later and you want to revert back to a particular commit.

	<br>

1. Visually look at the the details regarding the commit you performed.

	```text
	> gitk
	```

	**Output:**

	```text
	*master added words from ant to horse
	```	

	`gitk` is a tool which allows you to visualize the commits you performed.  You may use `gitk` as a guide when you need to revert to a particular commit.
	
	<br>

1. Update `animal.txt` to include words `iguana` to `pig`:

	```text
	ant
	bat
	cat
	dog
	eagle
	fox
	goat
	horse
	iguana
	jaguar
	kangaroo
	lion
	monkey
	newt
	octopus
	pig
	```

	<br>
	
1. Track and commit the changes made in `animal.txt`.

	```text
	> git add animal.txt
	> git commit -m "added words from iguana to pig"
	```

	**Output:**

	```text
	[master bf58ca9] added words from iguana to pig
	 1 file changed, 9 insertions(+), 1 deletion(-)
	```	
	
	<br>

1. Update `animal.txt` to include words `quail` to `zebra`.  In addition, delete `dog` and change `monkey` to `mouse`:

	```text
	ant
	bat
	cat
	eagle
	fox
	goat
	horse
	iguana
	jaguar
	kangaroo
	lion
	mouse
	newt
	octopus
	pig
	quail
	rabbit
	snail
	tiger
	uakari
	vulture
	wolf
	x-ray tetra
	yak
	zebra
	```

	<br>
	
1. Track and commit the changes made in `animal.txt`.

	```text
	> git add animal.txt
	> git commit -m "added words from quail to zebra, deleted dog, and changed monkey to mouse"
	```

	**Output:**

	```text
	[master 4444081] added words from quail to zebra, deleted dog, and changed monkey to mouse
	 1 file changed, 12 insertions(+), 3 deletions(-)
	```	
	
	<br>




####Undo Previous Commits

Undoing previous commits allow you to recover the contents of a file. 

1. Get a summary of the commits performed in the local Git repository.

	```text
	> git log
	```

	**Output:**

	```text
	commit 444408167f0a81c4684d8ba5d32017455c3b562d
	Author: Alexis V. Pantola <pantolav@gmail.com>
	Date:   Sun Jan 3 15:39:05 2016 +0800
	
	    added words from quail to zebra, deleted dog, and changed monkey to
	commit bf58ca94a64c0b71d84aee72cdfefa7789714a01
	Author: Alexis V. Pantola <pantolav@gmail.com>
	Date:   Sun Jan 3 15:30:11 2016 +0800
	
	    added words from iguana to pig
	
	commit 9a2936788cecdce40f7c9abb2602f7c5129dcb92
	Author: Alexis V. Pantola <pantolav@gmail.com>
	Date:   Sun Jan 3 12:16:55 2016 +0800
	
	    added words from ant to horse
	```

	>You may press `Q` to exit the log console.

	Notice that the three commits performed earlier are listed in the log.  In addition, there are unique hash values identifying each comit.  As an example, the commit `added words from ant to horse` has a hash value of `9a2936788cecdce40f7c9abb2602f7c5129dcb92`.  

	The hash value is important to undo previous commits and go to a particular state of a file.

1. Go to the state of the file when the words ant to horse has just been added.

	```text
	> git reset --hard 9a2936788cecdce40f7c9abb2602f7c5129dcb92
	```

	**Output:**

	```text
	HEAD is now at 9a29367 added words from ant to horse
	```

1. Open (or reload) `animal.txt`and verify that its content is now the following:

	```text
	ant
	bat
	cat
	dog
	eagle
	fox
	goat
	horse
	```

####End of Tutorial


####What's next?




