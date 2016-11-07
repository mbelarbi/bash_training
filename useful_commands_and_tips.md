useful commands
===============

Bang bang
---------
That's `!!`. It repeats the previous bash command line.

where am i?
-----------
`pwd`
make sure you know exactly where you are before running any program that cannot be undone (i.e an rm -rf)

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
