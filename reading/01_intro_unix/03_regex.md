# Regular Expressions

Regular expressions, or RegEx, are string text that are descriptive of a specific pattern.

```

.		single character

*		the preceding character matches 0 or more times

+		the preceding character matches 1 or more times

?		the preceding character matches 0 or 1 times only

{n}		the preceding character matches n number of times

{n, m}	the preceding character matches at least n times, and up to m number of times

^		matches the beginning of the line

$		matches the end of the line

()		concatenate several regex

|		OR operator

[abc]	the character is one of the characters included in the square brakets

[a-d]	the character is within the range of a to d, thus matching a, b, c, d.

[^abc]	the character is not one of the characters included in the square brakets

[a-zA-Z]	it matches any letter 		equivalent: \w
[0-9]		it matches any digit		equivalent: \d

' '			space						equivalent: \s

'	'		tab							equivalent: \t

```

[Adapted from the Evomics Workshop](https://sites.google.com/view/wg2023unix/regex?authuser=0)