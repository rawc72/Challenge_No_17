# Hyper Regex

## Summary
This regex seeks to provide multiple examples of the different regular expresion terms. It provides snipbits of code that outline the terms in a way that is simply to understand and apply. 

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

### Anchors
Anchors have special meaning in regular expressions. They do not match any character. Instead, they match a position before or after characters:

^ – The caret anchor matches the beginning of the text.
$ – The dollar anchor matches the end of the text.

For example:

```
let str = 'JavaScript';
console.log(/^J/.test(str));
```

Output

```
true
```

The /^J/ match any text that starts with the letter J. It returns true.

The following example returns false because the string JavaScript doesn’t start with the letter S:

```
let str = 'JavaScript';
console.log(/^S/.test(str));
```

Output

```
false
```

Similarly, the following example returns true because the string JavaScript ends with the letter t:

```
let str = 'JavaScript';
console.log(/t$/.test(str));
```

Output

```
true
``

### Quantifiers
Quantifiers match a number of instances of a character, group, or character class in a string.

#### Exact count {n}

A number in curly braces {n}is the simplest quantifier. When you append it to a character or character class, it specifies how many characters or character classes you want to match.

For example, the regular expression /\d{4}/ matches a four-digit number. It is the same as /\d\d\d\d/:

```
let str = 'ECMAScript 2020';
let re = /\d{4}/;

let result = str.match(re);

console.log(result);
```

Output

```
["2020"]
```

#### The range {n,m}

The range matches a character or character class from n to m times.

For example, to find numbers that have two, three or four digits, you use the regular expression /\d{2,4}/g:

```
let str = 'The official name of ES11 is ES2020';
let re = /\d{2,4}/g;

let result = str.match(re);
console.log(result);
```

Output

```
["11", "2020"]
```

Because the upper limit is optional, the {n,} searches for a sequence of n or more times.

For example, the regular expression /\d{2,}/ will match any number that has two or more digits.

```
let str = 'The official name of ES6 is ES2015';
let re = /\d{2,}/g;

let result = str.match(re);
console.log(result);
```

Output

```
["2015"]
```

The following example uses the regular expression /\d{1,}/g to match any numbers that have one or more digits in a phone number:

```
let numbers = '+1-(408)-555-0105'.match(/\d{1,}/g);
console.log(numbers);
```

Output

```
["1", "408", "555", "0105"]
```

### OR Operator
| is the alternation operator, and it is used to specify alternatives. For example:

```
^P|[0-9]
```

matches any string that matches either ‘^P’ or ‘[0-9]’. This means it matches any string that starts with ‘P’ or contains a digit.

The alternation applies to the largest possible regexps on either side. In other words, ‘|’ has the lowest precedence of all the regular expression operators.

### Character Classes
A character class allows you to match any symbol from a certain character set. A character class is also called a character set. Suppose that you have a phone number like this:

```
+1-(408)-555-0105
```

And you want to turn it into a plain number:

```
14085550105
```

Character classes in regular expressions can help you to achieve this.

Let’s explore the digit character class first. The digit character class is denoted by \d which matches any single digit:

```
\d
```

The following example uses the \d to match the first number in the phone number:

```
let phone = '+1-(408)-555-0105';
let re = /\d/;

console.log(phone.match(re));
```

Output

```
["1"]
```

When you add the global flag (g), the regular expression will search for all numbers, not the first one:

```
let phone = '+1-(408)-555-0105';
let re = /\d/g;

console.log(phone.match(re));
```

Output

```
["1", "4", "0", "8", "5", "5", "5", "0", "1", "0", "5"]
```

Now, you can turn the phone number into a plain number as follows:

```
let phone = '+1-(408)-555-0105';
let re = /\d/g;

let numbers = phone.match(re);
let phoneNo = numbers.join('');

console.log(phoneNo);
```

Output

```
14085550105
```

To make it short, you can chain the match() and join() methods like this:

```
console.log('+1-(408)-555-0105'.match(/\d/g).join(''));

```

Besides the character class for digits (\d), regular expressions support other character classes.

The most commonly used character classes are:

\d – match a digit or a character from 0 to 9.
\s – match a whitespace symbol such a space, a tab (\t), a newline (\n), etc.
\w – w stands for word character. It matches the ASCII character [A-Za-z0-9_] including Latin alphabets, digits, and the underscore (\_)
In practice, you often combine the character classes to form a powerful match.

For example \w\d matches any word followed by a digit like O2:

```
let str = 'O2 is oxygen';
let re = /\w\d/g

console.log(str.match(re));

```

Output

```
02
```

#### Inverse Classes

A character class has an inverse class with the same letter but in the uppercase e.g., \D is the inverse of \d. The inverse class matches all the other characters. For example, the \D match any character except a digit (or \d). The following are the inverse classes:

\D – matches any character except digits e.g., a letter.
\S – matches any character except whitespace e.g., a letter
\W – matches any character except word character e.g., non-Latin letter or space.

You can remove the non-digit using the \D inverse class and replace all non-digit characters with blank, like this:

```
let phone = '+1-(408)-555-0105';
let re = /\D/g;

console.log(phone.replace(re,''));
```

The dot (.) character class
The dot (.) is a special character class that matches any character except a newline:

```
let re = /E.6/
console.log('ES6'.match(re));
```

Output

```
["ES6", index: 0, input: "ES6", groups: undefined]

```

However, the following example returns null:

```
let re = /ES.6/
console.log('ES\n6'.match(re));
```

If you want to use the dot (.) character class to match any character including the newline, you can use the s flag:

```
let re = /ES.6/s
console.log('ES\n6'.match(re));
```

Output

```
["ES
6"]
```

### Flags
| Flag | Name          | Modification                                                                                                                                       |
| ---- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| i    | Ignore Casing | Makes the expression search case-insensitively.                                                                                                    |
| g    | Global        | Makes the expression search for all occurences.                                                                                                    |
| s    | Dot All       | Makes the wild character . match newlines as well.                                                                                                 |
| m    | Multiline     | Makes the boundary characters ^ and $ match the beginning and ending of every single line instead of the beginning and ending of the whole string. |
| y    | Sticky        | Makes the expression start its searching from the index indicated in its lastIndex property.                                                       |
| u    | Unicode       | Makes the expression assume individual characters as code points, not code units, and thus match 32-bit characters as well.                        |

#### Ignore casing

The first and foremost flag we shall explore in this section is the i flag, where the 'i' stands for ignore casing.

As the name suggests, the i flag serves to make an expression look for its matches while ignoring character casing. That is, a lowercase character in the expression matches both lowercase as well as uppercase characters in the string.

By default, a regular expression searches for its first match case-sensitively. However, using the i flag, we can modify this default behaviour, and usually we do need to.

#### Global search

The second most important flag in the world of regular expression is g.

The flag g stands for global, more specifically, global searching. It serves to make an expression look for all its matches, rather than stopping at the first one.

By default, when a regex engine finds the first match for a given pattern in a given test string, it terminates and prevents any further searching. To modify this behaviour, we have at our dispense the g flag.

For example, let's say we have two expressions /cats/ and /cats/g and our string is "cats love cats".

The first expression (without the g flag) would match only the first word 'cats' ("cats love cats"). In contrast, the second expression (with the g flag) would match both the words 'cats' ("cats love cats").

#### Dot all

A fairly recent introduction to the list of flags in JavaScript's regular expressions is that of s.

The flag s means dot all. That is, it makes the . dot character (technically refered to as the wildcard character) match everything, even newlines. In other words, with the s flag, the dot matches all possible characters.

By default, the dot character in a regular expression matches everything, but newline characters. To get it to match newline characters as well, we are given the s flag.

#### Multiline mode

The flag m stands for multiline mode and serves to make the boundary tokens ^ and $ match the beginning and end of each line.

By default, the ^ and $ characters in an expression match the beginning and ending boundaries of a given test string. But with the m flag in place, they instead do this for every line in the string.

#### Sticky searching

Often times, we might want an expression to start its searching routine, within a given test string, from an index other than 0. In other words, we might want to search for matches in the string from a custom position, like 2, 3, 4 and so on.

This can be accomplished using the y flag.

The y flag stands for sticky searching. It makes an expression search from the position specified in its lastIndex property.

Without changing the lastIndex property on an expression that has the y flag set, makes the flag useless - searching would begin at the default index 0.

#### Unicode search

The u flag, which stands for unicode, makes an expression treat characters in a given test string as code points, rather than code units.

This means that with the u flag set, we can get our expressions to behave normally on characters that are outside the BMP range of the UTF-16 encoding.

The u flag is only required in special cases, where test strings contain characters outside the normal range of the UTF-16 character set. It's not a flag you'll be using very often.

### Grouping and Capturing
Groups use the ( ) symbols (like alternations, but the | symbol is not needed). They are useful for creating blocks of patterns, so you can apply repetitions or other modifiers to them as a whole. In the pattern ([a-x]{3}[0-9])+, the + metacharacter is applied to the whole group.

Also, another main use of groups is for processing parts of a match like extracting data or replacing it.

( ) Unnamed Groups
With pattern1(pattern2)pattern3, you'll capture the results of pattern2 for later use but not the parts matched by pattern1 or pattern3. This is useful when you want to extract only a portion of the search. Imagine that you are reading some text files that are formatted as forms. They could have data like this:

```
Name:"John" Surname:"Doe" Email:"john@example.com"
```

If you need to extract the value of the Name part (John in the example), you can use a pattern like Name:"([\w]+?)" to capture just the useful data, using the Name:" as a reference for locating the data within the text.

(?: ) Non capturing Groups
Use (?: ) for non capturing groups. If you need to use a group as a block but you won't process the results later, then make it non-capturing.

Named Groups
Use (?<groupname> ) to capture a group with name groupname. This is useful for later processing when input data may be presented in a different order than desired.

```
Name:"John" Surname:"Doe" Email:"john@example.com"
```

Consider the following regex pattern:

```
Name:"(?<Name>[\w]+?)".*?Surname:"(?<Surname>[\w]+?)".*?Email:"(?<Email>\b[\w.%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b)"
```

This pattern will match each piece of data and will create three Name Groups: Group 'Name' with data John, Group 'Surname' with data Doe and Group 'Email' with data john@example.com. Each language and regex engine define how to access matched groups. Check your language documentation to learn how to iterate and process matched groups.

### Bracket Expressions

A bracket expression represents a character set via a list of characters enclosed by the square brackets: '[' and ']'. It normally matches the target string with any single character from the list.

If the character list begins with '^', it matches any single character not from the rest of the list.

If two characters in the list are separated by '–', this is shorthand for the (inclusive) range of characters between those two. It is illegal for two ranges to share an endpoint, e.g. a-c-e. Ranges are collating-sequence dependent and should be avoided for portability.
Most special characters lose their special status and become literals within brackets. Additionally,
– is literal at the end or beginning of a bracket expression
^ is literal if not at the beginning of a bracket expression
The backslash character, \, retains its meaning in bracket expressions, permitting the usage of the special escape sequences: such as \n, \t, \w, \d, etc.

### Greedy and Lazy Match

#### Greedy: As Many As Possible (longest match)

By default, a quantifier tells the engine to match as many instances of its quantified token or subpattern as possible. This behavior is called greedy.

For instance, take the + quantifier. It allows the engine to match one or more of the token it quantifies: \d+ can therefore match one or more digits. But "one or more" is rather vague: in the string 123, "one or more digits" (starting from the left) could be 1, 12 or 123. Which of these does \d+ match?

Because by default quantifiers are greedy, \d+ matches as many digits as possible, i.e. 123. For the match attempt that starts at a given position, a greedy quantifier gives you the longest match.

#### Lazy: As Few As Possible (shortest match)

In contrast to the standard greedy quantifier, which eats up as many instances of the quantified token as possible, a lazy (sometimes called reluctant) quantifier tells the engine to match as few of the quantified tokens as needed. As you'll see in the table below, a regular quantifier is made lazy by appending a ? question mark to it.

Since the _ quantifier allows the engine to match zero or more characters, \w_?E tells the engine to match zero or more word characters, but as few as needed—which might be none at all—then to match an E. In the string 123EEE, starting from the very left, "zero or more word characters then an E" could be 123E, 123EE or 123EEE. Which of these does \w\*?E match?

Because the _? quantifier is lazy, \w_? matches as few characters as possible to allow the overall match attempt to succeed, i.e. 123—and the overall match is 123E. For the match attempt that starts at a given position, a lazy quantifier gives you the shortest match.

### Boundaries

#### Word Boundary: \b

The word boundary \b matches positions where one side is a word character (usually a letter, digit or underscore—but see below for variations across engines) and the other side is not a word character (for instance, it may be the beginning of the string or a space character).

The regex \bcat\b would therefore match cat in a black cat, but it wouldn't match it in catatonic, tomcat or certificate. Removing one of the boundaries, \bcat would match cat in catfish, and cat\b would match cat in tomcat, but not vice-versa. Both, of course, would match cat on its own.

Word boundaries are useful when you want to match a sequence of letters (or digits) on their own, or to ensure that they occur at the beginning or the end of a sequence of characters.

#### Not-a-word-boundary: \B

\B matches all positions where \b doesn't match. Therefore, it matches:

✽ When neither side is a word character, for instance at any position in the string $=(@-%++) (including the beginning and end of the string)
✽ When both sides are a word character, for instance between the H and the i in Hi!

### Back-references

When matching string patterns using regular expressions, you might wish to match the same piece of text more than once. When the pattern used to perform the first match includes non-literal elements, you can look for the repeated text using a backreference. A backreference in a regular expression identifies a previously matched group and looks for exactly the same text again.

#### Numbered Backreferences

A numbered backreference matches text already found in a group. You simply add a backslash character and the number of the group to match again. For example, to find the text matched by the first group in a regular expression, you would include, "\1" in your regular expression pattern. The text extracted into the captured group is then searched for in the input string at the position of the backreference.

#### Named Backreferences

One way to avoid the ambiguity of numbered backreferences is to use named backreferences. These allow you to match the text that has been captured by a named group. If the same name has been used twice or more, the backreference will match the text from the most recent match. To define a named backreference, use "\k", followed by the name of the group.

### Look-ahead and Look-behind

#### Lookahead

The syntax is: X(?=Y), it means "look for X, but match only if followed by Y". There may be any pattern instead of X and Y.

For an integer number followed by €, the regexp will be \d+(?=€):

```
let str = "1 lesson costs 15€";
console.log(str.match(/\d+(?=€)/)); // 15, the number 1 is ignored, as it is not followed by the sign €
```

The lookahead is just a test, hence the parentheses contests (?=...) are not included in the result 10.

While looking for X(?=Y), the engine of the regular expression detects X and then checks whether there is Y right after it. In case there is no Y, then the match is skipped, and the search goes on.

A pattern like X(?=Y)(?=Z) considers searching for X followed by and then Z simultaneously. It can be possible only when Y and Z are mutually exclusive.

#### Negative Lookahead¶

Now, imagine you need to get the quantity instead of the price from the same string. In our case, it’s a number \d+, not followed by €.

You can use the negative lookahead for that purpose.

The syntax of the negative lookahead is X(?!Y), considering the search for X, only if it is not followed by Y, like here:

```
let str = "2 lessons cost 30€";
console.log(str.match(/\d+(?!€)/)); // 2 (skipping the price)
```

#### Lookbehind

As it was noted above, lookahead allows adding a condition for what is ahead. Now, let’s discover loohbehind. The same logic works here. Lookbehind allows adding a condition for what is behind. In other words, it allows matching a pattern only if there is something before it.

Lookbehind can also be positive and negative. The positive lookbehind syntax is (?<=y)x< kbd>, considering that X will be matched only if there is Y before it. the syntax of the negative lookbehind is (?, considering that X will be matched, only if there is no Y before it.

Let’s check out an example:

```
let str = "1 lesson costs $15";
// the dollar sign is escaped \$
alert(str.match(/(?<=\$)\d+/)); // 15,the sole number is skipped
```

## Author
Robert Williams is an international banking executive with over 20-yr's experience in leadership, corporate finance, wealth management, retail banking and enterprise risk management. Robert is also an experienced board member with a strong corporate governance background.

His passion for learning has taken Robert to study Civil Engineering, Business Administration, Law, and now Full Stack Web Development given his belief that life is about staying open minded and humble versus the amount of accumulated knowledge that exists.

Robert's Github profile link is: https://github.com/rawc72
