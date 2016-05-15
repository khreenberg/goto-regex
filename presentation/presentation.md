<!-- ##### WELCOME AND ABOUT ##### -->

<!-- .slide: data-background="#000" -->
## GOTO Night
<h1 class="f60">Regular Expressions</h1>

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
    <td class="value">Guest461</td>
  </tr>
  <tr>
    <td class="leftcol">Password</td>
    <td></td>
    <td class="value">Easvzgap</td>
  </tr>
</table>

*Win7/Linux go to <span class="f60">`cp.easv.dk`</span> to login.*

---

## About me
- Software Development PBA student
- Lazy programmer
- Self-taught regular expressions disciple

---

## About this talk

Q: Why should we even care about formal languages?  
A: Because they are *sets* of words. This means that we can search for more multiple things in the same search!

***TODO*** <!-- .element: style="color: red" -->

***AGENDA*** <!-- .element: style="color: red" -->


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

<!-- .element: style="width: 70%" -->


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

<!-- ##### EPILOGUE ##### -->
	
### Information
- http://www.rexegg.com <!-- .element: style="float: left;" -->
- http://www.regular-expressions.info/

### Libraries & Frameworks
- https://github.com/hakimel/reveal.js/
- https://github.com/calevans/external

### Tools
- http://www.regexpal.com/ 
- https://www.sublimetext.com/3


