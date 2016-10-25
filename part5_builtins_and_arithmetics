Command name priority
=====================
Lets recap the basic structure of a command in bash:

	[ var=value ... ] name [ arg ... ] [ redirection ... ]
		P1		P2		P3

The command name is the key in all the parts, without it the rest is useless. It is also what bash first looks at to try and identify what it is you want it to do. It gets tricky once there are multiple commands that share the same name. The order of priority that bash uses to determine what exactly to call is as follows:

	1) functions
	2) aliases
	3) builtins
	4) external commands

Functions you declare in your scripts always take priority. When a function is declared, bash puts the name of the function in a list in memory, it then searches this list first and formost when ever a command is run. After functions, any aliases that match the command name are used instead. Builtins are next in the pecking order, they are basically a bunch of builtin commands/function into bash. We'll look at these later. Finally external commands are last, these are system programs are looked up in the 'PATH'.

If after bash has exhausted all efforts to find the name of the command you want to run and still can't find anything, the nit will issue a command not found error.

Built ins
---------
These are what it says on the tin, commands built into bash, some are very common and some are uncommon, lets take alook at some common ones:
echo, cd, pwd, history and if then else.

to get a complete list of all builtins, just type help on the bash shell, to get more information and optional arguements for a specific builtin, just type help followed by the built in.


external commands
-----------------
These are other programs installed on the system. i.e vim, python, mysql, psql etc. Bash uses the PATH variable to identify the location of any external programs. each lookup location is separated by a colon.
	echo $PATH

When bash defaults to using an external program (i.e. the command name wasn't found as a function, alias or builtin) it will search each PATH location and stop at the first location it finds. Bash then stores this location so the next time you use the external program, it nows where to go and fetch it from.

So how do you know if something is a function, alias, builtin or an external command???? I could create a function called echo, and it will always be used instead of the builtin echo. the best way to check is to use type:
	
	type command-name

task 1: write a conditional block that echos YES if echo is a builtin, otherwise it echos NO to the screen.
task 2: Modify the script to write the result to a file


Arithmetics operations
======================
lets echo to the screen a simple 2+2
	echo 2+2

Whats wrong with this? how does echo now we want to compute 2+2 and print the result to FD1 rather than send to FD1 the actual string '2+2'?
we can do this in one of 2 ways:
	echo $[2+2]
	echo $((2+2))

Bash's arithmetic is strictly limited to integers, no floats of decimals. this means that echo $[3/2] will result in 1 and not 1.5. if you wish to do decimal arithmetic you will have to deligate to another tool, i.e python :)

	python <<< 'print 3.0/2.0'

