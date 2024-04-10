---
tags:
- neovim
- terminal
- coding
---

Insert mode is an important mode in vim. I've put together a cheatsheet with 8 tips and tricks to use insert mode more efficiently.
## Different ways to get into insert mode

There are different ways to get into insert mode besides `i`. Here are some of them:  

```
i      " insert text before cursor
I      " insert text before the first non-blank character of the line.
a      " append text after cursor
A      " append text at the end of line
o      " starts a new line below cursor and insert text
O      " starts a new line above cursor and insert text
gi     " insert text in same position where last insert mode was stopped in current buffer
gI     " insert text at the start of line (column 1)
```

Notice the lowercase/ uppercase pattern.

## Different ways to exit insert mode

There are several ways to exit insert mode back to normal mode.  

```
<esc>     " exits insert mode and go to normal mode
Ctrl-[    " exits insert mode and go to normal mode
Ctrl-c    " like Ctrl-[ and <esc>, but doesn't check for abbreviation
```

## Repeating Insert Mode

You can give a count before you enter insert mode. For example:  

```
10i
```

Then if you type "hello world!" and exit insert mode, it will repeat the text "hello world!" 10 times. This works with any methods you use to enter insert mode (`10I, 11a, 12o`)

## Deleting chunks in insert mode

There are different ways to delete while in insert mode besides `<backspace>`:  

```
Ctrl-h      " delete one character
Ctrl-w      " delete one word
Ctrl-u      " delete entire line
```

By the way, these shortcuts also work in command-line mode and Ex mode too, not just vim.

## Inserting from register

Registers are like spaces in memory to store texts. To learn more about registers, check out `:h registers`.

Vim can insert contents from any register with `Ctrl-r` plus register symbol. You can use any alphabetical character a-z for _named register_. To use it, if you want to yank _a word_ to register a, you can do:  

```
"ayiw
```

To insert content from register a:  

```
Ctrl-r a
```

Vim also has 10 numbered registers (0-9) to save the most recent 10 yanks/deletes. To insert content from numbered register 1:  

```
Ctrl-r 1
```

In addition to named and numbered registers, here are another useful register tricks:  

```
Ctrl-r "    # insert the last yank/delete
Ctrl-r %    # insert file name
Ctrl-r /    # insert last search term
Ctrl-r :    # insert last command line
```

Vim has an expression register, =, to evaluate expressions.

You can evaluate mathematical expressions `1 + 1` with:  

```
Ctrl-r =1+1
```

Another way to get values from registers is with `@` followed by register symbol.  

```
Ctrl-r =@a
```

This works with any registers mentioned above, including `",/,%`. I think the first method is a little faster, but I just want to show a different way. 

You can also evaluate basic vim script expression. Let’s say you have a function  

```
function! HelloFunc()
    return "Hello Vim Script!"
endfunction
```

You can evaluate its value just by calling it.  

```
Ctrl-r =HelloFunc()
```

## Scrolling

Did you know that you can scroll while inside insert mode?   
You can use Vim’s `Ctrl-x` _sub-mode_.  

```
Ctrl-x Ctrl-y   " scroll up
Ctrl-x Ctrl-e   " scroll down
```

## Autocompletion

Vim has a built-in autocompletion. Although it is not as good as [intellisense](https://code.visualstudio.com/docs/editor/intellisense) or any other Language Server Protocol (LSP), but for being so lightweight and available right out of the box, Vim’s autocomplete is a very capable feature.

You can use autocomplete feature in insert mode by invoking `Ctrl-x` sub-mode.  

```
Ctrl-x Ctrl-l   " insert a whole line
Ctrl-x Ctrl-n   " insert a text from current file
Ctrl-x Ctrl-i   " insert a text from included files
Ctrl-x Ctrl-f   " insert a file name
Ctrl-x Ctrl-]   " insert from tags (must have tags)
Ctrl-x Ctrl-o   " insert from omnicompletion. Filetype specific.
```

You can also autocomplete without using `Ctrl-x` with:  

```
Ctrl-n
Ctrl-p
```

To move up and down the pop-up window, use `Ctrl-n / Ctrl-p`.

Autocomplete is a vast topic in Vim. This is just the tip of the iceberg. To learn more, check out `:h ins-completion`.

## Executing normal mode command

While in insert mode, if you press:  

```
Ctrl-o
```

You'll be in `insert-normal` sub-mode. If you look at mode indicator on bottom left, normally you will see `-- INSERT --`, but `Ctrl-o` will change it to `-- (insert) --`. You can do _one_ normal mode command. Some things you can do:

**Centering and jumping**  

```
Ctrl-o zz    " center window
Ctrl-o H/M/L " jump to top/middle/bottom window
Ctrl-o 'a    " jump to mark 'a'
```

**Repeating text**  

```
Ctrl-o 100ihello
Ctrl-o 10Ahello
```

**Executing command line**  

```
Ctrl-o !! curl https://google.com
Ctrl-o !! pwd
```

The possibility is endless. 