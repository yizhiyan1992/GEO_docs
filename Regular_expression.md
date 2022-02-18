## Basic operations

1. [...]: a **single** character in the bracket

   reg: [abc]000

   match: b000

2. [^...]: a single character **not** present in the list

   reg: [^abc]000

   match: d000

3. range in the square bracket: match with any characters within a range

   reg: [a-zA-Z0-9]vvv

   match: bvvv, 6vvv, Gvvv

4. repeating characters (e.g. phone number)

   reg: [0-9]{11} # must be numbers with 11 digits

   reg: [0-9]{5,10} # must be numbers within the range from 5 to 10 digits

   reg: [0-9]{5,} # must be numbers higher than (or euqal to) 5 digits

   reg: [0-9]+ # must be at least one digit

   reg: [0-9]\* # zero or more number of digits

5. metacharacters

   - `\d` match any digit character (same as [0-9])
   - `\w` match any word character, digit, and underscore (a-z, A-Z, 0-9, and '\_')
   - `\s` match a whitespace character (space and tabs etc.)
   - `\t` match a tab character only

   reg: \d{3}\s\w{5}

   match: 123 ninja

6. special characters

   - `\` escape character
   - `+` one or more quantifier
   - `*` zero or more quantifier
   - `?` zero or one quantifier
   - `[]` the character set
   - `[^]` the negate symbol in a character set
   - `.` any character whatsoever (except newline)

7. start and ending patterns

   - `^` start of a newline or a string (the space does not count)
   - `$` end of a newline or a string (the space does not count)

   reg: ^apple$

   match: apple

   does not match: apple nancy # the space does not count!

8. alternate characters (|)

   - `|` or

   reg: a|pyre # match "a" or "pyre"

   - `()` grouping

   reg: (a|p)yre # match "ayre" or "pyre"

## Examples

1. telephone number (11 digits)

   `^\d{11}$`

2. username: Username must be alphanumetic and contain 5-12 characters

   `^[a-zA-z\d]{5,12}$`

3. password: password must alphanumeric (@, \_ are - are also allowed) and be 8-20 characters.

   `^[\w@-]{8,20}$`
