Mardown is used as a format for writing for the web, while html for publishing.
For any markup that is not covered by Markdown's syntax, you simple use HTML itself.  There is no need to preface or delimit it.
The only restriction iare theat block-level html --e.g., <div>,<table>,<pre>,<p> ,etc. -- must be separated fromsurrounding content by blank lines, and the start and end tages of the block should not be indented with tabs or spaces.
Markdown syntax is not processed within block-level html tags.
Span-level html tags--e.g., <span>, <cite>, or <del> --can be used anywhere in a Markdown paragraph. And markdown syntax is processed within span-level tags.
Markdown translate < or & for you into &lt or &amp automatically if need.

## Block elemtns
1. paragraphs and line breaks
   A paragraph is simple one or more consecutive lines of text, separated by one or more blank lines. Markdown doesn't translate every line break in a paragraph into a <br /> ag.
   Normal paragraphs should not be indented with spaces or tabs.
   When you do want to insert <br /> break tag using Mardkown, you end a line with two or more spaces, then type return.

## Headers
1. underlined: ====== or------; any number will work
2. Atx-style use 1-6 hash characters ath the start of the lin.

## Blockquotes
Markdown uses email-style > characters for blockquoting.
you can put > in every line or just the first line.
Blockquotes can be nested or contain other markdown elements, e.g.:
> blalba
blala
or 
> blabla
> blabla

## lists
Unordered lists uses *, +, or -.
Ordered lists use nubmers, and the sequence and value of the numbers do not matter.
But if you use laze list numbering ,you should start the list with number 1.
* like this
and blabla  // no need to indent
list items may consist of multiple paragraphs. each subsequent paragraph in a list item must be indented by either 4 spaces or noe tab.
indent a blockquote within a list item.
Indent twice a code block withinin a list item.
escape : 1986\.

## code blocks
Markdown wraps a code block in both <pre> and <code> tags.
simple indent every line of the block by at least 4 spaces or 1 tab.
regular markdown or html is not processed within code blocks.

## horizontal rules
place thredd or more hypens, asterisks, or undersocres.

##Span elements
### links
    [foo](url links)
    [foo](local position)
    [foo][reference]
    [google][]  // shortcut for [google][google]

### emphasis
single or double surrounding * or _ without spaces as a indicate of emphasis.
use \* to escape *.
    [reference]: link "title"

### code 
Use a pair of backquote `` to indicate a span of code.
or use a pair of `` `` to quote backquote inside.
It can also escape html characters.

###images
    ![alt text](path/to/img.jpg)
    ![alt text](path/to/img.jpg "Optional title")
    ![Alt text][id]
    [id]: url/to/image "optional title"

### Automatic links
    <http url>
    <address@foo.com>

### backsplash escapes
\* provides backslash escape for the following characters:
\, `, *, _, {}, [], (), #, +, -, ., !

