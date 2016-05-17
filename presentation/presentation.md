<!-- ##### WELCOME AND ABOUT ##### -->

<!-- .slide: data-background="#000" -->
## GOTO Night
<h1 class="f60" style="margin-top: -.2em">Regular Expressions</h1>

Kim Hesselholt Reenberg  
2016-05-17

<br/>

**Wifi**

<table class="wifi">
	<colgroup>
		<col style="border-right: 1px solid;">
		<col >
		<col>
	</colgroup>
  <tr>
    <td rowspan="2" class="leftcol">SSID</td>
    <td class="ssidgroup">Win7/Linux</td>
    <td class="value">easv-portal</td>
  </tr>
  <tr>
    <td class="ssidgroup">Everyone else</td>
    <td class="value">EASV</td>
  </tr>
  <tr>
    <td class="leftcol">Username</td>
    <td></td>
    <td class="value" style="font-style:bold">Guest461</td>
  </tr>
  <tr>
    <td class="leftcol">Password</td>
    <td></td>
    <td class="value" style="font-style:bold">Easvzgap</td>
  </tr>
</table>

*Win7/Linux go to <span class="f60">**`cp.easv.dk`**</span> to login.*

<br>

*Full presentation is available online at <br><span class="f60">**[`http://khreenberg.github.io/goto-regex`](http://khreenberg.github.io/goto-regex)**</span>*

---

## About me
- Software Development PBA student
- Lazy programmer
- Self-taught regular expressions disciple

---

### This talk is split into three main parts:

<ol>
  <li class="fragment">A brief introduction to formal languages</li>
  <li class="fragment">Regex features and syntax</li>
  <li class="fragment">Examples and workshop</li>
</ol>


===


<!-- ##### INTRODUCTION TO REGULAR EXPRESSIONS ##### -->
## Regular expressions
- **have many names**
  - Regex
  - Regex***p***
  - `/reg(?:(?:exp?(?:e?s)?)|(?:ular expressions?))/ig`
  - *Black magic*
- **have many usages**
  - Validation
  - Search & Replace
  - CSS selectors
  - String manipulation
  - ~~Party-trick~~
- are _*not*_ regular expressions

---

## Regex ≠ regular expressions
- "*Real*" regular expressions can only recognize *regular languages*
- A regular language is a type of *formal language*  

<div style="margin-top: 1em">
	<small>From wikipedia:</small>
	<blockquote cite="https://en.wikipedia.org/wiki/Formal_language" style="margin-top: -.5em; width: 50%">
		&hellip;a formal language is a 
		<span class="fragment highlight-current-f60" data-fragment-index="1">set of strings of symbols</span> that may be 
		<span class="fragment highlight-current-f60" data-fragment-index="2">constrained by rules that are specific to it</span>. 
	</blockquote>
</div>
<ul>
	<li class="fragment" data-fragment-index="1">formal languages are sets</li>
	<li class="fragment" data-fragment-index="2">defined by *formal grammar*</li>
</ul>
<p class="fragment"></p>


---

## Formal languages 101
- <em>The alphabet</em>, `$\Sigma$`, is the set of 
- <em>Tokens</em>, `$\sigma$`, used to form
- <em>The words</em>, `$w$`, that make up
- <em>The language</em>

---

## (Relevant) set theory 101

Symbol        | Meaning        | Example
--------------|----------------|--------
`$\emptyset$` | The empty set  | `$\emptyset=\{ \}$`
`$\in$`       | Element of     | `$a \in \{a, b, c\}$`
`$\notin$`    | Not element of | `$x \notin \{a, b, c\}$`
`$\ni$`       | Contains       | `$\{a, b, c\} \ni a$`

<br/>

- Two sets (A and B) are equal iff `$$x \in A \iff x \in B$$`
- `$\therefore \{1, 2, 3\}=\{1, 1, 3, 1, 2, 1, 1, 2, 3\}$`
- Because the number of elements is not a factor in set equality, we will often ignore or remove duplicates


---

## Set operations

Name          | Symbol     | Example                    | Effect 
:-------------|:----------:|:--------------------------:|:-------:
Union         | `$\cup$`   | `$\{1,2\} \cup \{2,3\}$`   | `$\{1,2,3\}$`
Intersection  | `$\cap$`   | `$\{1,2\} \cap \{2,3\}$`   | `$\{2\}$`

### Operations on sets of strings

Description   | Symbol    | Example                    | Effect
:-------------|:---------:|:--------------------------:|:-------:
Concatenation | `$\cdot$` | `$\{a,b\} \cdot \{c, d\}$` | `$\{ac, ad, bc, bd\}$`
Kleene Star   | `$^*$`    | `$\{a,b\}^*$`              | `$\{\lambda,a,b,aa,ab,ba,bb,aaa,\ldots\}$`


<br/>

- The symbol for *logical disjunction* (`$\vee$`) is visually similar to that of *union*
- The symbol for *logical conjunction* (`$\wedge$`) is visually similar to that of *intersection*
- Which makes sense as

$$x \in A \cup B \iff (x \in A) \vee (x \in B)$$
$$x \in A \cap B \iff (x \in A) \wedge (x \in B)$$

%SPEAK%

- logical disjunction = "or"
- logical conjunction = "and"

---

## So, with the basics in place... 
## ... let's get back to languages <!-- .element: class="fragment f60" -->

===

## Formal definition of regular languages
The collection of regular languages over `$\Sigma$` is defined recursively as follows:

- `$\emptyset$` and `$\{\lambda\}$` are regular languages
- For each `$\sigma \in \Sigma$`, the language `$\{\sigma\}$` regular
- Let `$L_1$` and `$L_2$` be regular languages over `$\Sigma$`; the languages `$L_1\cup L_2$`, `$\ L_1\cdot L_2$` and `${L_1}^*$` are also regular
- No other languages are regular

%SPEAK%

Explicitly mentioning `$\{\lambda\}$` is redundant, as `$\emptyset^*=\{\lambda\}$`


---

## Examples 

With the alphabet `$\Sigma=\{a,b,c\}$`, we could for example define the following languages
- `$L_1=\{w \mid w\ \text{is a word that ends in}\ a\}$` 
- `$L_2=\{w \mid w\ \text{is a palindrome}\}$`
- `$L_3=\{w \mid w\ \text{is a word where all $a$s are immediately followed by a $b$, unless they are preceded by a $c$, and}\ldots\}$`

&hellip;if only we had a simple and precise way of defining languages&hellip;

---

## Describing languages with regular expressions

Set <!-- .element: style="width: 50%" --> | Regular expression
:-----------------------------------------|-------------------
`${L_1}^*$`                               | `${L_1}^*$`
`$L_1 \cdot L_2$`                         | `$L_1L_2$`
`$L_1 \cup L_2$`                          | `$L_1+L_2$`


---

## Previous examples
$$
\\begin{align}
  L_1 & =\\{w \mid w\ \text{is a word that ends in}\ a\\} \\\\
  L_2 & =\\{w \mid w\ \text{is a palindrome}\\} \\\\
  L_3 & =\\{w \mid w\ \text{is a word where all $a$s are immediately followed by a $b$, unless they are preceded by a $c$}\\} \\\\
\\end{align}
$$
<!-- .element: style="float: left" -->

<br><br><br><br><br>

## With regular expressions
\\begin{align}
  L_1 & = (a+b+c)^*a \\\\
  L_2 & = \text{???} \\\\
  L_3 & = (ab+b)^∗(\lambda+(c(a+b+c)^∗))
\\end{align}
<!-- .element: style="float: left" -->


===

## Regex ≠ regular expressions (cont.)
- "*Real*" regular expressions can only recognize *regular languages*
- Regexes can recognize a much wider range of languages, including palindromes

<br>
<br>

![][Chomsky]
<!-- .element: style="float:right; padding: .1em" -->

Type   | Language               | Recognized by
-------|------------------------|----------------------
Type-0 | Recursively enumerable | Turing machine
       | Recursive              | *Total* Turing machine
Type-1 | Context-sensitive      | Linear-bounded automaton
Type-2 | Context-free           | Pushdown automaton
Type-3 | Regular                | Finite automaton
<!-- .element: style="float:left" -->

[Chomsky]: presentation/img/Chomsky-hierarchy.svg "Chomsky heirarchy; By J. Finkelstein - Own work, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=9405226"

---

<table>
  <thead>
    <tr>
      <th>Type</th>
      <th>Language</th>
      <th>Recognized by</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Type-0</td>
      <td>Recursively enumerable</td>
      <td>Turing machine</td>
    </tr>
    <tr>
      <td></td>
      <td>Recursive</td>
      <td><em>Total</em> Turing machine</td>
    </tr>
    <tr class="fragment highlight-f60" data-fragment-index="2">
      <td>Type-1</td>
      <td>Context-sensitive</td>
      <td>Linear-bounded automaton</td>
    </tr>
    <tr>
      <td>Type-2</td>
      <td>Context-free</td>
      <td>Pushdown automaton</td>
    </tr>
    <tr class="fragment highlight-f60" data-fragment-index="1">
      <td>Type-3</td>
      <td>Regular</td>
      <td>Finite automaton</td>
    </tr>
  </tbody>
</table>

<br>

1. Kleene's theorem states that regular expressions and finite automata are equal &mdash; They are simply different ways to convey the same meaning.
<!-- .element: data-fragment-index="1" class="fragment" -->

2. Regex can be simulated by a linear-bounded automaton.
<!-- .element: data-fragment-index="2" class="fragment" -->

<!-- .element: style="width: 70%; list-style-type: none" -->


%SPEAK%
I will talk about Kleene's theorem later, if there's time and interest.

===

## Formal theory done,
## let's see some regex!
<!-- .element: class="fragment f60" -->

===

## Regex examples

Regular expression                       | Regex
-----------------------------------------|-------
`$L_1 = (a+b+c)^*a$`                     | <code>$L_1 = $[abc]*a</code>
`$L_2 = \text{N/A}$`                     | <code>$L_2 = $((.)(?1)\2&#124;.?)</code>
`$L_3 = (ab+b)^*(\lambda+(c(a+b+c)^*))$` | <code>$L_3 = $(ab&#124;b)\*(c[abc]\*)?</code>

---

## *Flavou?rs*
- There are many different regex *engines*
- The engines typically use an algorithm based on either *backtracking* (BT) or a *finite-state automaton* (FSA)
- BT is often slightly faster than FSA, but ~~an "unlucky"~~ a careless regex processed by BT can take a ***very*** long time
- Which algorithm is used can affect the "default" behaviour of the regex  
  For example: FSA-based engines are typically biased towards the longest possible match, 
  whereas those based on BT are likely to be biased towards the first —or leftmost— possible match.
- The term *flavor* (or *flavour*) refers to the slight —and sometimes not so slight— differences in semantics between different engines

This might not make a lot of sense right now, but it is something to keep in mind.

<br>

For more information on the differences between regex engines, see  
https://en.wikipedia.org/wiki/Comparison_of_regular_expression_engines

%SPEAK%
On the careless regex:  
- ***very*** long time: Anything from "a few seconds" to "completely unresponsive"
- I will talk about how not to be careless later

A quick Google search did not reveal whether or not FSA-based algorithms can typically recognize the same languages as those based on BT

---

## Regex anatomy

<!-- ## `/reg(?:(?:exp?(?:e?s)?)|(?:ular expressions?))/ig` -->

<h2><!--
--><code><!--
--><span class="fragment highlight-current-f60" data-fragment-index="1">/</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="2">reg</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="3"><span class="fragment highlight-current-f60" data-fragment-index="18">(?:</span></span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="4"><span class="fragment highlight-current-f60" data-fragment-index="12">(?:</span></span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="5">ex</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="6">p?</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="11"><!--
	--><span class="fragment highlight-current-f60" data-fragment-index="7"><span class="fragment highlight-current-f60" data-fragment-index="10">(?:</span></span><!--
	--><span class="fragment highlight-current-f60" data-fragment-index="8">e?</span><!--
	--><span class="fragment highlight-current-f60" data-fragment-index="9">s</span><!--
	--><span class="fragment highlight-current-f60" data-fragment-index="10">)</span><!--
-->?</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="12">)</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="13">|</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="14"><span class="fragment highlight-current-f60" data-fragment-index="17">(?:</span></span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="15">ular expression</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="16">s?</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="17">)</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="18">)</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="19">/</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="20">i</span><!--
--><span class="fragment highlight-current-f60" data-fragment-index="21">g</span><!--
--></code><!--
--></h2>
<br>
<ul style="float: left;">
  <li class="fragment" data-fragment-index="1">
    <span class="fragment highlight-current-f60" data-fragment-index="1">
    <span class="fragment highlight-current-f60" data-fragment-index="19">
    	Delimiter
    </span>
		</span>
  </li>
  <li class="fragment" data-fragment-index="2">
    <span class="fragment highlight-current-f60" data-fragment-index="2">
    <span class="fragment highlight-current-f60" data-fragment-index="5">
    <span class="fragment highlight-current-f60" data-fragment-index="9">
    <span class="fragment highlight-current-f60" data-fragment-index="15">
    	Literal symbols
		</span>
		</span>
		</span>
		</span>
  </li>
  <li class="fragment" data-fragment-index="3">
    <span class="fragment highlight-current-f60" data-fragment-index="3">
    <span class="fragment highlight-current-f60" data-fragment-index="4">
    <span class="fragment highlight-current-f60" data-fragment-index="7">
    <span class="fragment highlight-current-f60" data-fragment-index="14">
    	Non-capturing group
		</span>
		</span>
		</span>
		</span>
  </li>
  <li class="fragment" data-fragment-index="6">
    <span class="fragment highlight-current-f60" data-fragment-index="6">
    <span class="fragment highlight-current-f60" data-fragment-index="8">
    <span class="fragment highlight-current-f60" data-fragment-index="11">
    <span class="fragment highlight-current-f60" data-fragment-index="16">
			Optional (0 or 1)
		</span>
		</span>
		</span>
		</span>
  </li>
  <li class="fragment" data-fragment-index="10">
    <span class="fragment highlight-current-f60" data-fragment-index="10">
    <span class="fragment highlight-current-f60" data-fragment-index="12">
    <span class="fragment highlight-current-f60" data-fragment-index="17">
    <span class="fragment highlight-current-f60" data-fragment-index="18">
			End non-capturing group
		</span>
		</span>
		</span>
		</span>
  </li>
  <li class="fragment" data-fragment-index="13">
    <span class="fragment highlight-current-f60" data-fragment-index="13">
			Alternation (*or*)
		</span>
  </li>
  <li class="fragment" data-fragment-index="20">
    <span class="fragment highlight-current-f60" data-fragment-index="20">
			Modifier: Case insensitive
		</span>
  </li>
  <li class="fragment" data-fragment-index="21">
    <span class="fragment highlight-current-f60" data-fragment-index="21">
			Modifier: Global
		</span>
  </li>
</ul>

<ul class="fragment" style="float: right;">
	<li>regex</li>
	<li>regexs</li>
	<li>regexes</li>
	<li>regexp</li>
	<li>regexps</li>
	<li>regexpes</li>
	<li>regular expression</li>
	<li>regular expressions</li>
</ul>

===

# Syntax

---

## Metacharacters
<!-- .element: style="margin-top:-1em" -->

<!-- table style -->
<style>
	table.metacharactertable tr th:nth-child(3),
	table.metacharactertable tr td:nth-child(3),
	table.metacharactertable tr td:empty {
		border: none;
	}

	table.metacharactertable tr code {
		background: lightgray;
		padding: 0 .25em 0 .25em;
	}

	table.metacharactertable tr sup {
		font-size: 70%;	}
</style>

Symbol   | Meaning                                                                                                      |   | Symbol              | Meaning
:-------:|--------------------------------------------------------------------------------------------------------------|---|:-------------------:|--------
`\`      | Escape character                                                                                             |   | `.`                 | <span class="fragment highlight-current-f60" data-fragment-index="2">Any single character<sup>2</sup></span>
`^`      | <span class="fragment highlight-current-f60" data-fragment-index="1">Beginning of string<sup>1</sup></span>  |   | `[…]`               | Character class
`$`      | <span class="fragment highlight-current-f60" data-fragment-index="1">End of string<sup>1</sup></span>        |   | `[^…]`              | Negated character class
`?`      | Zero or one                                                                                                  |   | <code>&#124;</code> | Alternation
`*`      | Zero or more                                                                                                 |   | `-`                 | <span class="fragment highlight-current-f60" data-fragment-index="3">Character class range<sup>3</sup></span>
`+`      | One or more                                                                                                  |   |                     |
`{…}`    | Explicit quantification                                                                                      |   |                     |
<!-- .element: class="metacharactertable" -->

***

<ol class="stretch">
  <li class="fragment" data-fragment-index="1">
    Behavior can change with certain modifiers. <br/>
    Often the default behaviour is finding the start and end of individual lines in the input.
  </li>
  <li class="fragment" data-fragment-index="2">
    Typically does not match newline characters without using modifiers.
  </li>
  <li class="fragment" data-fragment-index="3">
    <p><code style="background: lightgray; padding: 0 .25em 0 .25em">-</code> is only a meta character if it is used between characters in a character class.<br/>
    E.g. `[-az]`, `[a\-z]` and `[az-]` all match ***a***, ***z***, and ***–***. `[a-z]` however matches any character between *a* and *z*. </p>
  </li>
</ol>


%SPEAK%
By *metacharacters* I mean characters that needs to be escaped if you want to search for them.

There is no `{,*n*}`, use `{0,*n*}` instead.


---

## Character classes

Let's say we want to find all lowercase letters in the english alphabet.
<!-- .element: style="margin-bottom: 1em" -->

<div class="fragment">
	<p>This will work…</p>
	<pre class="nohighlight" style="text-align: center; font-size: 100%;"><code>a|b|c|d|e|f|g|h|i|j|k|l|m|n|o|p|q|r|s|t|u|v|w|x|y|z</code></pre>
	<p>…but it's *quite* verbose.</p>
</div>
<div class="fragment">
	<p>Instead, we can use *character classes*, like so:</p>
	<pre class="nohighlight" style="text-align: center; font-size: 100%;"><code>[abcdefghijklmnopqrstuvwxyz]</code></pre>
</div>
<div class="fragment">
	<p>Or even better—we can use '–' to specify a range:</p>
	<pre class="nohighlight" style="text-align: center; font-size: 100%;"><code>[a-z]</code></pre>
</div>

Metacharacter escaping is only neccessary for those that have a special meaning within character classes.
<!-- .element: class="fragment" -->

---

## Shorthand character classes

Shorthand <!-- .element: width="30%" --> | Matches
-----------------------------------------|--------
`\w`                                     | Word character
`\d`                                     | Digit
`\s`                                     | Whitespace
`\v`                                     | Vertical whitespace
`\h`                                     | Horizontal whitespace
<!-- .element: width="50%" -->

Each shorthand can be negated by using the uppercase letter instead:
<!-- .element:  class="fragment" data-fragment-index="1" -->

Shorthand <!-- .element: width="30%" --> | Matches
-----------------------------------------|--------
`\W`                                     | Anything but a word character
`\D`                                     | Anything but a digit
`\S`                                     | Anything but a whitespace
`\V`                                     | Anything but a vertical whitespace
`\H`                                     | Anything but a horizontal whitespace
<!-- .element: width="50%" class="fragment" data-fragment-index="1" -->


%SPEAK%
Note that the shorthand character classes are particularly dependent on flavor.

---

## Negated character classes

<p class="fragment">Explicit character classes like `[abc]` can also be negated.</p>
<p class="fragment">`[^abc]` matches anything that isn't an **`a`**, **`b`** or **`c`**.</p>
<br>
<p class="fragment">Character classes—negated or not—can be used with the shorthand classes as well.</p>
<p class="fragment">`[\w\v]` matches any character that is either a *word character* or a *vertical whitespace*.</p>
<br>
<p class="fragment">By "exploiting" double negation, we can write some pretty neat shortcuts.</p>
<p class="fragment">For example, <span class="f60">`[^abc\W]`</span> matches the same characters as <span class="f60">`(?![abc])\w`</span>.</p>

%SPEAK%
Hopefully I will get to show some more cool usages of double negation later in the talk.

---

## Unicode character classes

Some flavors feature character classes for each of the Unicode categories, scripts and blocks.

These character classes are typically accessed with <span class="f60">`\p{Category_Name}`</span>.

<div class="fragment">
	<h3>Examples</h3>
	<table>
		<thead>
			<tr>
				<th>Type</th>
				<th>Usage</th>
				<th>Description</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>Category</td>
				<td>`\p{Lowercase_Letter}`</td>
				<td>Any lowercase letter from any script that has an uppercase variant</td>
			</tr>
			<tr>
				<td>Script</td>
				<td>`\p{Hiragana}`</td>
				<td>Any character from the hiragana syllabary</td>
			</tr>
			<tr>
				<td>Block</td>
				<td><code data-noescape>\p{<span class="f60">**In**</span>Hiragana}</code></td>
				<td>Any Unicode code point in the range `U+3040–U+309F`</td>
			</tr>
		</tbody>
	</table>
</div>

<p class="fragment">As with the other shorthand character classes, <span class="f60">**`\p{…}`**</span> can be negated with <span class="f60">**`\P{…}`**</span>.</p>

---

## A quick word on POSIX
<ul style="list-style-type: none;">
	<li class="fragment" data-fragment-index="1"><p data-fragment-index="1" class="fragment highlight-current-f60">You may come across the word '*POSIX*' when reading or referencing regex materials.</p></li>
	<li class="fragment" data-fragment-index="2"><p data-fragment-index="2" class="fragment highlight-current-f60">POSIX is an abbreviation for *"Portable Operating System Interface for uniX"*, which is a set of standards that a UNIX OS should support.</p></li>
	<li class="fragment" data-fragment-index="3"><p data-fragment-index="3" class="fragment highlight-current-f60">These standards define two flavors of regex, the *POSIX Basic Regular Expressions* (POSIX BRE) and *POSIX Extended Regular Expressions* (POSIX ERE).</p></li>
	<li class="fragment" data-fragment-index="4"><p data-fragment-index="4" class="fragment highlight-current-f60">The POSIX flavors are in many ways quite different from other (more modern) flavors, and many regex references will mention the POSIX character classes.</p></li>
	<li class="fragment" data-fragment-index="5">For example, the POSIX character class <span class="f60">**`[:digit:]`**</span> will match the same characters as <span class="f60">**`[0-9]`**</span>, <span class="f60">**`\d`**</span> and <span class="f60">**`\p{Decimal_Digit_Number}`**</span>.</li>
</ul>

===

## Quantifiers

<p class="fragment">~~Sometimes~~ *Very often* we want to repeat (a part of) our regex.</p>

<p class="fragment">Consider checking a piece of text to see if it contains a numbers-only substring of length 10.</p>

<p class="fragment"><span class="f60">`\d\d\d\d\d\d\d\d\d\d`</span> works, but it is neither very readable or scalable.</p>

<p class="fragment">By using quantifiers, we can tell the regex engine to repeat the `\d` part of our regex 10 times.</p>

<p class="fragment"><span class="f60">`\d{10}`</span> matches the same substrings as the regex above.</p>

---

## Quantifier reference
Syntax     <!-- .element: width="40%" --> | Meaning
:-----------------------------------------|--------
<code>&nbsp;?</code>                      | Zero or one
<code>&nbsp;*</code>                      | Zero or more
<code>&nbsp;+</code>                      | One or more
<code>{***n***}</code>                    | Exactly <code>***n***</code>
<code>{***n***,}</code>                   | <code>***n***</code> or more
<code>{***n***,***m***}</code>            | <code>***n***</code> to <code>***m***</code> (inclusive)

---

## Quantifier behaviour
<p class="fragment">Regex quantifiers are *greedy* by default.</p>

<br>
<br>

<div class="fragment">
<p>For example, the regex <span class="f60">`A\w*C`</span> applied to</p>
<pre class="nohighlight" style="text-align: center; font-size: 100%;"><code data-noescape>bbbCAbbbCAbbbCAbbb</code></pre>
</div>

<div class="fragment">
<p>won't just match</p>
<pre class="nohighlight" style="text-align: center; font-size: 100%;"><code data-noescape>bbbC<span class="f60">AbbbC</span>AbbbCAbbb</code></pre>
</div.fragment>

<div class="fragment">
<p>but instead</p>
<pre class="nohighlight" style="text-align: center; font-size: 100%;"><code data-noescape>bbbC<span class="f60">AbbbCAbbbC</span>Abbb</code></pre>
</div.fragment>

---

## Modifying the behaviour of quantifiers
<table>
	<thead>
		<tr>
			<th>Syntax</th>
			<th>Behaviour</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><code data-noescape>\w<span class="fragment highlight-current-f60" data-fragment-index="3">\*</span></code></td>
			<td>Greedy</td>
			<td>Matches as many characters as possible</td>
		</tr>
		<tr>
			<td><code data-noescape>\w<span class="fragment highlight-current-f60" data-fragment-index="3">\*</span>?</code></td>
			<td>Lazy</td>
			<td>Matches as few characters as possible</td>
		</tr>
		<tr>
			<td><code data-noescape>\w<span class="fragment highlight-current-f60" data-fragment-index="3">\*</span>+</code></td>
			<td>Possessive</td>
			<td>Will not backtrack</td>
		</tr>
	</tbody>
</table>

<p class="fragment" data-fragment-index="3">The <span class="f60">`*`</span> quantifier can of course be substituted by any of the other quantifiers.</p>


---

## Behaviour examples

<table>
  <thead>
    <tr>
      <th>Regex</th>
      <th>Behaviour</th>
      <th>Match</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code data-noescape>[ab]+b</code></td>
      <td>Greedy</td>
      <td><code data-noescape><span class="f60">aabaab</span></code></td>
    </tr>
    <tr>
      <td><code data-noescape>[ab]+?b</code></td>
      <td>Lazy</td>
      <td><code data-noescape><span class="f60">aab</span>aab</code></td>
    </tr>
    <tr>
      <td><code data-noescape>[ab]++b</code></td>
      <td>Possessive</td>
      <td><code data-noescape>aabaab</code></td>
    </tr>
  </tbody>
</table>

===

## Assertions

<p class="fragment">Sometimes we need to inspect our surroundings without actually matching them.</p>
<p class="fragment">Consider the input string <span class="f60">**`<h1>Replace me!</h1>`**</span></p>
<p class="fragment">If we want to replace the content of the `<h1>`-tag with <span class="f60">`I was replaced.`</span>, we could</p>
<ul>
	<li class="fragment">search for <span class="f60">**`<h1>.*?</h1>`**</span>, and</li>
	<li class="fragment">replace it with <span class="f60">**`<h1>I was replaced.</h1>`**</span></li>
</ul>
<p class="fragment">This example is not too bad, but one can imagine how some searches would need quite a bit of repetition.</p>
<p class="fragment">Assertions make it possible to inspect the string without consuming any characters.</p>
<p class="fragment">For example, we can achieve the same result as before by using regex *look-arounds* to</p>
<ul>
	<li class="fragment">search for <span class="f60">**`(?<=<h1>).*?(?=</h1>)`**</span>, and</li>
	<li class="fragment">replacing with <span class="f60">**`I was replaced.`**</span></li>
</ul>

---

## Assertion types

<table style="font-size: 90%;">
	<tbody>
		<thead>
			<tr>
				<th style="font-weight: normal; font-style: italic;">Usage</th>
				<th style="font-weight: normal; font-style: italic;">Description</th>
				<th style="font-weight: normal; font-style: italic;">Example</th>
			</tr>
		</thead>
		<tr>
			<td colspan="3" align="center">**Boundaries**</td>
		</tr>
		<tr>
			<td><code>\b</code></td>
			<td>Word boundary</td>
			<td><code>\bword\b</code></td>
		</tr>
		<tr>
			<td colspan="3" align="center">**Anchors**</td>
		</tr>
		<tr>
			<td><code>^</code></td>
			<td>Start of input (or line)</td>
			<td><code>^.\*$</code></td>
		</tr>
		<tr>
			<td><code>$</code></td>
			<td>End of input (or line)</td>
			<td><code>^.\*$</code></td>
		</tr>
		<tr>
			<td><code>\A</code></td>
			<td>Absolute start of input</td>
			<td><code>\A.\*$</code></td>
		</tr>
		<tr>
			<td><code>\Z</code></td>
			<td>End of input (ignoring blank lines)</td>
			<td><code>\Z\s</code></td>
		</tr>
		<tr>
			<td><code>\z</code></td>
			<td>Absolute end of input</td>
			<td><code>\s\z</code></td>
		</tr>
		<tr>
			<td colspan="3" align="center">**Look-arounds**</td>
		</tr>
		<tr>
			<td><code>(?=…)</code></td>
			<td>Positive look-ahead</td>
			<td><code>a(?=b)</code></td>
		</tr>
		<tr>
			<td><code>(?!…)</code></td>
			<td>Negative look-ahead</td>
			<td><code>b(?!a)</code></td>
		</tr>
		<tr>
			<td><code>(?<=…)</code></td>
			<td>Positive look-behind</td>
			<td><code>(?<=a)b</code></td>
		</tr>
		<tr>
			<td><code>(?<!…)</code></td>
			<td>Negative look-behind</td>
			<td><code>(?<!b)a</code></td>
		</tr>
	</tbody>
</table>

===

## Groups

<p class="fragment">We've already seen the non-capturing group <span class="f60">`(?:…)`</span> syntax a few times.</p>

<p class="fragment">As the name suggests, regex also has *non-non-capturing groups*—or just *capturing groups*.</p>

<p class="fragment">The syntax for capturing groups is simply <span class="f60">`(…)`</span>.</p>

<p class="fragment">These are particularly useful for *search & replace*, and can even be used for *backreferencing*.</p>

---

## Search and replace example

By searching for <span class="f60">`^(.*)$`</span> and replacing with <span class="f60">`Hello, $1!`</span> we can quickly change the following input

<pre data-fragment-index="1" class="fragment" style="font-size: 100%;">
<code class="nohighlight" data-noescape data-trim>
<span class="fragment highlight-current-f60" data-fragment-index="4">World</span>
<span class="fragment highlight-current-f60" data-fragment-index="5">GOTO</span>
<span class="fragment highlight-current-f60" data-fragment-index="6">regex</span></code></pre>

<p class="fragment" data-fragment-index="2">to the following output</p>

<pre data-fragment-index="3" class="fragment" style="font-size: 100%;" data-trim>
<code class="nohighlight" data-noescape data-trim>
<span class="fragment" data-fragment-index="4">Hello, <span class="fragment highlight-current-f60" data-fragment-index="4">World!</span></span>
<span class="fragment" data-fragment-index="5">Hello, <span class="fragment highlight-current-f60" data-fragment-index="5">GOTO!</span></span>
<span class="fragment" data-fragment-index="6">Hello, <span class="fragment highlight-current-f60" data-fragment-index="6">regex!</span></span></code></pre>

---

## Backreferencing

<p class="fragment" data-fragment-index="1">Recall that we can recognize a larger set of languages with regex compared to regular expressions.</p>
<p class="fragment" data-fragment-index="2">This is because we are able to reference a capture group *within the regex itself*.</p>
<p class="fragment" data-fragment-index="3">For example, <span class="f60">`(.)\1`</span> will match <span style="color: green">`aa`</span> and <span style="color: green">`>>`</span>, but not <span style="color: red">`xy`</span></p>

<br>
<p class="fragment" data-fragment-index="9">In addition to referencing the contents of a group, it is also possible to reference the *regex* for the group. <br>This is called a *subroutine*.</p>
<p class="fragment" data-fragment-index="10">Let's revisit our regex that matches palindromes.</p>

---

## Subroutines

<h3>`^((.)(?1)\2|.?)$`</h2>

<h4>Input: **`abba`**</h4>

Level | Group 1 | Group 2 | Trying to match | Finds
-----:|:-------:|:-------:|-----------------|-------
    0 |         | a       |                 | (?1)   
    1 |         | b       |                 | (?1)   
    2 |         | b       |                 | (?1)   
    3 |         | a       |                 | (?1)   
    4 |         |         |                 | $
    3 |         | a       | aa              | a$
    2 |         | b       | bb              | ba
    1 |         | b       | bb              | bb
    0 | bb      | a       | abba            | abba

%SPEAK%

Heavily simplified for readability; I am ignoring the fact that `.?` is greedy.

---

## Named groups

<p class="fragment">Capture groups and subroutines can be named for easier replacement and backreferencing.</p>
<p class="fragment">The syntax for named groups is very flavor-dependent, and the examples here might not work with your engine.</p>
<table class="fragment">
	<thead>
		<tr>
			<th>Syntax</th>
			<th>Meaning</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><code>(?&lt;name&gt;…)</code></td>
			<td>Creates a capture with the given <code class="f60">name</code></td>
		</tr>
		<tr>
			<td><code>\k&lt;name&gt;</code></td>
			<td>Matches what is currently in the <code class="f60">name</code> group</td>
		</tr>
		<tr>
			<td><code>(?P>name)</code></td>
			<td>Call the <code class="f60">name</code> subroutine</td>
		</tr>
		<tr>
			<td><code>${name}</code></td>
			<td>Insert the contents of <code class="f60">name</code> when replacing</td>
		</tr>
	</tbody>
</table>

<br>

<div class="fragment">
	<p>Here is our palindrome regex with and without named groups:</p>
	<pre style="font-size: 100%; width: "><code class="nohighlight" data-noescape>^(?&lt;palindrome&gt;(?&lt;first&gt;.)(?P&gt;palindrome)\k&lt;first&gt;|.?)$</code></pre>
	<pre style="font-size: 100%; width: "><code class="nohighlight" data-noescape>^((.)(?1)\2|.?)$</code></pre>
</div>

===



<!-- ##### EPILOGUE ##### -->
	
### Information
- http://www.rexegg.com <!-- .element: style="float: left;" -->
- http://www.regular-expressions.info/

### Libraries & Frameworks
- https://github.com/hakimel/reveal.js/

### Tools
- https://regex101.com/
- https://www.sublimetext.com/3


