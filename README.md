Download Link: https://assignmentchef.com/product/solved-cse-340-project-2
<br>
<h1>2         Introduction</h1>

In this project, you will write a C or C++ program that reads a description of a context free grammar, then, depending on the command line argument passed to the program, performs one of the following tasks:

<ol>

 <li>print the list of terminal followed by the non-terminals in the order in which they appear in the grammar rules,</li>

 <li>find <em><sub>useless </sub></em>symbols in the grammar and remove rules with useless symbols,</li>

 <li>calculate <sub>FIRST </sub>sets, 4. calculate <sub>FOLLOW </sub>sets , or</li>

 <li>determine if the grammar has a predictive parser.</li>

</ol>

<h1>3         Input Format</h1>

The following context-free grammar specifies the input format:

<table width="763">

 <tbody>

  <tr>

   <td width="201">Rule-list</td>

   <td width="28"><em>→</em></td>

   <td width="194">Rule Rule-list       |    Rule</td>

   <td width="340"></td>

  </tr>

  <tr>

   <td width="201">Id-list</td>

   <td width="28"><em>→</em></td>

   <td width="194">ID Id-list             | <sub>ID</sub></td>

   <td width="340"></td>

  </tr>

  <tr>

   <td width="201">Rule</td>

   <td width="28"><em>→</em></td>

   <td width="194">ID ARROW Right-hand-side</td>

   <td width="340">HASH</td>

  </tr>

  <tr>

   <td width="201">Right-hand-side</td>

   <td width="28"><em>→</em></td>

   <td width="194">Id-list       |</td>

   <td width="340"></td>

  </tr>

 </tbody>

</table>

The tokens used in the above grammar description are defined by the following regular expressions:

ID                                = letter (letter + digit)*

HASH                = #

DOUBLEHASH = ##

ARROW             = -&gt;

Where <sub>digit </sub>is the digits from 0 through 9 and <sub>letter </sub>is the upper and lower case letters a through z and A through Z. Tokens are case-sensitive. Tokens are space separated and there is at least one whitespace character between any two successive tokens.

<h1>4        Semantics</h1>

Each grammar <sub>Rule </sub>starts with a non-terminal symbol (the left-hand side of the rule) followed by <sub>ARROW</sub>, then followed by a sequence of zero or more terminals and non-terminals, which represent the right-hand side of the rule. If the sequence of terminals and non-terminals in the right-hand side is empty, then the right hand side represents .

The set of non-terminals for the grammar is the set of symbols that appear to the left of an arrow. Grammar symbols that do not appear to the left of an arrow are terminal symbols. The start symbol of the grammar is the left hand side of the first rule of the grammar.

Note that the convention of using upper-case letters for non-terminals and lower-case letters for terminals that I used in class does not apply in this project.

<h2>4.1       Example</h2>

Here is an example input:

decl -&gt; idList colon ID # idList -&gt; ID idList1 # idList1 -&gt; #

idList1 -&gt; COMMA ID idList1 # ##

The list of non-terminal symbols in the order in which they appear in the grammar is:

Non-Terminals = <em>{ </em>decl<em><sub>,</sub> </em>idList<em><sub>,</sub> </em>idList1 <em>}</em>

The list of terminal symbols in the order in which they appear in the grammar is:

Terminals = <em>{ </em>colon<em><sub>,</sub> </em>ID<em><sub>,</sub> </em>COMMA <em>}</em>

The list of grammar rules in the order in which they appear is:

<table width="763">

 <tbody>

  <tr>

   <td width="763">decl <em><sub>→ </sub></em>idList colon IDidList <em><sub>→ </sub></em>ID idList1idList1 <em>→ </em>idList1 <em><sub>→ </sub></em>COMMA ID idList1</td>

  </tr>

 </tbody>

</table>

Note that even though the example shows that each rule is on a line by itself, a rule can be split into multiple lines, or even multiple rules can be on the same line, according to the formal specification. The following input describes the same grammar as the above example:

decl -&gt; idList colon ID # idList -&gt; ID idList1 # idList1 -&gt; # idList1

-&gt; COMMA ID idList1 #                    ##

<h1>5          Output Specifications</h1>

Your program should read the input grammar from standard input, and read the requested task number from the first command line argument (we provide code to read the task number). Then, your program should calculate the requested output based on the task number and print the results in the specified format for each task to standard output (stdout). The following specifies the exact requirements for each task number.

<h2>5.1       Task 1</h2>

Task one simply outputs the list of terminals followed by the list of non-terminals in the order in which they appear in the grammar rules.

<strong>Example:           </strong>For the input grammar

decl -&gt; idList colon ID # idList -&gt; ID idList1 # idList1 -&gt; # idList1 -&gt; COMMA ID idList1 # ##

the expected output for task 1 is:

colon ID COMMA decl idList idList1

<strong>Example:           </strong>Given the input grammar:

decl -&gt; idList colon ID # idList1 -&gt; # idList1 -&gt; COMMA ID idList1 # idList -&gt; ID idList1 # ##

the expected output for task 1 is:

colon ID COMMA decl idList idList1

Note that in this example, even though the rule for <sub>idList1 </sub>is before the rule for <sub>idList</sub>, <sub>idList </sub>appears before <sub>idList1 </sub>in the grammar rules.

<h2>5.2          Task 2: Find useless symbols</h2>

Determine <em><sub>useless </sub></em>symbols in the grammar and remove rules that contain useless symbols. Then output each of the remaining rules on a single line in the following format:

&lt;LHS&gt; -&gt; &lt;RHS&gt;

Where <sub>&lt;LHS&gt; </sub>should be replaced by the left-hand side of the grammar rule and <sub>&lt;RHS&gt; </sub>should be replaced by the right-hand side of the grammar rule. If the grammar rule is of form <em><sub>A → </sub></em>, use <sub># </sub>to represent the epsilon. Note that this is different from the input format.

The grammar rules should be printed in the same relative order that they originally had. So, if Rule1 and Rule2 are not removed after the elimination of useless symbols, and Rule1 appears before Rule2 in the original grammar, then Rule1 should be printed before Rule2 in the output.

<strong>Definition: </strong>Symbol <em><sub>A </sub></em>is <em><sub>not </sub></em>useless if there is a derivation starting from <em><sub>S </sub></em>of a string of <em><sub>w </sub></em>of terminals, possibly empty (<em><sub>w </sub>∈ </em><em><sub>T</sub><sup>∗</sup></em>), in which <em><sub>A </sub></em>appears:

<em>∗               ∗ S ⇒ … ⇒ xAy ⇒ … ⇒ w</em>

<strong>Example 1:             </strong>Given the following input grammar :

decl -&gt; idList colon ID # idList -&gt; ID idList1 # idList1 -&gt; # idList1 -&gt; COMMA ID idList1 # ##

the expected output for task 2 is:

decl -&gt; idList colon ID idList -&gt; ID idList1 idList1 -&gt; #

idList1 -&gt; COMMA ID idList1

Note that none of the symbols of this grammar are useless.

<strong>Example 2:            </strong>Given the following input grammar:

S -&gt; A B #

S -&gt; C #

C -&gt; c #

S -&gt; a #

<ul>

 <li>-&gt; a A #</li>

 <li>-&gt; b #</li>

</ul>

##

the expected output for task 2 is:

S -&gt; C

C -&gt; c

S -&gt; a

Note that A and B are useless symbols and the modified grammar has only three rules. Also note that the relative order of the rules is preserved.

<h2>5.3           Task 3: Calculate FIRST Sets</h2>

Compute the <sub>FIRST </sub>sets of all non-terminals. Then, for each of the non-terminals of the input grammar, in the order that it appears in the grammar, output one line in the following format:

FIRST(&lt;symbol&gt;) = { &lt;set_items&gt; }

Where <sub>&lt;symbol&gt; </sub>should be replaced by the non-terminal and <sub>&lt;set_items&gt; </sub>should be replaced by a comma-separated list of

elements of the set ordered in the following manner:

<ul>

 <li>If belongs to the set, represent it as <sub>#</sub></li>

 <li>If belongs to the set, it should be listed before any other elements</li>

 <li>All other elements of the set should be sorted in the order in which they appear in the grammar <strong>Example: </strong>Given the input grammar:</li>

</ul>

decl -&gt; idList colon ID # idList -&gt; ID idList1 # idList1 -&gt; # idList1 -&gt; COMMA ID idList1 # ##

the expected output for task 3 is:

FIRST(decl) = { ID }

FIRST(idList) = { ID }

FIRST(idList1) = { #, COMMA }

<h2>5.4         Task 4: Calculate FOLLOW Sets</h2>

For each of the non-terminals of the input grammar, in the order that they appear in the non-terminals section of the input, compute the <sub>FOLLOW </sub>set for that non-terminal and output one line in the following format:

FOLLOW(&lt;symbol&gt;) = { &lt;set_items&gt; }

Where <sub>&lt;symbol&gt; </sub>should be replaced by the non-terminal and <sub>&lt;set_items&gt; </sub>should be replaced by the comma-separated list

of elements of the set ordered in the following manner:

<ul>

 <li>If EOF belongs in the set, represent it as <sub>$</sub></li>

 <li>If EOF belongs in the set, it should be listed before any other elements</li>

 <li>All other elements of the set should be sorted in the order in which they appear in the grammar <strong>Example: </strong>Given the input grammar:</li>

</ul>

decl -&gt; idList colon ID # idList -&gt; ID idList1 # idList1 -&gt; # idList1 -&gt; COMMA ID idList1 #

##

the expected output for task 4 is:

FOLLOW(decl) = { $ }

FOLLOW(idList) = { colon }

FOLLOW(idList1) = { colon }

<h2>5.5               Task 5: Determine if the grammar has a predictive parser</h2>

Determine if the grammar has a predictive parser and output either <sub>YES </sub>or <sub>NO </sub>accordingly. If the grammar has useless symbols, your program should output <sub>NO</sub>.

<strong>Example:           </strong>Given the grammar:

decl -&gt; idList colon ID # idList -&gt; ID idList1 # idList1 -&gt; # idList1 -&gt; COMMA ID idList1 # ##

the expected output of Task 5 is:

YES

<h1>6         Implementation</h1>

<strong>6.1      Lexer</strong>

A lexer that can recognize <sub>ID</sub>, <sub>ARROW</sub>, <sub>HASH </sub>and <sub>DOUBLEHASH </sub>tokens is provided for this project. You are free to use it if you like.

<h2>6.2          Reading command-line argument</h2>

As mentioned in the introduction, your program must read the grammar from stdin and the task number from command line arguments. The following piece of code shows how to read the first command line argument and perform a task based on the value of that argument. Use this code as a starting point for your main function.

/* NOTE: You should get the full version of this code as part of the project material, do not copy/paste from this document. */

#include &lt;stdio.h&gt; #include &lt;stdlib.h&gt;

int main (int argc, char* argv[])

{ int task;

if (argc &lt; 2) {

printf(“Error: missing argument
”); return 1; }

task = atoi(argv[1]);

switch (task) { case 1:

// TODO: perform task 1. break;

// …

default: printf(“Error: unrecognized task number %d
”, task); break;

} return 0;

}

<h1>7        Evaluation</h1>

Your submission will be graded on passing the automated test cases. The test cases (there will be multiple test cases in each category, each with equal weight) will be broken down in the following way (out of 100 points):

<ul>

 <li>Task 1: 10 points • Task 2: 30 points • Task 3: 30 points</li>

 <li>Task 4: 25 points</li>

 <li>Task 5: 5 points</li>

</ul>

Submit your code on the course submission website: <a href="http://cse340.fulton.asu.edu/cse340/">http://cse340.fulton.asu.edu/cse340/</a>


