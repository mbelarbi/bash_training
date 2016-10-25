Bash commands
=============
Bash needs instructions from you. It does what you tell it and only what you tell it. It's a simple step by step interpreter. That is it takes one command at a time and executes it, during which you are unable to interact with it or give it more commands until it has finished its current command.

Some commands run quick and some take a long time, and some will not end unless you end them, this is sometimes the case when you launch an application from bash.

Bash reads commands 1 line at a time, i.e:

	echo 'hello world'  # this is one line and is executed immediately
	
	if [[ 1 > 0 ]]; then  # the if statement started but not complete so bash cannot run this yet
		echo 'yes';
	else
		echo 'no';
	fi  # only once 'fi' has been issued does bash run this as a single command.

The if block is a compound command, that is there is no specific output or result from doing an if, it ust groups a bunch of smaller logical commands.

Bash will continue asking for more information until it has enough to execute the command, we are doing this in the interactive mode of bash. alternatively we could just put that if block in a file and run the script, this is the non interactive mode.

Hash bang #!
------------

In non interactive mode (i.e writing the commands in a file), the commands are just a list of commands, not a bash script. For it to truely be a bash script we need to tell the kernel that runs that file how it should execute it (right now it could be anything, a list of python commands, a shopping list etc). at the top of any bash script we should explicity indicate that we want bash to run this, we do this by:

	#!<path to bash>

in most bash scripts you might see this as `#!/bin/bash`. The only problem with this is bash might not be installed on your system in that directory. it better to use the env command to locate bash for us so instead we should do:

	#!/usr/bin/env bash

env finds and starts other programs, in this instance we tell env to find and start bash, this is better than universally assuming a bash path, sicne not all machines and servers have bash installed in the same location. It's better practive to use `#!/usr/bin/env bash`.


Structure of a bash command
===========================

	[ var=value ... ] name [ arg ... ] [ redirection ... ]
			P1				P2					P3
			
The simplest of bash commands is made up of 3 parts (above as P1, P2 and P3). Everything inside squared brackets is optional. That means everything except the command name itself is optional. Let's break this down into the 3 parts, starting with P2 since it's the easiest.

P2 (name [ arg ... ])
---------------------
This is the main part of the command, you would need to tell bash what is the name of the command you wish to run and the commands optional arguements, an example of this would be:

	ls -l -a -h

ls being the command name and -l, -a, -h being optional arguements to the command ls.
optional arguements that take no variables can be grouped under a single hyphen (-), i.e: 
	
	ls -lah 

P3 ([ redirections ... ])
-------------------------
Redirection are operations applied to a command, they change what the file descriptors plug into, a simple example of this is changing the FD1 (that's stdout) to plug into a file instead of the screen. i.e:

	ls -lah > my_files.txt

Instead of displaying the files/directories in the present working directory, we output it to a file called my_files.txt instead. Here we have told bash that we want to use a different FD1 to the default.


P1 ([ var=value ... ])
----------------------
This lets you optionally assign a few variables before issuing the command itself. These variables apply only to the command's environment. for example:

	#!/usr/bin/env bash	
	DIR='/home/miloudbelarbi'
	ls $DIR -lah

Here we have assigned the variable DIR and used it in the ls command

Variables are powerful in any programming language, and bash is no different, they let us reuse common values in our scripts. When talking about variables we musn't ignore 'local' variables. These are indicated by the keyword 'local', for example:

	#!/usr/bin/env bash
	DIR='/home/miloudbelarbi'
	function lsdir {
	    local DIR='/home/miloudbelarbi/projects'
	    ls $DIR -lah
	}

	ls $DIR -lah
	lsdir

Pipelines
=========
	command1 [ | OR |& ] command2


In the real world pipes serve to take something, from one place and transfer it to another place. It's exactly the same in Bash. The pipe is oneof the easiest, most common and most powerful control operators in bash. We'll cover control operators in the next section.

The pipe is a way to connect 2 comamnds together by passing the FD1 of command 1 as the FD0 of command 2. (That is the standard output of command 1 is used as the standard input of command 2). For example:

	echo 'Hello world' | grep 'hello'

this is broken down in 2 commands, command1 (the `echo` command) and command 2 (the `grep` command). The standard output (FD1) of the `echo` command is used as the standard input (FD0) of `grep` command.

A pipe redirect can also be issued as `|&`. This is the equivalent of `2&>1` that we talked about previously. This means that we not only want to pass the standard output (FD1) but also the standard error (FD2) both as the standard input (FD1) of the grep command. Sometimes this can be undesirable unless the second command is prepared to handle any error the first command will throw. here is an example of this:

	find -name 'myfile.*' |& grep -v "Permission denied"

Task: why do we need `|&` and not the regular `|` ?


