My Developer Notes

Caveats wrt. traditional BASICs:
================================

PRINT is a function call, not a statement (mostly because if used as an embedded scripting language there may not be anywhere to print to).

Comments use hash symbols rather than single quotes. This is partly because I wanted single quotes to have the same meaning as double quotes, as in Javacript.

REM is not supported for comments.

Statements found in some old BASICs that are not available include CLS, LOCATE, WHILE-WEND and so on.

There aren't any arrays, only hash tables, and they are indexed using [] instead of ().

DIM is not needed (and not implemented) because of the way these hash tables are implemented.

'&' is the string concatenation operator.
'~' is the not-equals operator.

DATA is also a function, rather than a statement.

The operator @ operator is for /references/. The expression @identifier converts the identifier to a string, so @ARRAY$ will be equivalent to "ARRAY$". 

It is useful, because functions like DATA() and MAP() takes as a first parameter the name of the variable as a string. Using the @ operatior makes it clear that the parameter is a reference to a variable.

Array indexes are case sensitive: 'people["John Doe"]' and 'people["john doe"]' refer to two different variables, even though all other variables are case insensitive ('Person' and 'person' will refer to the same variable). 

Things I would like to change (when I find time):
=================================================

mu_par_num should be mu_par_int in the API.

There are some places where a call to mu_throw() could cause memory to be leaked. Moral of the story: write your Musl programs in such a way that mu_throw() need never be called.

The INPUT$() function should take an optional second parameter that is the name of the variable to read, so that you can use
	
	INPUT$ "Please enter a number", @n
	
instead of
	
	n = INPUT$("Please enter a number")
	
If an undefined variable ends with $ it should be initialised to "" rather than 0.
so `PRINT undefined$` would print nothing.

ToDo Built-In Functions:
========================

Ideas for built-in functions that may be useful go in this section.

Replace$(str$, find, repl) - Replaces occurances of 'find' with 'repl' in str

GSUB$() - Like Awk's gsub() function (instead of replace$() above)

Speaking of Awk, how about a function AWK(line$, [delim$]) that takes line$ and splits it into columns in the _C[i], so that a Musl script can do some of the same work as Awk. It returns the number of columns found in the line. _C["length"] should be the same as the arrays filled in by DATA() 

Actually, the above is just a SPLIT() function. I would actually like to have functions like ColdFusion's List functions. They are very much in the spirit of Musl.

ASC() and CHR() for converting between characters and numbers: ASC('A') is 65 and CHR(65) is 'A'

How about an Oracle-like DECODE() function (see http://www.techonthenet.com/oracle/functions/decode.php for details)? I can give mine a different name, like SWITCH or SELECT

I'd also like to have a feature to dump all the variables somewhere for debugging Musl programs. It should be an API function and a Built-in function.
