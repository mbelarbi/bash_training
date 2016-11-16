Functions
=========
Functions in bash serve the same purpose as all programming languages, they create a temporary command that does a unit of work. This unit can be as big or as small as you want. This unit of work is later invoked down the line zero or more times. the structure of a function is as follows:

	[ function ] name [ () ] { unit-of-work } [ redirection ]

the keyword function is optional, this is the n followed by the name of the function. The () are always empty in bash they do not server to take in arguements like some other programming languages, their purpose is to indicate that this is a function. This is then followed by the acutual unit of work inside {}. Finally we have any redirections we wish to add to our functions.

In bash you can choose to put the keyword 'function' or omit it, there is no difference whatso ever in bash, for example the following 2 functions are both valid in bash:
	
	function hello() {
		echo 'hello'
	}

	world() {
		echo 'world'
	}

The only difference is that functions defined without the `function` keyword are more portable to other non bash shells.
Because the brackets `()` only serve to tell us this is a function, we can also omit them if we choose to use the keyword `function`. We must however use at least one of the 2, either the keyword `function`, or `()` or both. The use of just the brackets is more commonly used. Completely up to you in what you choose.

arguements
----------
Arguements in bash are not named or decalred as part of the function definition. Function declarations are arguement agnostic, that is they dont care or not interested in your arguements, they will handle them as and when you need them. let's use an example to better demonstrate this:

	greetings () {
		echo "hello $1"
	}
	
	# lets call out function
	greetings Miloud

You refer to ordered and not named arguements in bash, this is done with `$n`, where n is a positive number. `$0` is reserved for the script call itself. The beauty of this is if you wish to handle extra args, you can do so easily without changing the function declaration, only the unit of work and the piece that calls it. The downside is that it can get a little hard to track or keep up with ordered arguements, named arguements are easier to handle and read.

Special arguement parameters
----------------------------
There are several augmentations or useful expansions that bash allows you to do with respect to the arguements passed in to a command.

* `$*`: Join all arguements with current IFS (internal field separator)
* `$#`: Number of arguements passed in
* `$?`: The exit code of the previous command (1 for failure, 0 for success)
* `$$`: The unique proccess identifier of the current shell process that is running

Calling functions
-----------------
Functions are simply called by using their name :) followed by any areguements. The only constraint is that a function must be declared before it is called. So in the above greetings function, I cannot call greeting before the function is even declared.
