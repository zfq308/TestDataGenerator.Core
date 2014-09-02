
#TestDataGenerator
A library and command line tool that can be used to generate data for testing or other uses.  You provide it with a pattern containing symbols defining the output you want to produce and it will create random data to match that pattern.

## Quick Start with examples

### Placeholders
When using the commandline tool place all patterns and symbols inside '&lt;&lt; &gt;&gt;' tokens e.g. 'Generate some Letters &lt;&lt;\L\L&gt;&gt;'. 

### Symbols
The pattern is as follows:
- `\.` - A single random upper-case, lower-case letter or number.
- `\W` - A single random upper-case or lower-case letter.
- `\w` - A single random upper-case or lower-case letter.
- `\L` - A single random upper-case Letter.
- `\l` - A single random lower-case letter.
- `\V` - A single random upper-case Vowel.
- `\v` - A single random lower-case vowel.
- `\C` - A single random upper-case Consonant.
- `\c` - A single random lower-case consonant.
- `\D` - A single random non number character.
- `\d` - A single random number, 1-9.
- `\s` - A whitespace character (Tab, New Line, Space, Carriage Return or Form Feed)
- `\n` - A newline character.
- `\t` - A tab character.

### Groups
- `(\d){5}` - Five digits between 0 and 9.
- `\L(\d){3}\L` - A upper-case letter, five digits between 0 and 9 and another upper-case letter.
- `(.[100-101]){3}` - Three items, each will include a dot '.' and either 100 or 101 e.g. *'.100.101.101'*

### Ranges
- `[a-z]` - A single lower-case letter between a and z.
- `[a-z]{5}` - Five lower-case letter between a and z.
- `[a-z]{5,10}` - Between five and ten lower-case letter between a and z.
- `[A-Z]` - A single upper-case letter between Z and Z.
- `[1-5]` - A number between 1 and 5.
- `[100-500]` - A number between 100 and 500.
- `[1-28]/[1-12]/[1960-2013]` - A date value between 1960 and 2013.
- `[1.00-5.00]` - A decimal number between 1.00 and 5.00.

### Alternations
**Alternatives must be contained within a Group**
- `(\L\L|\d\d)` - Either two upper-case letters OR two numbers.
- `(\L\L|\d\d|[AEIOU]|[100-120])` - Either two upper-case letters OR two digits OR an upper-case vowel OR a number between 100 and 120.

### Named Parameters
A named pattern is surrounded with @ characters and links to a predefined pattern loaded from a file. The `default.tdg-patterns` file located in the same directory as the tdg executable file contains a list of named patterns which can be used in other patterns you write.  For example to generate you could write something like `([1-9]\d\d-\d\d-\d\d\d\d)` or you can use the named parameter in the file `(@ssn@)` to a similar value.  You can add more patterns to the file as you wish.  Named patterns can also include other named patterns if you so wish.  Take a look at the `@us_address_type1@` pattern in the file as an example of a compound pattern than uses other patterns to produce an output.
'443 Meadow Lane, Marin County, New York, OV"Wo'

### CommandLine tool
You can use the `tdg.exe` application to generate test data from the command line.  It can handle provided templates directly from the command line or from a file. The tool also supports exporting the generated output to either the command line or another file.

### Parameters:
- `-t, --template:`    The template containing 1 or more patterns to use when producing data.
- `-p  --pattern:`     The pattern to use when producing data.
- `-i  --inputfile:`   The path of the input file.
- `-o, --output:`      The path of the output file.
- `-c, --count:`       The number of items to produce.
- `-v, --verbose:`     Verbose output including debug and performance information.
- `--help`            Display the help screen.
  
### Examples
- Single repeating symbols using the following syntax
  - `tdg -t 'Letters &lt;&lt;\L{20}&gt;&gt; and Numbers &lt;&lt;\d{12}&gt;&gt;'`
  - Produces items like *'Letters XTPXIBSQYJLJIULMVEMH and Numbers 408474979122'*.
- Repeating patterns containing multiple letters or numbers of random length.
  - `tdg -t '&lt;&lt;(\L){5}&gt;&gt;'` - Will generate 5 random upper-case characters. e.g. *'BGLOE'*
  - `tdg -t '&lt;&lt;(\L\L\d){24}&gt;&gt;'`  - Will generate 24 repeating letter-letter-number values e.g. *'ZF6ET9BI8HA2DQ3EH7XY1DW1XS7DF3BI8HW9WD6BT1WE9OT5VB7ON6BW3NG0FX1PF6IW7KI1'*
- Variable length data can be generated also
  - `tdg -t '&lt;&lt;(\L){10,20}&gt;&gt;'` - Will generate a string containing between 10 and 20 characters of random value e.g. *'WEZHSDJYFH'*
  - `tdg -t 'Letters &lt;&lt;\L{2,20}&gt;&gt; and Numbers &lt;&lt;\d{2,12}&gt;&gt;'` produces items like *'Letters NNVH and Numbers 0638'*
- Input can contain several placeholders.
  - `tdg -t 'Hi there &lt;&lt;\L\v{0,2}\l{0,2}\v \L\v{0,2}\l{0,2}\v{0,2}\l{0,2}\l&gt;&gt; how are you doing?  Your SSN is &lt;&lt;[1-9]\d\d-\d\d-\d\d\d\d&gt;&gt;.' -c 100` 
  - Produces 100 items like *'Hi there Quau Huloilc how are you doing?  Your SSN is 639-00-6906.'*
- Generate 100 SSN like values and output to console window.
  - `tdg -t '&lt;&lt;[1-9]\d\d-\d\d-\d\d\d\d&gt;&gt;' -c 100`
  - Produces 100 items like *']06-77-9230'*.
- Generate 100 strings with random name like values and output to file.
  - `tdg -t 'Hi there &lt;&lt;\L\v\l\v \L\v\l\l\v\v\l\l\v&gt;&gt; how are you doing?' -c 100 -o C:\test1.txt`
  - Produces 100 items like *'Iuzo Waakueyaa'*.
- `tdg -t '&lt;&lt;Letters \w{2,20} and Numbers \d{2,12}\n&gt;&gt;'` produces the following output: *'Letters hwaWXDPlaloxiqaHOp and Numbers 601
'*

## More Information

### Placeholders
You can place your symbols and patterns within placeholders which will be replaced with the generated values.  These placeholders contain 1 or more symbols representing the desired output characters.  Note that placeholders are wrapped in double angle brackets `&lt;&lt;PATTERN&gt;&gt;` without the backslash.
*Please note I am supplying a backslash at the start of all placeholder examples so that they appear as examples correctly.  You do not need to add these.*

### Pattern Composition
If you are familiar with Regular Expressions then most of the syntax used will be familiar but there are significant differences in place given that regex is used to match a string against a pattern.  The generator instead uses simple patterns opf symbols to produce strings, because of the difference in usage the syntaxes cannot match up entirely.  Patterns define what the generated values will be and can be composed using text and symbols.  Sections of the pattern can be repeated a specific number of times (they can also be repeated a random number of times by providing a min and max).  Patterns can also include alternate items that will be randomly selected, helping to produce relatively complicated outputs. 

`&lt;&lt;\L\v\l\v&gt;&gt;` is a placeholder containing the pattern of symbols `\L\v\l\v`.

### Symbol Repetition
Individual symbols can be repeated by a supplying a repeat section immediately after the symbol.  For example `\L{5}` will produce 5 upper case letters.  You can also add some randomness to the mix by supplying a range: `\L{min,max}`.  The pattern `\L{1,100}` will produce between 1 and 100 upper case letters. Here's one *'VNUBPTQDQRFMITBIVGDYUMKYSKFPQXNUAWEKENFMKCQUZVRXSZHJZFUJDV'*

### Symbol Grouping
Individual symbols can be grouped together using parenthesis characters.  When grouped together they can be repeated using the same repeat syntax.  `(\l\d){5}` will produce something like *'i1o0n7d4l4'*.
You can also include the random range syntax from above.

### Alternating Symbols and Groups
Patterns can contain several individual symbols or groups of symbols and randomly alternate between them when generating the output value.  `&lt;&lt;\C|\c{10}|\V\V\V|(\v\v){2,3}&gt;&gt;` will produce either a single upper-case consonant, 10 lower-case consonants, 3 upper-case vowels or between 10 and 15 lower-case vowels.  Which one gets outputed is randomly selected when processing the pattern.

### Other patterns:
- `'&lt;&lt;This is a \L\L string&gt;&gt;'` will produce something similar to *'This is a PR string'*.
- `'&lt;&lt;This is a \d{19} string&gt;&gt;'` will produce something similar to *'This is a 1206568698738661880 string'*.
- Individual symbols can be repeated a specific number of times using the syntax `\L{10}` which will generate 10 upper case letters.
- Individual symbols can be repeated a random number of times using the syntax `\L{10,20}` which will generate between 10 and 20 upper case letters.
- 1 or more Symbols can be combined into patterns by wrapping them in parenthesis e.g. `(\*\L\d)`.
- Patterns can be repeated a specific number of times using the syntax `(\L\d){10}` which will generate 10 repeated letter-number pairs e.g. *'V2J2A9J5X6O3E2B8A3I6'*.
- Patterns can be repeated a random number of times using the syntax `(\L\d){10,20}` which will generate between 10 and 20 repeated letter-number pairs e.g. *'N5V4L2D8D9Q6K7K8C0S1B9'*.

### Profiling results
*These timings are taken from unit tests making direct API calls, the command line tool will have higher times as it has additional IO work to output the values to screen or file.  Should still be fast.*
- 1000 instances of the following template generated in 172 milliseconds.
  - `&lt;&lt;\L{1}\d{1}\L{2}\d{2}\L{4}\d{4}\L{8}\d{8}\L{16}\d{16}\L{32}\d{32}\L{64}\d{64}\L{128}\d{128}\L{256}\d{256}\L{512}\d{512}\L{1024}\d{1024}&gt;&gt;`
- 1000 instances of the following template generated in 3 milliseconds.
  - `&lt;&lt;\L{50}&gt;&gt;`
- 1000 instances of the following template generated in 3 milliseconds.
  - `&lt;&lt;\L{50,51}&gt;&gt;`

## This README was generated using the generator.  See the unit tests for other examples.
