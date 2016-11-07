Strings and quotes
==================

There are 2 ways to use strings in bash:
* `'hello world'`
* `"hello world"`
	
Unlike some programing languages which don't mind which kind of quotations you use, bash does to some degree.
This becomes important whe nyou need to add variables as part of strings, lets demonstrate this with an example:

	echo 'my path is: $PATH'
	echo "my path is: $PATH"
	
The single quotes will echo the literal string $PATH, whereas the double quotes will evaluate it as a variable.
This is also the case when handling arguements (`$1` etc).

Using braces
------------
Sometimes it can be useful to include braces in variable expansions, for example:

	time=23
	echo "your current record is $times"
	
We want to add a `s` to the end of the time variable (indicating seconds), however bash tries to find the variable `times`.
to tell bash that the `s` isa literal string and not part of the variable name we do it using braces:

	time=23
    echo "your current record is ${time}s"

arguement strings
-----------------
passing an arguement to a command is done simple by succeeding the command with the arguements themselves separated with spaces.
for example if we wish to delete multiple files we can do this as follows:

	rm file1.txt file2.txt 
	
Now suppose one of our arguements (in this case the filenames) have a space in them:

	rm file1.txt file2.txt 'final file.txt'
	
Notice how we have to quote the entire arguement, otherwise bash wil interpret the space between `final` and `file.txt` as syntax and not literal.
We can alternatively use backslashes to escape the space:
	
	rm file1.txt file2.txt final\ file.txt
	
But I personally think this is less readable. Having several of thsese can make the command cryptic.
Quotes are always better than no quotes, if things get messy, always quote your data. 
*If there is a space or symbol in your data, you must quote it. And always double quote any variable or arguement expansion*.

Lets demonstrate the importance of double quoting variable expansions:
	
	read -p 'what document file would you like to remove? ' filename
	
	rm -r home/miloudbelarbi/Documents/$filename
	
If the `IFS` (internal field separator) is not set to bash's default new line character and instead set to nothing, and you enter ` file1` (that is space and then file1),
the `rm` command will remove all files not just file1, becuase it might expand `$filename` as follows: `rm -r home/miloudbelarbi/Documents/ file1`.
Notice the space before `file1`, to eliminate this nasty bug we should wrap our variable expansions in double quotes:

	rm -r "home/miloudbelarbi/Documents/$filename"
	
Yes you could write and if statement to check the IFS, but why not just use double quotes? :)

Length of String
----------------
unlike most languages, there is no function for finding the length of a string, instead we use the `#` special character (see btable below)

	name='Miloud'
	echo "there are ${#name} characters in '$name'"		# there are 6 characters in 'Miloud'
	
Substring
---------

	name='Miloud'
	fullname="$name Belarbi"
	echo "${name:2}" 		# loud
	echo "${name:1:3}"		# ilo
	echo "${name:0:-2}"		# Milo
	echo "${name#M*l}"		# oud (From the front of the string, remove the shortest match)
	echo "${fullname##M*d}"		# Belarbi (From the front of the string, remove the longest match)
	echo "${name%o*d}"		# Mil (From the back of the string, remove the shortest match)
	echo "${fullname%%B*i}"		# Miloud (From the back of the string, remove the longest match)
	
String replacement
------------------

	name='Mi name is Miloud'
	echo "$name/Mi/Be"		# Be name is Miloud (Replace the first occurance of 'Mi' with 'Be')
	echo "$name//Mi/Be"		# Be name is Beloud (Replace all occurances of 'Mi' with 'Be')
	
Special characters
==================
Bash considers the following as special:

|Character| Description													|
|---------|-------------------------------------------------------------------------------------------------------------|
|`" "`	  |Whitespace: This would be a space, tab, new line etc. Bash uses this to determine where words start and end  |
|`$`	  |Expansion: Used to evaluate variables and arguements								|
|`''`	  |Single quotes: used encapsulate strings inside as literals. special characters are ignored			|
|`""`	  |Double quotes: same as single quotes but allows for variable expansions					|
|`\`	  |Backslash: escape special characters										|
|`#`	  |Comment. Also used for length in variable expansion								|
|`[[]]`	  |conditional test: evaluate soemthing to either true or false, i.e. [[ 1 > 0 ]] evaluates to true		|
|`!`	  |Negate: reverse the result of a test or command								|
|`>`, `<` |Redirection: redirect the input or output of a command							|
|`|`	  |Pipe: special redirection, redirect the output of one command as the input of another			|
|`;`	  |Command separator: separate 2 bash commands									|
|`{}`	  |inline group: group multiple commands as though they were one command					|
|`()`	  |subshell group: same as above but the group is executed in another shell and the result being used inline	|
|`((  ))` |Arithmetic expression: evaluate and arithmetic expression using +, -, *, /, <, <=, >, >=			|


> **Task:** Can you think of why we need double brackets for arithmetic expressions?

take 2 if statements:
	
	age=14
	# one of no set of brackets: THIS IS WRONG
	if ( age > 18 ); then
		echo "you can vote"
	fi
	
	# double brackets: THIS IS RIGHT
	if (( age > 18 )); then
		echo "you can vote"
    fi
	
In the 1st example, bash will try to do a redirect, i.e redirect age to a file called 18. To tell bash we want to evaluate the arithmetic expression of 'greater than', we need double brackets `((  ))`. Alternatively we can use the `-gt` (greater than) notation:

	if [ age -gt 18 ]; then
		echo "you can vote"
    fi
	
Since this is not an arithmetic expression, rather a syntax specific evaluation, we do nto need the doubel brackets `((  ))`
