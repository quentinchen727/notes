# Learning VIMScript the hard way
Vimscript isn't going to help you much if you wind up fidding with your editor
all day instead of working, so it's important to strike a balance.
Practice makes perfect.
Learing Vim and Vimscript is a different experience from learning a normal
programming language.
You need to type in all the commands, and do all the exercises.

## Creating a Vimrc file
:echo $MYVIMRC to find the location and name of the configuration file.

## 1 Echoing Messages
" ephemeral message
:echo "hello"

" Persistent echoing
:echom "Hello again" // print to a list of messages. Use messages to check.
:messages // can help to debug

" Comments
// sometimes it doesn't work

## 2 Setting Options
2.1 Boolean Options
:set number
:set nonumber
" All booleans work the same way
:set <name>
:set no<name>

2.2 Toggling Boolean Options
:set number!
:set <name>!

2.3 Checking Options
:set num?
:set <name>?

2.4 Options with Values
:set numberwidth? // check the value of numberwidth
:set numberwidt=10
:set <name>=<value>
:set <name>?

2.5 Setting Multiple Options at Once
:set number numberwidth=6

:set relativenumber is equal to :set rnu
:wrap
:shiftround applies to > or <{lines} to shift lines left or right.
:set shiftwidth=4; :set sw=4;
:help matchtime; //number of tenth of a sec to show matching paren.

## 3 Basic Mapping
:map - dd

3.1 Special characters
:map <space> viw
:map <c-d> dd
:map <keyname>  to tell Vim about special keys

3.2  Commenting
:map <keyname>  to tell Vim about special keys
:map <space> viw "something " // Vim comment don't work

## 4 Modal Mapping
Mappings also take effect in visual mode.
:nmap \ dd " normal mode mapping
:vmap "visual mode mapping
:imap "insert mode mapping

4.1 Muscle memory

4.2 Insert Mode
:imap <c-d> <esc>ddi

## Strict Mapping
recusive mapping
:nmap - dd
:namp \ -

" unmap key
:nunmap -

:nunmap \

5.1 Recursion has side effects

5.2 Nonrecursive mapping
:nmap x dd
:nnoremap \ x  // will does whatever it do for x by default
Each of the *map comands has a *noremap counterpart: nnoremap, inoremap,
vnoremap.
Always, seriously, use *noremap version.*

## 6 Leaders
In normal mode, -, H, L, <space>, <cr> and <bs> are seldomly used.

6.1 Mapping key sequences
Unlike Emacs, Vim makes it easy to map more than just single keys.
:nnoremap -d dd
:nnoremap -c ddO

6.2 Leader
:let mapleader = "-"
customize your own leader key.

6.3 Local Leader
a prefix for mappings that only take effect for certain types of files, like
Python files or HTML files.
:let maplocalleader = "\\"  // first to escape the \ character

## 7 Editing your Vimrc

7.1 Editing Mapping
:nnoremap <leader>ev :vsplit $MYVIMRC<cr>
ZZ to save the change.
:nnoremap <leader>sv :sourec $MYVIMRC<cr>

$MYVIMRC: first places will be searched and the first existing one will be
used.

## Abbrevations
for use in insert, replace, and command modes.
:iabbrev adn and

8.1 Keyword characters
Vim will substitute an abbreviation when you type any "non-keyword character"
after an abbreviation.
set iskeyword?
The abbreviation will be expaneded when you type anything that's not a letter,
number or underscore.

8.2 More abbreviations
:iabbrev qch@qch.com qch@qch.com

8.3 Why not use mapping?
Mappings don't take into account what characters come before or after the ap.

## 9 More mappings
:nnoremap <leader>" viw<esc>a"  // add an pair of enclosing quote around the
word.

`<: go to left top corner of the visual block
`>: go to the right bottom corner of the visual block
<number>H goes to the nth line from the current top.

## 10 Training your fingers
Ways to exit insert mode in Vim:
- <esc>
- <c-c>
- <c-[>

10.1 Learning the map
:inoremap <esc> <nop> // mapping esc to a non-functional key

**When you want to change a habit, make it harder or impossbile to do!**
The right way to use Vim is to get out of insert mode as soon as you can and

## 11 Buffer-LOcal Options and Mappings
:nnoremap <leader>d dd
:nnoremap <buffer> <leader>x dd // define a buffer-local mapping

11.2 Local leader
When you create a mapping that only applies to specific buffers you should use
<localleader> instead of <leader>.
Using two seperate leader keys provides a sort of "namespace" that will help
you keep all your various mappings straight in your head.
It is even more important when you write a plugin for someone else.

11.3 Settings
setlocal wrap
NOt all options can be used with setlocal.

11.4 Shadowing
:nnoremap <buffer> Q x
:nnoremap Q dd
The first is more specific and it will shadow the second one.

:set nu  // will set the global and local
:setl nu // will set the local, and local will have higher priority
:setg nu // only set the global
:unmap <buffer> Q

## 12 Autocommands
:autocmd BufNewFile * :write

12.1 Autocommand Structure
:autocmd BufNewFile * :write
:autocmd event_to_watch_for pattern_to_filter_the_event command_to_run
You can't use special characters like <cr> in the command.

12.2 Another example
:autocmd BufWritePre *.html :normal gg=G
*indent before writing.

12.3 Mulitple Events.
:autocmd BufWritePre,BufRead *.html :normal gg=G
*A common idiom is to pair BufRead and BufNewFile together to run a command
whenever you open a certain kind of file, regardless of whether it exists or
not.
:autocmd BufNewFile,BufRead *.html setLocal nowrap
*s

12.4 FileType Events
:autocmd FileType javascript nnoremap <buffer> <localleader>c I//<esc>
:autocmd FileType python nnoremap <buffer> <localleader>c I#<esc>

## 13 Buffer-local abbreviations
:iabbrev <buffer> --- &mdash;

13.1 autocommands and abbreviations
:autocmd FileType python :iabbrev <buffer> iff if:<left>
:autocmd FileType javascript :iabbrev <buffer> iff if()<left>

## 14 Autocommands Groups
Vim has no way of knowing if you want it to replace an existing one. It is
possbile to creaet duplicate autocommands. Every time you source your ~/.vimrc,
you will be duplicating autocommands.
:autocm BufWrite * :sleep 200m // source again, and it will slow more.

14.2 Grouping autocommands
:augroup testgroup
:   autocmd bufwrite * :echom "foo" // the case of event doesn't matter
:   autocm bufwrite * :echom "Bar"
:augroup END
:augroup testgroup
:autocmd bufwrite * :echom "BAZ"
:augroup END
It will not replace testgroup, instead adding it to the first one.

:augroup testgroup
:   autocmd!
:   autocmd bufwrite * :echom "Cats"
:augroup END
:autocmd! clear the group.

## 15 Operator-Pending Mappings
Operator: d, c, y
An operator is a command that waits for you to enter a movment command, and
then does something on the text between you currently are and where the
movement would take you.

15.1 Movement mappings
:onoremap p i(
def count(i):
    i += 1
    print i
    return foo
:onoremap b /return<cr>
When you define a new operator-pending movement, think of it like this:
1. start at the curosr position.
2. enter visual mode(charwise).
3. ..mapping key go here...
4. All the text you want to include in the movement should now be selected.

15.2 Changing the start
:onoremap in( :<c-u>normal! f(vi(<cr>
:onoremap il( :<c-u>normal! F)vi(<cr>

15.3 General rules
1. If your operator-pending mapping ends with some text visually selected, vim
   will operate on that text.
2. Otherwise, Vim will operate on the text between the original curosr position
   and as a command.the new position.
normal! to execute the commands in the normal mode, and <cr> to execute the
normal! command.
<c-u> to ignore the range that vim may insert;
command line mode by default operate on a single line.

## 16 More operator-pending mappings
16.1 Normal
:normal gg
:normal >>
takes a set of characters and performs whatever action they would do if they
were typed in normal mode.

16.2 Execute
It takes a Vimscript string and performs it as a command.
:normal! gg/a<cr> doesn't work, as normal! doesn't recognize special characters
like <cr>.
:onoremap ih :execute "normal! ?^==\\+$\r:nohlsearch\rkvg_"<cr>
execute will substitute any special characters it finds befoer running it.
g_ moves to the last non-black character of the current line, not including the
newline.
^: first nonblank character.
:onoremap ah: <c-u>execute "normal! ?^==\+$\r:nohlsearch\rg_k0"<cr>
:help pattern-overvieas a command.
:help expr-quote
" Change the next email address
:onoremap in@ :execute "normal! /@\r:nohlsearch\rBvt@"<cr>
normal! to prevent remapping.

## 17 Status lines
set statusline=%f\ -\ FileType:\ %y
relative filepath and file type
Use the backslash to escape the spaces.
:set statueline+=%l\ /%L
current line and total lines.

17.1 Width and Padding
:set statusline=[%4l]
:set statusline=Current:\ %-4l\ Total:\ %-4L   "Padding to the right"
:set statusline=%.20F  "Truncate the full path to 20 characters at most"
:set statusline=%= "switch to the right side for the followings

:augroup python_statusline
    :autocmd!
    :autocmd FileType python setlocal statusline=%f\ %F
:augroup END
Always wrap the autocommands in groups to prevent duplications.

## 18 Responsible coding
18.1 commenting
Be defensive when writing anything that takes more than a few lines of
Vimscripts. Add a comment explaining what it does, and if there is a relevant
help topic, mention it in the comment!.

18.2 Grouping
Use Vim's code folding capabilities and group lines into sections.
" VimScripe file settings----------- {{{
augroup filetype_vim
    autocmd!
    autocmd FileType vim setlocal foldmethod=marker
augroup END
" }}}
Uset git to sync up the configure. Create a symbolic link to ~/.vimrc.

18.3 Short Names
:setlocal wrap
:setl wrap

## 19 Variables
:let foo= "bar"
:let foo = 11

19.1 Options as variables
:set textwidth=80
:echo &textwidth
Use the & to tell Vim that you're referring to the option, not a variable that
happens to have the same name.
Vim treats any non-zero integer as "truthy".

this also sets the options:
:let &textwidth = 100
:set textwidth?

:let &textwidth = &textwidth + 10
When you set an option using set, you can only set it to a single literal
value.

19.2 Local options
:let &l:number = 1
:let &l:number = 0

19.3 Registers as variables
** registers **
- unnamed "": text deleted with "d", "D", "c", "C", "s", "S" or copied with
  "y", regardless of whether or not a specific register was used. It is always
pointing to the last used register, except the blackhole "_.
The contents of the unnamed is used for any put command used without a
specified register.
- numbered "0 to "9: 0 contains the text from the most yank command not
  specified by any other regiester, while 1 contains the most recently deleted
or changed one, unless the command specified another register or the text is
less than one line, then the "- is also used. With each sucessive deletion or
change, the contents will be shifted to higher registers.
- small "-
whether or not a specific register was used. It is always
pointing to the last used register, except the blackhole "_.
The contents of the unnamed is used for any put command used without a
specified register.
- named "a to "z or "A to "Z: small case for replacing, big case for appending.
- ". : last insert text
- "% : the name of the current file
- ": : the most rencet executed command line.
- "# : the name of the alternate file
- "* : used in GUI
- "~ : drag and drop content
- "_ : blackhole registers
- "/ : the most recent search-pattern. U could change it to different content.


  Use @ to access registers.  :let @a = "hello!"
:echo @a //
"ap  // paste the contents of registers a here.
:echo @/   "echo the latest search
:echo @"    /* echo the latest copy */
Never use let if set will suffice.

## 20 Variable scoping
:let b:hello = "world"
:echo b:hello  // local to the current buffer
scoping: b, w, t, g, v, s, a, l.
:help internal-variables

## 21 Conditionals
21.1 multiple-line statements
:echom "foo" | echom "bar"    // use | to seperate each line with a pipe
character |.

21.2 Basic if
:if 1
:    echom "ONE"
:endif
- Vim will try to coerce variable(and literals) when necessary. 10 + "20foo"
  will be 30;
- Strings that starts with a number are coerced to that number, otherwise
  they're coerced to 0.

21.3 Else and Elseif
:if 0
: echom :if"
:elseif "nope!"
:   echom "elseif"
:else
:   echom "finally"
:endif

## 22 Comparisons
22.1 Case sensitivity
:set noignorecase
:if "foo" == "FOO"
:   echom "vim is case insensitive"
:elseif "foo" == "foo"
:   echom "vim is case sensitive"
:endif
The behavior depends on a user's settings.

22.2 Code defensively
:set noignorecase
:if "foo" ==? "FOO"
:   echom "first"
:endif    // ==? is the "case-insensitive" regardless of setting

:set ignorecase
:if "foo" ==# "FOO"
:   echom "one"
:elseif "foo" ==# "foo"
:   echom "two"
:endif
==# is the case-sensitive version regardless of user setting.
Always use ==# and ==? regardless of string or integer.

## 23 Functions
:function Meow()
:   echom "Meow!"
:endfunction
Vimscript functions must start with a capital letter if they are unscoped!
Or with a s: prefix(script local)

23.1 Calling functions
:call Meow()
:echom GetMeow()

23.2 Implicit returning
:echom Meow()
If the function doesn't explicitly return a value, it will return 0 from the
function, which is falsy.

## 24 Function Arguments
:function Display(name)  " take at most 20 arguments"
:echom "Hello! My name is :"
:echom a:name
:endfunction
Always need to prefix those arguments with a: when you use them to tell Vim
that they're in the argument scope.

24.1 Varargs
:function Varg(...)
:   echom a:0  "number of total vargs"
:   echom a:1  "the first varg"
:   echo a:000 " the list of vargs"
:endfunction

24.2 Assignment
You can't reassign argument variables.
:function Assign(foo)
: let a:foo = "nope"  "Error"
: echom a:foo
:endfunction

But inside function, let foo_tmp = a:foo is ok.
:help function-argument
:help local-variables

## 25 Numbers
25.1 Number formats
decimal, hex(0x), octal(0)
25.2 Float formats
15.3e9 // decimal and number after it is not optional.
25.3 Coercion
When u combein a number and a float, vim will cast the number to a float.

## 26 Strings
26.1 Concatenation
:echom "Hello" + "a" // display 0
vim's + operator is only for numbers. Vim will not corece string to floats.
:echom "hello" . " , world" // use . to concantenate.
:echom 10 . "foo" // 10foo
:echom 10.1 . "foo" // error
Vim is a lot like javascript: it allows you to play fast and loose with tyes
sometimes, but it's really a bad idea to do so because it will come back to
bite you some point.
26.2 Speical Characters
:echo "foo\nbar"  // echo the escaped characters.
:echom "foo\nbar" // will echo the exact characters of strings.
26.3 literal strings
:echo '\n\\' // display literal strings
:echom 'That''s enough' // Two single quotes is the only sequence that has
special meaning in a literal string.
26.4 Truthienes
String will be evaluated to 0.
:help expr-quote

## 27 String Functions
27.1 Length
strlen("foo")
len("foo")
27.2 splitting
:echo split("one two three")
or split("one,two,three", ",")
27.3 Joining
join(["foo", "bar"], "...")
27.4 lower and upper case
tolower("Foo")
toupper("Foo")
:help functions

## 28 Execute
:execute "rightbelow vsplit " .bufname("#")

## 29 Normal
:normal! G "! to avoid remapping
When writing Vim scripts you should always use normal!, and never use plain old
normal.
normal doesn't recognize any special characters.
:help helpgrep
<c-o> to excute a command in insert mode and return to insert mode.
:nn <leader>d ddi<c-g>u<esc>dd  " This breaks the undo sequence. We can undo
one line one time.

## 30 Execute Normal!
:execute "normal! gg/foo<cr>dd"
:execute "normal! mqA;\<esc>`q"  // add a ; to the end and return to the
original position.
'a return to the marked row, `a returns to the marked row and column

## 31 Basic Regular Expressions
31.1 Highlighting
:set hlsearch incsearch
:execute "normal! G?print\<cr>"
31.3 Magic
:execute "normal! gg/for .\\+ in .\\+:\<cr>"
- First, execute takes a String, so the double backslashes we used turn into
  single backslashes by the time they get to normal!
- Vim has four different "mode" of parsing regular expressions! The default
  mode requires a backslash before + character.
31.4 literal strings
:execute "normal! gg" . '/for .\+ in .\+:' . "\<cr>"
We need \<cr> in the pattern to be escaped into a real carriage return
character, which tells the search command to run.
31.5 Very magic
:execute "normal! gg" . '/\vfor .+ in .+:' . "\<cr>"
If you simple start all of your regular expression with \v you'll never need to
worry about Vimscript's three other crazy regex modes.
Use of "\v" means that in the pattern after it all ASCII characters except
'0'-'9', 'a'-'z', 'A'-'Z' and '_' have a special meaning.  "very magic"_

<c-r> to paste content from registers.
:match Error /\v\s+$/   "highlight trailing whitespace"
:match     "clear the latest match"

:nnoremap <leader>w :match Error /\v\s+$/<cr>
:help pattern-overview
:help hlsearch
:help incsearch
:help nohlsearch

## 32 Case Study: grep operator
:help quickfix-window
:copen
:ccl
:cw
:help :grep
:help :make
32.3 A preliminary sketch
:nnoremap <leader>g :grep -R something .<cr> --
32.4 The search term
:nnoremap <leader>g :grep -R '<cword>' .<cr>
<cword> means the current word under the cursor used in command-line mode.
<cWORD> means the WORD under the cursor.
Use '' to escape any harmful shell character in <cword> or <cWORD>
32.5 Escaping Shell Command Arguments
:nnoremap <leader>g :exe "grep -R " . shellescape(expand("<cWORD>"))." ."<cr>
that's
32.6 Cleanup
:nnoremap <leader>g :silent exe "grep! -R " . shellescape(expand("<cWORD>"))." ."<cr>:copen<cr>

:copen [height]
:cnext :cprevious
:help silent
:silent! // ignore the error msg

## 33 Case study: grep operator, part two
33.1 Create a File
Inside ~/.vim/plugin/, create a grep-operator.vim.
33.2 Skeleton
nnoremap <leader>g :set operatorfunc=GrepOperator<cr>g@
function! GrepOperator(type)
    echom "Test"
endfunction
g@ calls the defined function.
33.3 Visual Mode
vnoremap <leader>g :<c-u>call GrepOperator(visualmode())<cr>
<c-u> delete all characters before the cursor
call anyFunction
33.4 Motion Types
    echom a:type
mode: v, V, , char, line.
viw<leader>g: mode v
Vjj<leader>g: mode V
<leader>giw: mode char
<leader>gG: mode line
33.5 Copying the text
echom @@ // @@ is the unamed register that Vim places text into when you yank
or delete without specify a particular register.
33.5 Escaping the Search term
grep! does not jump to the first line of serach, but the resuts are stored in
the quicklist.
:help visualmode()
:help c_ctrl-u
:help operatorfunc
:help map-operator

## 34 Case Study: Grep Operator, Part three
function! GrepOperator(type)
    let saved_unnamed_register = @@
    if a:type ==# 'v'
        normal! `<v`>y
    elseif a:type ==# 'char'
        normal! `<v`>y
    else
        return
    endif
    silient execute "grep! -R " . shellescape(@@) . " ."
    copen
    let @@ = saved_unnamed_register
endfuntion

34.2 Namespacing
nnoremap <leader>g :set operatorfunc=<SID>GrepOperator<cr>g@
function! s:GrepOperator(type) : s: means only search in current script
namespace.
<SID> will be expanded into <SNR>number_function

## 35 Lists
heterogenous list: [1, 2, ['a',3]]
index like python, from front or back.
slice [12,2,3][0:2]
U can safely exceed the uppper bound.
U can also index and slice string. But u cannot use negatkive indeces.
Concantenation: ['a', 'b'] + ['c'] creates a new list.
add(list, 'a'), len(list), get(list, 0, 'default'), index(foo, 'b'), join(foo,
'seperator')

## 36 Looping
for i in [1,2,3]
    let c += 1
endfor

while c<= 4
endwhile

## 37 Dictionaries
similiar to Python's dicts, Ruby's hash, and Js's objects.
Value are heterogenous, but keys are always coerced to strings.
U can add a common after the last element in the dictionary: {'a' :1, 1 :
'foo',}.
Vimscript coerces the index to a string before performing the lookup.
two styles of lookup: [] or .a if the key is a string consisting only of
letters, digits and/or underscores.
remove(foo,'a') remvoes entries from a dictionary and returns the value.
unlet will only remove the entry.
has_key(foo, 'a')
keys(foo), values(foo) pull keys/values.

## 38 Toggling
function! FoldColumnToggle()
    if &foldcolumn
        setlocal foldcolumn=0
    else
        setlocal foldcolumn=4
    endif
endfunction

## 39 Functional Programming
use function as variables and use data structure with immutable state.

let Myfunc = function("Pop"). If function variable, names must start with a capital letter.

High-order functions are functions that take other functions and do something
with them.

## 40 Paths
expand('%'): % means the current file
expand('%:p'): :p tells Vim that you want the absolute path.
fnamemodify('foo.txt',':p') : more flexible vim fucntion.

echo globpath('.', '*'): return a string of all files seperated by newlines.
echo split(globpath('.', '*'), '\n'): if using **, it means recursive.

helpful:
expand(), fnamemodify(), filename-modifiers, simplify(), resolve(), globpath(),
wildcards

## 42 Plugin Layout in the dark ages
Inside ~/.vim:
- colors: for all the colorschems
- plugin: run once every time Vim starts.
- ftdetect: filetype dection. Run every time you start Vim. Should set up
  autocommands taht detect and set the filetype of files and nothing else.
- ftplugin: The naming of these files matters. If you set filetype=foo, vim
  will look for foo.vim in ~/.vim/ftplugin/. If that file exists, it will run
it. Or it will look for ~/.vim/ftplugin/foo/, and execute all the files inside.
It should only setlocal, as each a buffer's filetype is set. Local only.
- indent: indentation related to each file type. Local only.
- compiler: compiler-related options. Local only.
- after: loaded every time Vim starts, but after the files in the
  ~/.vim/plugin/. This allows you to `override` Vim internal files.
- autoload: delay the loading of your plugin's code until it is actually
  needed.
- doc: documentation.

RuntimePath:
:set runtimepath=/user/qch/sss

Pathogen or Plug will add any directory inside ~/.vim/bundle into your
runtimepath.

Vim automatically wraps the contens of ftdetect/*.vim files in autocommand
groups for you.
au BufNewFile,BufRead *.pn set filetype=potion

Syntax Highlighting
if exists("b:current_syntax")
    finish
endif

let b:current_syntax="potion" // Those two blocks prevents reloading it again,
just like header guard in C.

To highlight a piece of syntax:
1. define a "chunk" of syntax using `syntax keyword` or related command, e.g.
    syntax keyword potionKeywords loop times to while
2. link "chunks" to highlighting group, e.g.
    highlight link potionKeyword Keyword
syntax keyword does not reset the syntax group -- it adds to it!

syntax keyword potionFunctions print join string
highlight link potionFunctions Function

:help iskeyword, group-name, syn-keyword

# open link in vim
`gx`
