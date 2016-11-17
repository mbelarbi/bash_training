Lists 
=====
	command1 co command2 co command3 ...

Lists are just a sequence of 2 or more commands. A bash script is a list, many commands one after the other. The important thing with lists is that there is a control operator (co) that separates them and tells bash how to run them when in sequence. Writing a bash command is fairly straight forward, the get the most of bash it is important to understand how we connect 1 or more comamnds together. This is why control operators are important.


Control operators
=================
Control operators are the glue for a list of commands. There are several list operators in bash:

Semicolon ;
-----------
This is the most basic co, it just runs the commands one after another irrespective of the outcome of the first. This is equivalent to starting a new line in a bash script. The only subtle difference is that with a semicolon, bash has to continue to read in and parse all the commands separated by the semicolon before it begins executing any of them, whereas with a new line, bash executes then starts parsign the next command.

Ampersand &
-----------
This allows you to run a command in the background and start another command in the foreground without waiting for the first command to finish, i.e:

	command1 & command2

Here we are running command1 and immediately starting command2 without waiting on command1.

Double ampersand &&
-------------------
The double && is used as an AND control operator for commands. It is used to run a command if and only if the command that proceeded it finished successfully (i.e exit code 0).

	command1 && command2

Command2 will run after command1, but only if command1 finished successfully. Unlike the single ampersand operator, both command1 and 2 are run in the foreground. The double ampersand is described as:

	if command1; then
		command2;
	else false
	fi

Double pipe ||
--------------
This is the OR operator for commands. In other words, this will run a command only if the one before it failed.

	command1 || command2

Command2 will only run if command1 failed. Becuase this is an OR operator, there is no need to run command2 if command1 was successful.

task: How would you write the || operator using an if block?

Both && and || are left assertive, that is they will always start by running the command to their left first and then decide what to do based on the outcome.

Bang !
------
This is the NOT operator. Simple, just negate the outcome exit status of a command.

	! command1

This just returns an exit code 0 if the actual exit code of command1 is a non zero (i.e unsuccessful), and returns an exist code 1 if command1 was successful.

Usign the Not operator, we can desscribe the OR operator as follows:

	if !command1; then 
		command2; 
	fi

Double semicolon ;;
-------------------
This is used when writing switch/case statements, `;` is used to mark the end of a case statement. There is no other instance where you would use a double semicolon other than cases (as far as I know). See [part 8](part8_if_and_case.md) for an example of  case statements.

Brackets () {}
---------------
Brackets, as with in many other domains, serve to group things together. Bash is no different. Why would you want to group commands together? In the case where you are writing a squence of several commands joined by one of more control operators and it might be handy to group some together and uniqfy their output before passing it on to another command.


Redirections
============
We've covered the file descriptors and how they tie in to standard in, standard out and standard error. We've also briefly touched on redirection to see out we can send the output of one or more commands somewhere other than the default standard out. Lets get more specific with redirection.

Greater than >
--------------
This sends output of a command somewhere else:

	echo 'hello world' > file.txt

If file.txt doesn't exist, it will be created. If the file does exist, then its contents will be overwritten.
This is also often used to determine what file descriptors should go where:

	command > out.txt 2 > error.txt

This basically sends the FD1 (standard output) to the out.txt file and any error that occur (FD2) will be sent to the error.txt file. This is handy when you want to split specific kinds of output from your scripts.

Double grater than >>
---------------------
This is the same as the regular > with the only exception being that it does not overwrite the contents of the file, instead it appends to to it:

	command > out.txt 2 >> error.txt

We are sending the output of the command to out.txt (overwriting its content), whilst at the same time we are sending any errors to error.txt but appending to the error log.

Less than <
-----------
This give input to a command:

	grep 'hell' < file.txt

This will run the grep command on the contents of the file.txt.

Greater than and less than <>
-----------------------------
This is basically the same as the less than redirection, but instead it opens the file for both read+write operations. It's not common because rarely would you need to both read from and write to the same file on a single command, but it is good to know :)

Triple less than <<<
--------------------
This is used to provide input from the shell into a command rather than a file, for example:
	grep 'hell' <<< 'hello world'

Ampersand redirection &>, &>>
-----------------------------
The ampersand redirects both standard out and standard error, either overwriting (single >) or appending (double >>) respectively.


**IMPORTANT NOTE**: All redirections are interpreted before any commands are run. Here is a task scenario to explain the importance of this more clearly.

	echo 'hello world' > file.txt
	
We now have a file with the contents 'hello world'. Brilliant, all good so far.

	cat file.txt | grep 'hell' > file.txt
	
What do you expect the contents of file .txt to be? 

file.txt is actually empty. Why? Thats because bash handles redirections before commands. In other words, redirection of the output from the grep command causing file.txt to the opened for writing and because we are using a `>` we have truncated the contents of the file ready for writing. Bash then cats the file (it's empty remember), the pipes the output to grep and finally storing that output into the same file. 
