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

9. greedy and lazy match

   - greedy search ".+" : In the greedy mode (by default) a quantified character is repeated as many times as possible.

     a "witcth" and her "broom" is one.

     =====> "witch" and her "broom"

   - lazy search ".+?" : It means repeating minimal number of times. (usually a question mark `?` is a quantifier by itself (zero or one), but if added after another quantifier (or even itself) it gets another meaning - it switiches to matching mode from greedy to lazy.)

10. flags

    - /abc/i : i denotes case insensitive
    - / /g : global: does not return after the first match
    - / /m : multi-line

11. back reference (advanced)

## Examples

1. telephone number (11 digits)

   `^\d{11}$`

2. username: Username must be alphanumetic and contain 5-12 characters

   `^[a-zA-z\d]{5,12}$`

3. password: password must alphanumeric (@, \_ are - are also allowed) and be 8-20 characters.

   `^[\w@-]{8,20}$`

4. match emails

   pattern: (yourname) @ (domain) . (extension)(.again)

   exp: `^([a-zA-Z0-9\.-]+)@([a-zA-Z0-9-]+)\.([a-z]{2,8})(\.[a-z]{2,8})?$`

5. A string start with "The"

   exp : `^The\s` (Note if using `^The` then "These" will be matched as well.)

6. The quantifiers...

   - `abc*` : matches: ab, abc, abcc
   - `abc+` : matches: abc, abcc
   - `abc?` : matches: ab, abc
   - `abc{2,5}` : matches: abcc, abcccc
   - `a(bc){2,5}` : matches: abcbc, abcbcbc

7. OR operator - `a(b|c)` : ab or ac - `a[bc] : same as above

## Application fields of Regex

- data validation (check if a stime string is well-formed)
- data scraping (find all pages that contain a certain set of words)
- data wrangling (transform data from raw to another format)
- string parsing (catch all URL GET parameters)
- string replacement
- syntax highlighting, file renaming

## Documents

1. validation of pattern matching: https://regex101.com/
2. a tutorial video: https://www.youtube.com/watch?v=Eu1KRvw4tKg&list=PL4cUxeGkcC9g6m_6Sld9Q4jzqdqHd2HiD&index=16
