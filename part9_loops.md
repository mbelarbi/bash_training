Loops
=====

Loops in bash are similar to other programming languages, they iterate over a list, collection or some sort of data structure. All loops have 3 main components:

  - Condition: there will be no iterations (and hence no work done) unless this is true when the loop starts
  - Unit of Work: the piece of work we want repeat every time the condition holds true. This is the body of the loop.
  - Exit clause: the trigger that allows us to stop doing the unit of work (otherwise we'll be there forever).

There are 3 kinds of loops in bash.

while loop
----------
The while loop is designed to do some piece of work given that a specific condition is met. On every iteration, a while loop basically runs an internal if statement to determine whether it should continue the next iteration or not.
	
	while [ some condition is true ]; do
		some work
	done
	
> **Important:** It is our responsibility to ensure the while loop has an exist clause. Our exist clause is usually the condition faling, but on occasions we can have manual forced intervention (we'll cover this in loop control)

Let's look at an example:

	apples=5
	while [ $apples -gt 0 ]; do 	# semicolon indicates there are 2 lines here
		echo "I have $apples apples, lets donate one"
		let apples=apples-1			# let is a bash builtin, like echo. THIS ALSO TRIGGERS OUR EXIST CLAUSE
	done

Lets break this down:
  - Condition: we have more than 0 apples. This loop will not do its piece of work unless we have more than zero apples
  - Unit of work: we echo how many apples we have, and programmatically donate one by subtracting it away.
  - Exit clause: we have zero or less apples: the fact that we subtract an apple on every iteration, we guarantee our exist.
  

for loop
--------
The `for` loop is designed to iterate over data structures. More specifically it iterates exactly the length of the data structure you are looping over, whilst providing you access to each item on each iteration via a local variable. Let's loop at an example:

	days_of_the_week=("Monday" "Tuesday" "Wednesday" "Thursday" "Friday" "Saturday" "Sunday")
	
	for day in ${days_of_the_week[@]}; do		
		echo $day						# day is out local variable that gives us access to each variable 
	done								# we're done. close the for loop
	
> **Task:** What are the 3 main components (condition, unit of work and exist clause) if the above example?

> **Task:** Can you rewrite the above `for` loop using a `while` loop? (this will let you see the 3 main components more clearly)


until loop
----------
The `until` loop is very similar to the `while` loop in structure, with the exaception being that the the loop continues to iterate `until` the condition is met. Somewhat of a reverse while loop. Let's look at an example:

	apples=0
	until [ $apples -gt 5 ]; do 	
		echo "I have $apples apples, I'll stop taking apples until I have more than 5"
		let apples=apples+1		
	done
	
To better understand this, we need to negate the condition. in otherwords, the `until` loop thinks in the following way: 
	
	"I'm going to continue to do the unit of work, until some condtion is met". 

That is to say the exit clause is the condition being true. and the condition for doing the loop is if the `condition` is not true.


Loop control
============
Some times we need to prematurely exit out of a loop (even if our exit clause has not yet been met), or we need to skip the current iteration and go on to the next.

break
-----
This quits the current loop regardless of condition, exit clause or any other circumstance.

    apples=10
    while [ $apples -gt 0 ]; do
		if [ $apples -eq 1 ]; then
			echo "I only have 1 apple left, this one is for me"
			break
		fi
		
        echo "I have $apples apples, lets donate one"
        let apples=apples-1
    done
	
Here we can see that a break if introduced to quit out of the loop before the loops exist clause is met.

continue
--------
This allows the loop to squit execution of the unit of work for the current iteration.

	numbers=(1 2 3 4 5 6 7 8 9)
	# echo the even numbers
	for n in ${numbers[@]}; do
		if [ $(( n%2 )) -ne 0 ]; then
			continue
		fi
		echo "$n"
	done

Looping over our array of numbers, if the number is not divisible by 2, we want to skip it, but not quit the entire loop.


Exercise
--------

Write a simple bash script that continues to talk to the user and reply based on the user's input. The script should only stop talking once the user says `bye`.



