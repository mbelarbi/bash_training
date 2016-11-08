What is bash
============
Simply put bash is a program. 
More specifically, Bash is a shell program, this is important to know. Because unlike most other programs, a shell program is an interface to interact with other programs. In other words shell programs let you invoke or do things with other programs. There are many shell programs:

- Z shell (zsh)
- C shell (csh)
- Korn shell (ksh)
- Debian's Almquist shell (dash)
- Born again shell (bash) - The most popular and widely available shell.

They function very similar to once another but it's still important to know which one you are dealing with. We're dealing with bash :)


What do we do with it
---------------------
It's designed to take input from you (commands) and give your the results or the output of your commands. In otherwords bash lets you interact with your system (and it's programs) using a text based interface.


Modes
-----
When using bash, it's important to understand that it supports 2 modes of operation:

- Interactive mode
	This is when the bash shell waits for your commands before performing them. When a command is being executed by bash, you CAN NOT  interact with the shell. Once the command is finished, bash waits for you to issue other commands.

- Non-interactive mode
	This is when bash executes scripts. a pre-written set of commands where bash doesn't wait for your input or ask you what to do next.


Before you start bash
---------------------
How do you know if you really are running bash and not another shell program?

	echo "$BASH_VERSION"

This should output the version of bash you are currently running. If there is no output or you get an error, chances are you aren't using the bash shell.

The quickest way to initiate the bash shell is to simply run the command:
	
	bash


Bash I/O
========
Because you can only communicate with bash via a text-based interface we need to understand how the flow of communication goes:

From a high level point of view:

    Keyboard, mouse display   <----->   os gui   <----->   terminal (your text-based interface)  <---->  Bash 


From a low level point of view bash has atleast 3 forms of I/O (ways for you and bash to communicate with each other), these are called File descriptors:

- Standard input
	This is "File descriptor 0". This is how most commands run on bash receive their input, by default this is yoru keyboard.

- Standard output
	This is "File descriptor 1". This is how most commands you run on bash send you their output. By default its your screen.

- Standard error
	This is "File descriptor 2". This is how commands report errors to you. By default it's your screen again. But it's important to know that this is just another interface, we could easily connect it to an error file and all errors will be sent to the file while regular standard outputs will still be sent to the screen.


A bash process can have any number of these file descriptors, it can create as many as it needs and connect to them as it sees fit. These 3 are just the standard basic ones.


Exercise:
---------

Where do we commonly see file descriptors?
	
	the crontab

Ever see something like this:
	
	/path/to/my/amazing/script.sh 2>&1

what does the `2>&1` mean?

both 2 and 1 are file descriptors, 1 is the stdout and 2 is the stderr. what we are saying is we want to run the command and we wish to move all stderr outputs to the file descriptor 1 output which is stdout. In other words we want all errors to be displayed through the regular output.

So why not `2>1`, why do we need the `&`?

The `&` tells bash that what follows is a file descriptor, without it, the errout (from file descriptor 2) will be written to a file called '1'


Why are File descriptors important
==================================
Because the basis of all our bash commands and processes rely on one or more forms of inputs and one or more forms of outputs. We need to understand where the flow of our data is going to and where it's coming from.
you can write a bash process that sends it's errors to an error file while its regular output is passed as the input (FD0) for another command.

Each time a program is started or invoked by bash, bash creates a running process for it. Processes have plugs, called file descriptors which allow them to connect streams that lead to files, devices or other processes.


NOTE: it's important to know that File descriptors are specific to a process. That means even if you write a bash command that inturn invokes 2 separate processes, the file descriptors for each process are different. for example, lets brake this down from a file descriptor point of view:

	 echo "hello world" | grep "hell" 2>&1 > file.txt

How many processes? 

- 2 the echo and the grep processes*
- the echo process has input = keyboard,
- the grep command has input = the output from the echo command
- the grep command has error output = the screen
- the grep command has standard output = file.txt

Example usage of file descriptors
---------------------------------
	
	# Redirect standard out (FD1) and standard error (FD2) separately
	command >stdout_redirect.log 2>stderr_redirect.log

	# Redirect standard error and out together
	command >stdout_redirect.log 2>&1

	# Merge standard error with standard out and pass them both to another command
	command 2>&1 | command2

Exit code
=========
 - A successful bash command exists with code 0.
 - An unsuccessful bash command exists with a code other than 0 (mainly 1).
 - An exit code of 3 or 103 indicates that the command failed, but the details are program specific. So for example if we run a `psql` command in bash and it fails, we might get an exit code 3, where the details of the failure are handled by the failing program, in this case `psql`.
	

