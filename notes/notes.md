#General Info links and Notes

* [main demo page](https://bramp.github.io/js-sequence-diagrams/)
* [source](https://github.com/bramp/js-sequence-diagrams)
* [Issues Page](https://github.com/bramp/js-sequence-diagrams/issues)

Uses:

* [Raphael](http://raphaeljs.com/)
* either
  * [Underscrore](http://underscorejs.org/)
  * [lodash](https://lodash.com/)
* optionally
  * [JQuery](https://jquery.com/)
* [Daniel Font for handwriting](http://www.goodreasonblog.com/p/fontery.html)


Options:
* Use as a tool interactively, with default page
* Use as a library
* Explore configuration scope by using tool interactively with a different set of library components e.g. Raphael, lodash, no JQuery
* JS Heavy so risk of JS Compatibility errors
* Run and review the QUnit to identify coverage gaps and risks
* Create a new GUI page to control loaded components and minimise risk of custom editor interaction obscuring bugs or impacting testing
* compare 'standard' for sequence diagrams against implementation

* [x] Use font file as a coverage scope for actor names
* [] Use font file as a coverage scope for title, labels, notes
* [x] explore [documented ebnf](https://github.com/bramp/js-sequence-diagrams/blob/master/src/grammar.ebnf)
* [] Keyboard visible chars in name (only done top row)
* [] Identify other known special chars? (# and \n found so far but not explored)
* [] explore comment positioning in a diagram
* [] title formatting and positioning
* [] large/long diagrams
* [] explore \n in actor name, title, arrow text, notes
* [x] notes relative positioning to actors, left, right, over



#Lessons:

* Library execution from console
  - Diagram.parse("A->B:\nB->C:").drawSVG($('#demo').find('.diagram').get(0));

* Markdown notes writeup
  * Can read as .txt
  * Can easily convert to pdf via [dillinger.io](http://dillinger.io/)
  * Images embedded within text are not rendered (possibly good for adhoc image tracking)
  * like a text to diagram, a text to document allows embedded comments (images, html comments <!-- this won't be seen --> )

* Tool as documentation
  * since the diags support # as a comment we can use that to our advantage when documenting the testing or raising defects e.g. urls, environment, versions - all stored in the diagram txt
  * Use the title as the issue name or test ideal
  * Create minimal source to recreate the issue and embed in defect report

* Text to diagrams
  * Fast to create, autolayout
  * Can tweak for 'better' layout e.g. \n, aliases and naming
  * Learn the nuances of the tool
  * Version control and compare previous versions
  * Easier to auto generate the diagram source than a diagram equivalent
  * Use the 'comments' functionality for meta data and notes
  * Human readable at text, visual a different 'view'

* Environment Control important - see the cross browser (Chrome/Firefox Participant issue)
  * What version is the demo site running?
  * Does the editor interfere? Cross-Browser Risk

* Tester has different view of tool support required from testing
  - compare test.html with "explorer.html"
  - shows both simple/hand graph at same time
  - tracks history of usage
  - minimal libraries to avoid risk of interference
  - (but mine is buggy on IE because I don't know JS as well)
  - display parse errors on screen

* Cross browser testing
  - testing what? GUI, Rendering, Parsing?

* Observe, Interrogate, Manipulate
  * Console
  * JS Debugger - harder with minimised code (use pretty print in console)
  * Network Tab

#Notes and Issues Summary

* 20150708 15:00 - 17:00 (missing about 30 mins in middle of this) 1.5
  * used the system for a while to understand how it works
  * looked at the source to be a feel for the size and scope
  * read the docs for validation limits on characters
  * explored the font file used to find any rendering mismatches - found some
  * Can I use the js-sequence-diagrams as a library from the dev console? Yes
  * Markdown for note taking
* 20150709 10:20 - 12:00 (missing about 15 mins in middle of this) 1.5
  * used the grammar diagram as a basis for creating a diagram and combining the different options listed  found a path error and explored position of text items in the input and naming of verbs
  * adding images into evernote works well for my notes and visual scanning, and copy paste into markdown editor means images are not carried forward, so no need to filter
  * Using the tool to create minimal examples that illustrate the issues should work well for issue recreation, will use for all future isssues
* 20150710
     * tooling and cross browser tool
* 20150713
  * cross browser investigation
  * write up

##Issues:

* ISSUE: the char "¦" is not represented in the hand drawn diagram actor name, but it is in simple

~~~~~~~~
Title: the char "¦" is not represented in the hand drawn\n diagram actor name, but it is in simple 
¦->A:
# May be a char encoding issue - see source
~~~~~~~~

* ISSUE: the char "\" is represented as / in hand drawn actor name but \ in simple

~~~~~~~~
Title: char "\" is represented a "/" in hand drawn\n actor name but "\" in simple
\->A:
~~~~~~~~

* ISSUE: some characters `ΩμΣΠ⋲Δ∕·ˉŁłŠšŽž−->b:` do not render in actor names in handwriting mode, even though they are in the handwriting font used

~~~~~~~~
Title: some chars do not render in handwriting mode in actor
ΩμΣΠ⋲Δ∕·ˉŁłŠšŽž−->b:
# May be a char encoding issue - see source
~~~~~~~~

* ISSUE: spaces before the actor name are trimmed but spaces after the actor name are significant. so `  B` and `         B` are the same but `B->` and `B ->` are different actors

~~~~~~~~
Title: Issue: space before actor names are trimmed\nbut spaces after are significant\nleading to duplicates if not careful
A->B : here
B ->A : back atcha
~~~~~~~~

* ISSUE: if you use a participant, you must add an alias for it to be usable in the diagram, otherwise you create a duplicate actor box.
e.g.

~~~~~~~~
Title: Issue: duplicate ActorA for participant without alias in demo app
Participant ActorA
ActorA->ActorB:
# this fails with  https://bramp.github.io/js-sequence-diagrams/  on chrome (windows)
# but works with url on firefox
# and works in Chrome on Mac
# works in test app in chrome
~~~~~~~~


~~~~~~~~
Title: Alias participants do not create duplicate actors
Participant ActorA as A
A->ActorB:
~~~~~~~~

* ISSUE: If you use an alias with a participant you can only use the alias throughout the diagram otherwise it creates a duplicate actor
~~~~~~~~
Title: can not mix alias and name in same diagram
Participant ActorA as A
A->ActorB:
ActorA->ActorB:
~~~~~~~~

* ISSUE: with the [demo editing page](https://bramp.github.io/js-sequence-diagrams/) the error display in the editor is on line 1 and not the line you are editing which makes it harder to spot errors when editing a large diagram.

* ISSUE: when using `Note over B,A` if that is a right to left relationship then no box outline is drawn in simple mode, but it is in hand drawn mode.
~~~~~~~~
Title: Over box outline not drawn in simple theme
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Note over B,A:over B,A
~~~~~~~~

* ISSUE: when using `Note over B,A` if that is a right to left relationship then a shorter box outline is drawn in handwriting mode than if `Note over A,B` is used
~~~~~~~~
Title: Over box outline drawn short in hand drawn mode
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Note over B,A:over A,B
~~~~~~~~
~~~~~~~~
Title: Over box outline drawn well
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Note over A,B:over A,B
~~~~~~~~

* ISSUE: participants have to be listed at the top of the text sequence prior to aliases being used, otherwise the actor names are not displayed

~~~~~~~~
C-B:C-B
Note over A,C:over A,C
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Title: Issue: participants have to be listed \nprior to any note or actor relationships\nor the names are not displayed
~~~~~~~~
~~~~~~~~
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
C-B:C-B
Note over A,C:over A,C
Title: participants actor names used\n when listed at top of text
~~~~~~~~

##Notes:

* NOTE: "," is not allowed in an actor name
* NOTE: "-" is not allowed in an actor name
* NOTE: ":" and ">" are not allowed in an actor name
* NOTE: diagram to achieve grammar coverage

~~~~~~~~
Title: This is a simple diagram with all grammar paths

#Participants

Participant Actor A as A
Participant Actor B as B
Participant Actor C as C

# Issue with `Participant actor` path
#Participant Actor D
#Actor D->Actor D:

# Actor message paths
A->B: A->B
A-->B: A-->B
B->>C: B->>C
B-->>C: B-->>C
C-B: C-B
C--B: C--B
B-A: # B-A no message

# Some recursive loops
A-A:A-A
B--B:B--B
C->C:C->C
C-->>C:C-->>C

# Notes
Note left of A:#empty note
Note left of B: left of B
Note right of A: right of A
Note over C: over C
Note over A,B:over A,B
~~~~~~~~


# Session Notes

##20150708 15:00  Learn about tool and explore ways of interacting

[x] quick scan site and source

[x] view source to see which libraries used on main demo page
  - JQuery
  - Raphael
  - Underscore
  - also the [ace editor](http://ace.c9.io/#nav=about) from a cloudfront.net host

There may be a risk that lodash and lack of JQuery not covered by the automated QUnit checking.

Quick overview of \test folder suggests gaps, parser grammar seems to have some test coverage but suspect main JS has a manual visual check.

[x] Traffic?

After loading - none

[x] quick random char test

Issues:
ISSUE: the char "¦" is not represented in the hand drawn diagram, but it is in simple
ISSUE: the char "\" is represented as / in hand drawn but \ in simple

Notes:
NOTE: "," is not allowed in an actor name
NOTE: "-" is not allowed in an actor name
NOTE: ":" and ">" are not allowed in an actor name

TODO: investigate the switch between simple and handdrawn to identify other sources of risk

[-] Keyboard visible chars in name

Try as actor name al*an

~~~~~~~~
al`¬¦1!2"3£4$€5%6^7&8*9(0)-_=+an
~~~~~~~~

Editor throws an error on above - what happens if I use that string in the sequence as a library, rather than the GUI?

[x] use console to figure out how to experiment with the library on the demo page

Since it uses JQuery we can use JQuery to interrogate and manipulate the dom and JS

Simplest diagram `"A->B:"`

find diagram

~~~~~~~~
$('#demo').find('.diagram')
~~~~~~~~

reset diagram

~~~~~~~~
$('#demo').find('.diagram').html('')
~~~~~~~~

From src

~~~~~~~~
Diagram.parse(editor.getValue());
    diagram_div.html('');
diagram.drawSVG(diagram_div.get(0), options);
~~~~~~~~

Below would rule out any ace editor bugs or validation obscuring issues with the js-diagram (although in this case I think the error was caught from the parsing, rather than editor generated)

~~~~~~~~
Diagram.parse("A->B:").drawSVG($('#demo').find('.diagram').get(0));
~~~~~~~~

~~~~~~~~
Diagram.parse("al`¬¦1!2"3£4$€5%6^7&8*9(0)-_=+an->B:").drawSVG($('#demo').find('.diagram').get(0));
~~~~~~~~

oops, error - did not escape the " in the string

~~~~~~~~
Diagram.parse("al`¬¦1!2\"3£4$€5%6^7&8*9(0)-_=+an->B:").drawSVG($('#demo').find('.diagram').get(0));
~~~~~~~~

Parse error

~~~~~~~~
...$€5%6^7&8*9(0)-_=+an->B:
-----------------------^
Expecting 'MESSAGE', got 'LINE'
...$€5%6^7&8*9(0)-_=+an->B:
-----------------------^
Expecting 'MESSAGE', got 'LINE'
~~~~~~~~

there are a set of valid/invalid chars in the name validation

Syntax diagram at the bottom of the page does not say what the valid values in an actor are, does the bison file?

https://github.com/bramp/js-sequence-diagrams/blob/master/src/grammar.jison

Hmmm, I can't tell by reading the grammar.

Brute force editing to find the issue

~~~~~~~~
al`¬¦1!2"3£4$€5%6^7&8*9(0)-
~~~~~~~~

does not throw an issue in `al`¬¦1!2"3£4$€5%6^7&8*9(0)-->B:`

OK, rule - can't have "-" in an actor name

at all?

nope

~~~~~~~~
-a->b:
-ac->b:
a-c->b:
a - c->b:
~~~~~~~~

OK

~~~~~~~~
a  c->b:
~~~~~~~~


OH! "-" == LINE in the grammar

Can't see way to escape it from the bison so no '-' in actor name


[-] Other known special chars?

From docs on page
`#` is a comment
`\n` new line

### 16:00 Break

### 16:20 Font investigation

Uses the Daniel font from  http://goodreasonblog.blogspot.com/p/fontery.html

in the pdf doc of the above font the following chars are listed and / is among them

[pdf doc](C:\Users\Alan\Downloads\danielfont\danielfont\Font license - Daniel.pdf)

Use this as an oracle for the font conversion since I assume these should all be rendered due to them appearing in the font

~~~~~~~~
Daniel Regular
!”#$%&’()*+,./
0123456789:;<=>?@
ABCDEFGHIJKLMNOPQRSTUVWXYZ
[\]^_`abcdefghijklmnopqrstuvwxyz
{|}~ÄÅÇÉÑÖÜáàâäãåçéèêëíìîïñóòôöõúùûü†°¢
£§•¶ß®©™´¨ ≠ÆØ∞ ±≤≥¥ μ∂ΣΠπ∫ªºΩ æø
¿¡¬ √ƒ⋲Δ «»…
ÀÃÕOEoe–—“”‘’÷ ◊ÿŸ∕ ‹›fifl‡·
‚„‰ÂÊÁËÈÍÎÏÌÓÔ ÒÚÛÙıˆ˜ˉ˘˙°¸˝˛ˇ ŁłŠšŽž|DdY
Tt−123...€
~~~~~~~~

paste the above in as actor names

~~~~~~~~
!”#$%&’()*+,./
~~~~~~~~

NOTE: "," is not allowed in an actor name

~~~~~~~~
0123456789:;<=>?@
~~~~~~~~

NOTE: ":" and ">" are not allowed in an actor name

only

~~~~~~~~
0123456789;<=?@
~~~~~~~~


~~~~~~~~
ABCDEFGHIJKLMNOPQRSTUVWXYZ
[\]^_`abcdefghijklmnopqrstuvwxyz 
{|}~ÄÅÇÉÑÖÜáàâäãåçéèêëíìîïñóòôöõúùûü†°¢
£§•¶ß®©™´¨ ≠ÆØ∞ ±≤≥¥ 
~~~~~~~~

  odd that \ would be translated to / even though the font supports it

~~~~~~~~
μ∂ΣΠπ∫ªºΩ æø
¿¡¬ √ƒ⋲Δ «»…
ÀÃÕOEoe–—“”‘’÷ ◊ÿŸ∕ ‹›fifl‡·
‚„‰ÂÊÁËÈÍÎÏÌÓÔ ÒÚÛÙıˆ˜ˉ˘˙°¸˝˛ˇ ŁłŠšŽž|DdY
Tt−123...€
~~~~~~~~

Also Does not render

~~~~~~~
ΩμΣΠ
⋲Δ
∕·

ˉŁłŠšŽž
−
~~~~~~~

ISSUE: some characters `ΩμΣΠ⋲Δ∕·ˉŁłŠšŽž−->b:` do not render in actor names in handwriting mode, even though they are in the handwriting font used

~~~~~~~~
Title: some chars do not render in handwriting mode in actor
ΩμΣΠ⋲Δ∕·ˉŁłŠšŽž−->b:
~~~~~~~~

### 16:40 Plan Next Options

* Keyboard visible chars in name (only done top row)
* Identify other known special chars? (# and \n found so far but not explored)
* explore comment positioning in a diagram
* title formatting and positioning
* large/long diagrams
* explore \n in actor name, title, arrow text, notes
* notes relative positioning to actors, left, right, over
* explore [documented ebnf](https://github.com/bramp/js-sequence-diagrams/blob/master/src/grammar.ebnf)

Added above to the Plan in the head of this doc.

## 20150709 10:20

[-] explore [documented ebnf](https://github.com/bramp/js-sequence-diagrams/blob/master/src/grammar.ebnf)

* Is order of title, participant etc in the file important?

[-] Create a simple diagram following the grammar using many different elements

~~~~~~~~
Title: This is a simple diagram with all grammar elements
Participant Actor A as A
Participant Actor B as B
Participant Actor C as C
A->B: A to B using alias
Actor B -> Actor C:
~~~~~~~~


Note, I may have misunderstood the alias, I assumed that after creating an alias I could refer to either the alias or the actor name, but the above creates a duplicate.

~~~~~~~~
 B -> C:
B -> C:
B -> C :
 B-> C :
~~~~~~~~

After investigation it seems as though spaces before the actor name are trimmed but spaces after the actor name are significant. so `  B` and `         B` are the same but `B->` and `B ->` are different actors.

Issue: spaces before the actor name are trimmed but spaces after the actor name are significant. so `  B` and `         B` are the same but `B->` and `B ->` are different actors

~~~~~~~~
Title: Issue: space before actor names are trimmed\nbut spaces after are significant\nleading to duplicates if not careful
A->B : here
B ->A : back atcha
~~~~~~~~

Simplify `Actor B -> Actor C:` to `B->Actor C:`

And this creates a duplicate "Actor C"


~~~~~~~~
Title: This is a simple diagram with all grammar elements
Participant Actor A as A
Participant Actor B as B
Participant Actor C as C
A->B: A to B using alias
B->Actor C: Alias to name
B->C: alias to alias
~~~~~~~~

Is this a problem with aliases and names or because of names with spaces?


~~~~~~~~
Participant Actor A
Participant Actor B
Participant Actor C
Actor B->Actor C:
Actor A->Actor B:
Actor C->Actor B:
~~~~~~~~

I didn't expect this to create the Actors multiple times

Try without spaces

~~~~~~~~
Participant ActorA
Participant ActorB
Participant ActorC
ActorB->ActorC:
ActorA->ActorB:
ActorC->ActorB:
~~~~~~~~


Nope, not spaces.

Participants only seem to 'work' when there is an alias and you use the Alias throughout the diagram and not the name.

[x] minimise to illustrate issues:


ISSUE: if you use a participant, you must add an alias for it to be usable in the diagram, otherwise you create a duplicate actor box.

~~~~~~~~
Title: This creates a duplicate ActorA
Participant ActorA
ActorA->ActorB
~~~~~~~~
~~~~~~~~
Title: Alias participants do not create duplicate actors
Participant ActorA as A
A->ActorB:
~~~~~~~~

ISSUE: If you use an alias with a participant you can only use the alias throughout the diagram otherwise it creates a duplicate actor
~~~~~~~~
Title: can not mix alias and name in same diagram
Participant ActorA as A
A->ActorB:
ActorA->ActorB:
~~~~~~~~

? does participant 

10:54

[x] Continue to create a simple diagram following the grammar using many different elements, to then check element ordering

NOTE: only use aliases because we want to use participants as they are part of the grammar and allow us to order

~~~~~~~~
Title: This is a simple diagram with all grammar paths

#Participants

Participant Actor A as A
Participant Actor B as B
Participant Actor C as C

# Issue with `Participant actor` path
#Participant Actor D
#Actor D->Actor D:

# Actor message paths
A->B: A->B
A-->B: A-->B
B->>C: B->>C
B-->>C: B-->>C
C-B: C-B
C--B: C--B
B-A: # B-A no message

# Some recursive loops
A-A:A-A
B--B:B--B
C->C:C->C
C-->>C:C-->>C

# Notes
Note left of A:#empty note
Note left of B: left of B
Note right of A: right of A
Note over C: over C
Note over A,B:over A,B
~~~~~~~~

Note: as the above grew, I found myself editing it outside as the editor was slow in handwriting mode and you couldn't see the error as the error was only shown on the first line, switched to simple theme for interactive editing.

* ISSUE: with the [demo editing page](https://bramp.github.io/js-sequence-diagrams/) the error display in the editor is on line 1 and not the line you are editing which makes it harder to spot errors when editing a large diagram.

The above file covers all the paths listed in the grammar, but the path 'participant actor' has an error as found earlier.



[x] Expect ordering to make a difference since the items are shown in encountered order

Good - by moving B to the end it was possible to make `note over A,B` span all three

minimised:
~~~~~~~~
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Note over A,B:over A,B
~~~~~~~~

Can I reverse A,B?

~~~~~~~~
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Note over B,A:over B,A
~~~~~~~~

No.


... but only in simple mode, handwriting draws a box, but box is not was wide is if `Note over A,B:over A,B` is used


* ISSUE: when using `Note over B,A` if that is a right to left relationship then no box outline is drawn in simple mode, but it is in hand drawn mode.
~~~~~~~~
Title: Over box outline not drawn in simple theme
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Note over B,A:over B,A
~~~~~~~~

* ISSUE: when using `Note over B,A` if that is a right to left relationship then a shorter box outline is drawn in handwriting mode than if `Note over A,B` is used
~~~~~~~~
Title: Over box outline drawn short in hand drawn mode
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Note over B,A:over A,B
~~~~~~~~
~~~~~~~~
Title: Over box outline drawn well
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Note over A,B:over A,B
~~~~~~~~

[x] Do participant definitions have to be done at the top of the file?

Yes, otherwise the labels are not redrawn with the actor name.  That kind of feels like a bug.


Try with title, does that have to be at the top? No, Title can be anywhere, for that reason I will class participant positioning as an issue.

* ISSUE: participants have to be listed at the top of the text sequence prior to aliases being used, otherwise the actor names are not displayed

~~~~~~~~
C-B:C-B
Note over A,C:over A,C
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
Title: Issue: participants have to be listed \nprior to any note or actor relationships\nor the names are not displayed
~~~~~~~~
~~~~~~~~
Participant Actor A as A
Participant Actor C as C
Participant Actor B as B
C-B:C-B
Note over A,C:over A,C
Title: participants actor names used\n when listed at top of text
~~~~~~~~

[x] explore [documented ebnf](https://github.com/bramp/js-sequence-diagrams/blob/master/src/grammar.ebnf)
uppercase lower case in verbs?

uppercase/lowercase mix doesn't seem to make a difference
TiTle
title
TITLE
participant
PartICIpant
PARTICIPANT
etc. for Note,NOTE,noTE 

AAARgh - keep accidentally hitting a key that jumps me out of the browser editing window and losing my work

12:00 Planned Stop

20150710 Cross Browser 11:40

Came across 

http://doesitworkonedge.com/

Tried it on the test page 

http://doesitworkonedge.com/Home/Results/3239

Links to http://caniuse.com which checks for compatible html features across browsers.

Page created by http://www.browseemall.com/

Which claims to be a single tool which integrates all browsers. I haven't really trusted these tools in the past. But it has a SeleniumWebDriver implementation.

[x] TODO: follow up with browseemall.com for more technical info on how the tool works.

Normally I'd use the manual testing in SauceLabs or BrowserStack for compatibility testing.

Not convinced I trust the results from browseemall yet (different dev tools) but might be of some use?

12:10
[x] create a version of the page that lets me easily add examples for cross browser rendering testing

and add the diagrams to it - create examples for all issues that didn't have any

14:00

break

14:15

Examine the test.html

https://github.com/bramp/js-sequence-diagrams/blob/master/test/test.html

Seems like a simple page that would remove any issues with the editor conflict - make local by bringing in dependencies from cdn

14:44

Add 'tracking' to the 'test' page to make it an exploratory testing tool

see explorer.html

15:20

JQUery is supposed to be optional - remove it from the explorer page to allow checking for this

15:25

can now use this explorer.html to bring in different versions and remove libraries as required, and track testing more easily.

15:30 break

16:55 experiment with chars in other areas

* [x] Use font file as a coverage scope for title, labels, notes

* ISSUE: Notes, Titles, Links and Actors all have the same rendering issues from simple to handdrawn

~~~~~~~~
Title: ISSUE: known actor issue chars in ΩμΣΠ¦⋲Δ∕·ˉŁłŠšŽž title, note and link
ΩμΣΠ¦⋲Δ∕·ˉŁłŠšŽž->B:ΩμΣΠ¦⋲Δ∕·ˉŁłŠšŽž
Note over B:ΩμΣΠ¦⋲Δ∕·ˉŁłŠšŽž
~~~~~~~~

D:\Users\Alan\Documents\Documents\Compendium Developments\black_ops_testing\webinars\js-sequence-diagrams\see session recording 20150710_1655_explore_known_issue_chars_in_titles_etc.html

17:05 break


# 20150712 22:00 Skype with James, Tony and Steve

Tried to replicate some of the issues I'd seen, but couldn't in the cross browser page. They only appear with  https://bramp.github.io/js-sequence-diagrams/  on chrome but not firefox (Steve pointed this out)

# 20150713 08:00 - 08:30

[x] investigate cross platform issue from https://bramp.github.io/js-sequence-diagrams/ with

~~~~~~~~
Title: Issue: duplicate ActorA for participant without alias in demo app
Participant ActorA
ActorA->ActorB:
# this fails with  https://bramp.github.io/js-sequence-diagrams/  on chrome (windows)
# but works with url on firefox
# and works in Chrome on Mac
# works in test app in chrome
~~~~~~~~

- Use the same js libraries in the crossBrowserTestPage and try to recreate
- re-check all the bugs on the cross browser page and add comments into the text
- when I use the actual JS files from the page the issue does not occur - is it an issue triggered by interaction via the editor?
- flag issue in the comment in the diagram description but no further investigation performed

