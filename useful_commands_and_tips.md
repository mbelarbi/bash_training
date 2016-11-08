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

search history
--------------
`Ctrl+r`. Search your bash history as you type.

Brace expansion
---------------
Brace expansion can be very useful for things like cartesian product. lets look at a few examples:

	echo {Mon,Tues,Wednes,Thurs,Fri,Satur,Sun}day
	# Monday Tuesday Wednesday Thursday Friday Saturday Sunday
	
	echo a{b,c,d}e 
	# abe ace ade
	
	echo {a,b,c}{d,e,f} 
	# ad ae af bd be bf cd ce cf

It can also be used for sequencing (using the dot dot `..` notation):

	echo {1..10}
	# 1 2 3 4 5 6 7 8 9 10
	
	echo draft{1..5}.txt
	# draft1.txt draft2.txt draft3.txt draft4.txt draft5.txt
	
	echo {a..z}
	# a b c d e f g h i j k l m n o p q r s t u v w x y z

Also sequencing patterns:
	
	echo {1..20..2}		# number 1 to 20 every 2nd number
	# 1 3 5 7 9 11 13 15 17 19

	echo {a..z..3}		# letters a to z every 3rd letter
	# a d g j m p s v y
