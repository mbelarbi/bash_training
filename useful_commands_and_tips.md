Useful commands
===============

Bang bang
---------
That's `!!`. It repeats the previous bash command.

where am i?
-----------
`pwd`
make sure you know exactly where you are before running any program that cannot be undone (i.e an rm -rf)

go back
-------
`cd -` takes you back to the previous directory you were on. (Not the parent directory of where you are)

incognito
---------
start a command with a space and it will not be written in history

make full dir
-------------
`mkdir -p a/long/directory/path`

`-p` tells mkdir command to create the directory structure if it doesn't exist.

10 biggest files/folders
------------------------
`du -s * | sort -n | tail`

recursively remove all empty directories
----------------------------------------
`find . -type d -empty -delete`

Exit appropriately
------------------
`exit 0`: Put that at the end of your scripts.

`exit 1`: Use this to indicate an unssucceful exit.

Silver Searcher
---------------
Use this instead of grep. The silver searcher is a faster, prettier and easier to use version of grep.
https://github.com/ggreer/the_silver_searcher

word count
----------
`wc` is amazing. it counts number of lines, words and characters in a given file.

sleep
-----
`sleep 5` simple and straight forward, just hang for 5 seconds :)

Command substitution
--------------------
This is the process of assigning the output of a command to a variable by enclosing it in parentheses:

	today=$(date +%d-%b-%Y)
	echo $today
	
We have substituted the variable `today`, for the date command. You can put a full blown bash command wit hpipes and everything inside those brackets and assign it to a variable.
	
	
