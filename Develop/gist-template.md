# Title (replace with your title)

Introductory paragraph (replace this with your text)

## Summary

Regex stands for regular expresions. They are objects with a list of characters that describes a pattern
to try and match strings in data. Some good uses of this would to be find phone numbers or emails. This allows developers to use one regex instead of using a array of all possible options which could be infinite.

example: 
```
/([A-Z])\w+/g
```

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components
---

### **Anchors**:

Anchors are unique in regex. Instead of matching a special character they are used to match the position before or after specific characters

examples of anchores: 

`^` - Carrot anchor is used to match the beggining of the text

`$` - the dollar anchor is used to match the end of the text

examples of use: 

```javascript 
var regex = '/^J/'
var goodString = 'Javascript'; //when checking this would return true
var badString = 'Hello World'; //when checking this would return false
```

```javascript 
var regex = '/d$/'
var badString = 'Javascript';  //when checking this would return false
var goodString = 'Hello World';  //when checking this would return ture
```

Anchors also work for checking th full line by adding multiline flag. *Note: we will go into more depth on flags later on*. The multi line `m` at the end of a regex checks all lines and the anchor is applied accordingly. 

example: 
```javascript
var regex = '/^J/m';

//when checking this would return Javascript
var startAnchor = `
Javascript 
is the 
best`;

//when checking this would return World
var endingAnchor = `
Hello 
World`;
```
---

### **Quantifiers:**

Quantifiers what the name inplies and are used to match based of quantity. This can be used on single characters, groups of characters, or characters classes in a string.

Exact Count: `{n}`

Exact count can be done using the described notation but with replacing in with the number of instances you looking to match.

example:

```javascript
var regex = '/\d{4}/g';

var string = '1920, 1921, 1922, 1923, 1924, 1925'
//result = 1920, 1921, 1922, 1923, 1924, 1925

/* 
output would only grab the numbers and ignore the commas and spaces.
the /d stands for all digits or numbers and {4} is when those numbers 
appear 4 times. 
*/
```
Range: `{n,m}`

Range is similar to to exact count but gives a us a range of instances to match with. 
you would simply use the notation above. You can also exclude a number in `m`
spot to do a greater than. Notation `{1,}` would match with anything one or greater.

exapmle:

```javascript
var regex = '/\d{1,4}/g';

var string = '1920, 1921, 1922, 1923, 1924, 25'
//result = 1920, 1921, 1922, 1923, 1924, 25

/* 
output would only grab the numbers and ignore the commas and spaces.
the /d stands for all digits or numbers and {1,4} is when those numbers 
appear between 1 to 4 times. So it would still grab the years but would 
also grab the 25 at the end.
*/
```

```javascript
var regex = '/\d{3,}/g';

var string = '1920, 1921, 1922, 1923, 1924, 25'
//result = 1920, 1921, 1922, 1923, 1924

/* 
when applied would only select the full years since they are appear 3 or more. 
*/
```

#### Short Hand: 

Greater than: `+`

We saw with the range that we could do greater than by excluding `m` from our expression, but there is a short hand that we can utilize. The `+` is short hand for `{1,}` or greater that one. 

```javascript
var regex = '/\d{1,}/g';
var regex = '/\d+/g';

var string = '1920, 1921, 1922, 1923, 1924, 25'
//result = 1920, 1921, 1922, 1923, 1924, 25

/* 
Both of the expressions are doing the same thing and when applied to the string 
would find each date.
*/
```
Zero or one: `?`

The question mark indecates when searching for a instance of zero or one. 

```javascript
var regex = '/\d{0,1}/g';
var regex = '/\d?/g';

var string = '1920, 1921, 1922, 1923, 1924, 25'
//result = 1, 9, 2, 0, 1, 9, 2, 1, 1, 9, 2, 2, 1, 9, 2, 3, 1, 9, 2, 4, 2, 5

/* 
Both of the expressions are doing the same thing and would match with each 
individual numbers instead of the full year.
*/
```

Zero or more: `*`

The star indecates when searching for anything zero or greater

```javascript
var regex = '/\d{0,}/g';
var regex = '/\d*/g';

var string = '1920, 1921, 1922, 1923, 1924, 25'
//result = 1920, 1921, 1922, 1923, 1924, 25

/* 
Both of the expressions are doing the same thing and would match all the
dates. 
*/
```

---

### **OR Operator:**

The or operator or also known as alternation is represented as `|`. This will match with all cases on either side of the alternation. 

example: 

```javascript
var regex = '/Th(is|at)/g';

var string = 'This or That'
//result = This, That

/* 
this would match with both 'this' or 'that'
*/
```

### **Character Classes:**

Character classes are ways to select a specific type of charcter. We have seen one example of this in another example prior. There are four character classes of which inclde: 

`\d` - All numbers

`\w` - All Letter characters uppercase and lower, Number characters and undersocre

`\s` - All whitespace including tab and new line

```javascript
var numbers = '/\d/g';
var spaces = '/\s/g';
var words = '/\w/g';

var string = 'The quick brown fox jumps over the lazy dog was first seen on February 9, 1885'

/* 
When the numbers regex is applied output would match: 9 and 1885
When the spaces regex is applied only the spaces between each word is selected
and when the words regex is applied everything but the comma and white spaces are
selected. 
*/
```

Each of these regex has inverse options as well. They include:

`\D` - no numbers

`\W` - no Letter characters uppercase and lower, Number characters and undersocre

`\S` - no whitespace including tab and new line

If you simply capitalize the letter it will find the inverse of the selected character class

```javascript
var numbers = '/\D/g';
var spaces = '/\S/g';
var words = '/\W/g';

var string = 'The quick brown fox jumps over the lazy dog was first seen on February 9, 1885'


/* 
When the numbers regex is applied output would match everthing but 9 and 1885
When the spaces regex is applied everyhing but the spaces is selected
and when the words regex is applied only the spaces and comma are selected. 
*/
```

Last character class is the dot `.`

The dot selects everything except a new line.
Example: 

```javascript
var numbers = '/./g';

var string = `The quick brown fox jumps over the lazy dog was first seen on February 9, 1885

This is a new line but there is a empty line above this.`

/* 
This wiil select all characters but will not capture the empty new line in the middle.
*/
```

---
### **Flags:**

Flags are optional paramenters to add to your regex. We have already seen one in the examples above. Flags are added to the end of the expression on the outside of the forward slashes. Flags include: 

`g` - Global: this flag allows us to search the full length of the item. By default regex will only search the first index to prevent from infinte searching.

```javascript
var regex = '/\w/g';

var string = 'The quick brown fox jumps over the lazy dog was first seen on February 9, 1885'

/* 
This will match all letters in the string. if the "g" is removed only the "T" at the beggining would
selected.
*/
```

`i` - Ignore case: This does what the name describes. It ignores case when searching. 

```javascript
var regex = '/t/gi';

var string = 'The quick brown fox jumps over the lazy dog was first seen on February 9, 1885'

/* 
adding the 'i' at the end would not only select the lowercase t's but also the uppercase T's
*/
```

`m` - Multiline: This modifies the "^" and "$" to include multiple lines

```javascript
var regex = '/^\w+/gm';

var string = `The quick brown fox jumps over the lazy dog was first seen on February 9, 1885
This is a new line
This is also a new line`

/* 
adding the m to this would select the first word on each line. 
*/
```

`u` - Unicode: this will also allow you to search for unicode characters. Examples of unicode characters include items like emojis or unique characters. You also want to include `\u{}` with the unicode in the curly brackets.  

```javascript
var regex = '/^\u{0168}/gu';

var string = `Ũ`
//result = Ũ

/* 
Since the character in our string is outside the normal range of characters we would want 
to includde the "u" and select it with the "\u{10345}" which is it's corresponding unicode.
*/
```

`y` - Sticky: this alows us to set up a lastIndex to search by starting at that position.

```javascript
var regex = /cats/igy;
//Note: if we remove the single quotes it creates regex as an regex object
regex.lastIndex = 3;

var string = `Cats love cats, and we love cats.`
//result = cats, cats

/* 
This will start its search at index of three and result in the "Cats" at the beggining to be ignored
and leaving the other two iterations of cats to be selected.
*/
```

`s` - Single Line: This flag makes the "." character match with everything including new lines.

```javascript
var numbers = '/./gs';

var string = `The quick brown fox jumps over the lazy dog was first seen on February 9, 1885

This is a new line but there is a empty line above this.`
/* 
Now that we have added the "s" flag the new line will also be selected.
*/
```

---
### **Grouping and Capturing:**

With regular expressions we can use grouping to select specific searches and to prevent other selectors from interacting with that search. Example: 

```javascript
var numbers = /(\w+)[^.png]/gi;

var string = `testfile.png`
//result = testfile

/* 
This regex would be used to select the file name of .png files. 
*/
```
Groups can also be named to reference it later.

```javascript
var numbers = /(?<name>\w+)[^.png]/gi;

var string = `testfile.png`

/* 
If neccsary you can reference name to get that specific group. This will sill only select 
the file name.
*/
```

Groups can also be combined together. 

```javascript
var numbers = /(?:ha)+/gi;

var string = `hahaha haa hah!`
//result = hahaha, ha, ha

/* 
hahaha will combined together into one match because they match the group multiple times
*/
```
---


### **Bracket Expressions:**

Bracket expressions are indicated by brackets `[]`. They can be used to describe a range of characters such as letters `[a-z]` or numbers with `[0-9]`. Brackets also allow us to negate chracters buy using the carrot `^` like this: `[^atdc]`. This will exclude items in the brackets.

Regular use:
```javascript
var numbers = /[a-z]/g;

var string = `The quick brown fox jumps over the lazy dog was first seen on February 9, 1885`

/* 
This will select all lowercase letters.
*/
```
Negate use
```javascript
var numbers = /[^a-z]/g;

var string = `The quick brown fox jumps over the lazy dog was first seen on February 9, 1885`
/* 
This will select everything but lowercase letters a
*/
```
---


### **Greedy and Lazy Match:**

Greedy is the default way of searching in regex. This is where the expression is going to try and match the exact match. Lazy takes a different approach to finding a match. Lazy's main goal is to repeat minamal amount of times, and in the expression is notated by a question mark after the quantifier.

Example: `+?`, `*?` and could even be `??`

with lazy matching when it finds a match instead of going through the entire string will check the current match with the rest of the expression.

Example: 

```javascript
var numbers = /".+?"/g;

var string = `The quick brown fox jumps "over" the lazy dog was first seen on "February" 9, 1885`
//result = "over", "February"


/* 
This will grab all words with qutation marks which in the case is over and February. The reaseon 
for this is beacuse it will first element grabs the first qutation mark. Then it finds all 
characters in the 
*/
```


### **Boundaries:**

Boundries allows us to grab either the beggining or end of a "word character" and a "non-word character". What determines a word character is dependent on the version of regex you may be using. You can tell by what ever the `\w` catches in a match. 

```javascript
var numbers = /s\b"/g;

var string = `she sells seashells`
//result = s, s


/* 
this will capture all the s's at the end of each word. 
*/
```

There is also a inverse option with `\B`

```javascript
var numbers = /s\B"/g;

var string = `she sells seashells`
//result = s, s, s, s


/* 
this will caputre all the s's that are not at the end which includes the s in shells
*/
```

---
### **Back-references:**

Back references allows us to reuse grouped expressions. By adding a back-slash and the group that we want to reuse; regex will then apply that to the search.

In the example we will check the first letter in the check and then check to see if the next letter is an a. Once it hits `\1` it goes back to that group and checks to see if that current check is that same as the reference group. Thats why whe see the result `hah, dad, gag`, because the first letter matchs the last.

```javascript
var numbers = /(\w)a\1/gi;

var string = `hah dad bad dab gag gab`
//result = hah, dad, gag

/* 
This will grab the whole word because the the \1 references group one
or the (\w)
*/
```

---
### **Look-ahead and Look-behind:**

These expressions allow us to grab charecters either before or after certain groups or individual characters

Positive Lookahead:
```javascript
var numbers = /\d(?=px)/gi;

var string = `1pt 2px 3em 4px`
//result = 2, 4 

/* 
the ?= lets us know its a positive lookahead. This will grab
2 and 4 beacuse the come before the px
*/
```

Negative Lookahead:
```javascript
var numbers = /\d(?!px)/gi;

var string = `1pt 2px 3em 4px`
//result = 1, 3

/* 
the ?! lets us know its a Negative Lookahead. This will select the 1 and 3 because they
are not folowed by px
*/
```

Positive Lookbehind:
```javascript
var numbers = /\d(?<=px)/g;

var string = `pd1 px2 em3 px4`
//result = 2, 4 

/* 
the ?<= lets us know its a positive Lookbehind. This will grab
2 and 4 beacuse the come after the px
*/
```

Negative Lookbehind:
```javascript
var numbers = /\d(?<!px)/gi;

var string = `pd1 px2 em3 px4`
//result = 1, 3

/* 
the ?<! lets us know its a Negative loobehind. This will select the 1 and 3 becuase
they do not come after px.
*/
```

---
## Author

My name is Cayman Gill aka MaziveVelocity. Working towards becoming a full stack web developer through UC Berkly's Full Stack Web Development bootcamp. This is a tutorial describing what's been learned on regex 
and different parts of its workings. To see my projects and work please check out my GitHub where everything is posted. 

GitHub - https://github.com/MaziveVelocity/
