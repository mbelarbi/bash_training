Arrays
======
Arrays in bash are like arrays in other languages. Basically containers for a set of data.

	days=("Monday" "Tuesday" "Wednesday" "Thursday" "Friday" "Saturday" "Sunday") 	#method1
	
This is a simple array of the days of the week stored in a variable called `days`. a few things to note:

 - The delimiter is a space.
 - Arrays in bash are zero indexed
 
But how do we access the array's items?

	echo "The second item is $days[1]" 		# This is WRONG
	echo "The second item is ${days[1]}" 	# This is RIGHT
	
When accessing the elements in an array within an expansion we must use the inline grouping (`{}`). With out the inline grouping,
the `[1]` is not considered as part of the expansion, as such bash will interperate it as a string literal.
The array variable will always yield the zeo indexed item when called with no index (i.e. if we call `$days` with no index). Therefore the echo will look like:
	
	The second item is Monday[1]		# no inline grouping
	The second item is Tuesday			# with inline grouping
	
Index keys
----------
We can also create arrays using specific indexes:
	
	days=([6]="Sunday" [0]="Monday" [3]="Thursday" [1]="Tuesday" [4]="Friday" [2]="Wednesday" [5]="Saturday")	#method2
	days=([0]="Monday" [1]="Tuesday" [2]="Wednesday" "Thursday" "Friday" "Saturday" "Sunday")					#method3
	
The above 2 arrays along with the initial one we created are all identicle.
Although it's usually not best practice to use method 3 (mixture of index keys and no index keys) it still creates our desired array, thursday is assigned index 3, friday 4, saturday 5 and sunday 6.
When an element in an array is declared with no index (as in `#method1` and `#method3`) it follows these rules:
  
  - If it's the first item it will take index 0
  - It will use a ++ index notation: that is it will take the index on the previous element's item and increment by 1.
  
Lets take a closer look at how this is enforced by bash:

	days=("Monday" [2]="Wednesday" [1]="Tuesday" "Thursday" "Friday" "Saturday" "Sunday")
	
 - "Monday": This has no key, it is also the first element and so bash assigned it index 0
 - [2]="Wednesday": This has been given an index key of 2
 - [1]="Tuesday": Again this has been pre assigned the index key 1
 - "Thursday": This has no index key and it is not the first element. Bash will take the index of the previous element, in this case `1`, and increment by 1. We therefore have index key 2 for Thursday.
 - "Friday": No index key. Not first item, lets use the i++ notation to assign an index key of 3.
 - "Saturday": same as Friday, index key 4
 - "Sunday": same and above, index key 5
 
Two major bugs have been introduced:

 - 1: We have overwritten `[2]="wednesday"` with `[2]="Thursday"`
 - 2: Our array appears as though it has 7 elements, but due to the above point, we lose the `Wednesday` element, and infact we actually have 6 elements.
 

Quotation
---------
We could define our days array as follows:

	days=(Monday Tuesday Wednesday Thursday Friday Saturday Sunday)
	
Bash identified the words separated by spaces. This is a clean example, but there would be instances where elements aren't simple words (i.e file directories, have symbols etc). Which is why It is recommended to always quote your elements to reduce the chance of errors happeneing. Quotes can be single or double (double for expansions), i.e:

	mon="Monday"
	weekends=('Saturday' 'Sunday')
	days=("$mon" 'Tuesday' 'Wednesday' 'Thursday' 'Friday' "${weekends[0]}" "${weekends[1]}")
	
Monday, Saturday and Sunday need double quotes for the expansion. Saturday and Sunday further need inline grouping for the array access.
	
	
