If
==

The if staement is a standard conditional test. It tests if some condition is met, and executes a unit of work.

	if [ some condition is true ]; then
		do some work
	fi

The above is the basic structure of all if statements in bash. We can also do another unit of work if the condition is not true, using `else`.

	if [ some condition is true ]; then
		do some work
	else
		do some other work
	fi

We can further expand this to include multiple conditions that each have their own unit of work:

	if [ some condition is true ]; then
		do some work
	elif [some other condition is true ]; then
		do soem other work
	.... # as many elif blocks as needed
	else
		finally do some this work
	fi
	
If statements are linear. The first condition met is evaluated and no other. lets look at an example:

	day=5
	month=8
	year=1995
	
	if [ $year -lt 2000 ]; then
		echo "before millennium"
	elif [ $month -gt 5] && [ $month -lt 9 ]; then
		echo "summer time"
	elif [ $day -lt 8 ]; then
		echo "first week of the month"
	else
		echo "hi :)"
	fi
		
This is considered as one if statement. The unit of work for the first condition satisfied is executed. Even if subsequent condition might be true, bash will never get round to evaluating them since a condition has been satisfied. In the above example, bash will echo `before millennium`. Subsequent conditions are only evaluated in the order they are written if the previous conditions fail.

The `else` has a hidden condition that you do not need to specify: `if all previous conditions are false`. `else` **CANNOT** be used before `elif` statements in the same if block. It is a syntax error to do so.

AND
---
This is a way to ensure multiple conditions are met for a unit of work to be done, this is done using `&&`:

	if [ $colour == "orange" ] && [ $type == 'citrus' ]; then
		echo "this could be an orange"
	fi
		
Both the conditions MUST be met for the unit of work to be executed. If the first condition is not met, bash will not bother finding out if the `$type is citrus`.

OR
--
Similar to `AND` except if any of the conditions are met, the unit of work is executed, this is doen using `||`:
	
	if [ $day == "christmas" ] || [ $day == "birthday" ]; then
		echo "time for gifts"
	fi

If the first condition is met, bash does not bother evaluating the conditions that follow, since this is an `OR`.
