## Why visual mode

If you used a different text editor/IDE, you can probably highlight a text block and apply changes to it. Vim's visual mode works like that: you can select a text block and apply changes to it.

You can apply changes to a text object with vim's normal mode, but there are times when visual mode is better for the job. Suppose you have these:  

```
const one = "ONE";
const TWO = "Two";
```

You want to lowercase the section starting at the "N" in `"One";` and stopping before "T" in `"Two";`.  

```
const one = "One";
const two = "Two";
```

You can't do this easily with normal mode, because you are changing only _part_ of line one and two, not the entire two lines. With visual mode, you can visually select "n" up until before "T", and apply visual-mode lowercase operator (`u`).

Visual mode can be useful if you want to target a specific group of text that doesn't follow distinguishable pattern.

_(Actually in this case you can in normal mode, with your cursor on "N", do `gu/Two`: "apply lowercase from here until the next instance of phrase 'Two'". I also find it more intuitive doing this in visual mode.)_

## Entering visual mode

There are three _different_ visual modes in Vim.  

```
v          " character-wise visual mode
V          " line-wise visual mode
Ctrl-v     " block-wise visual mode
```

Character-wise visual mode is used to micro-select individual characters.

Line-wise visual mode selects lines. It is common in programming to apply changes line-wise. In the early programming days, many editors are _line_-based, like [Ed](https://en.wikipedia.org/wiki/Ed_(text_editor)).

Block-wise visual mode allows you to select on "columns" and "rows". It gives you more freedom to move around in this mode than the first two. There are also certain things you can do only in block-wise visual mode (I will cover them later).

On bottom left of your Vim window, you will see either `-- VISUAL --`, `-- VISUAL LINE --`, or `-- VISUAL BLOCK --` to indicate which visual mode you are in.

While you are inside a visual mode, you can switch to another visual mode by pressing either `v`, `V`, or `Ctrl-v`. For example, if you're in line-wise visual mode and you want to switch to block-wise visual mode, just type `Ctrl-v`. Try it!

There is one more way to enter visual mode:  

```
gv
```

This will start visual mode on the same selected area as the last visual mode. Suppose you just applied uppercase operation on a text block and forgot to apply another operation to the same text block. Instead of going back, reselecting, and applying operation, you can skip the first two steps if you used `gv`. It will make the same selection area as the last one, so you can just apply the operation you need.

Pretty neat!

## Moving in visual mode

Once you're in a visual mode, you can expand our selection with vim motions, like `hjkl`. For more on motions, check out `:h motion.txt`. 

While you are in visual selection, you will see your cursor on one end of the selection. It could either be on the top-left or bottom-right. You can expand your selection unidirectionally wherever your cursor is. So if your cursor is on the bottom end, pressing `j` will expand selection downward, but pressing `k`will not expand selection upward, but _contract_ selection upward. What if you need to expand your selection upward and downward? 

You need to change our cursor location to the direction you're expanding.

You can change the your cursor location with `o` or `O` while in visual selection. So if your cursor is on bottom of the selection and you want to expand upward, change cursor location with `o` and go up with `k`.

## Exiting visual mode

There are three ways to exit visual mode:

- `esc`
- `Ctrl-c`
- Same key as your current visual mode

The first two makes sense. Here's what the last one means: If you're currently in line-wise visual mode (`V`), you can exit line-wise visual mode if you press `V` again. If you're in character-wise visual mode, pressing `v` will exit visual mode. If you're in block-wise visual mode, pressing `ctrl-v` will exit visual mode.

## Visual mode operators

Vim is a modal editor. It means that the same key may work differently depending on the mode. Luckily, many keys in visual mode overlap functionalities with normal mode keys. If you know your normal mode operators, you're good. If not, don't worry, you just need to remember the important ones (they are mnemonics-friendly).

To use visual mode operators, first visually select an area of text (`v/V/Ctrl-v`+ motion), then press a visual-mode operator key. That's it.

Here are some most common ones:  

```
u        " lowercase
U        " uppercase
d        " delete
c        " change
y        " yank
>        " indent
<        " dedent
```

For more operators, check out `:h visual-operators`.

## Visual mode and ex commands

You can apply ex commands on visually selected areas. Suppose you have text:  

```
const one = "one";
const two = "two";
```

And you want to substitute `const` with `let` only on these two lines. You can highlight both lines with line-wise visual mode (`V`), and perform substitution with:  

```
:s/const/let/g
```

Any ex command will work with visual mode.

## Repeating visual mode

Vim's dot command (`.`) repeats the last change. When you repeat a visual mode operation, the same operation will be applied to the same text block.

Suppose you deleted these two lines with line-wise visual mode (`Vjd`).  

```
const one = "one";
const two = "two";
```

The next time you use dot command, it will also delete the next _two_ lines. 

## Inserting multiple texts

You can insert multiple texts with block-wise visual mode.

Suppose you have these and you want to add semicolon at the end of each line.  

```
const one = "one"
const two = "two"
const three = "three"
```

Here's how:

- Start with our cursor on first column, first line (the "c" in `const one`)
- Select block-wise visual mode, go down two more lines (`Ctrl-v 2j`). It will select all the "c"'s
- Horizontally select to the end (`$`)
- Append (`A`)
- Type `;`
- Exit visual mode (`esc`)

Now you'll see `;` appended to all lines. In block-wise visual mode, you can append or insert texts on multiple lines with `A` or `I`. You can also replace multiple characters with `r{char}`.

A way to insert characters on multiple lines using _any_ visual mode is to use `:normal!` command (`:h normal`). This command allows us to execute normal mode command. 

Here's how you can insert `;` with character-wise visual mode:

- select all 3 lines (`v2j`)
- type `:normal! A;`

PROTIP: if you're not in block-wise visual mode and want to append (`A`) or insert (`I`) texts, remember you can _switch to block-wise visual mode_ with `Ctrl-v` from whatever other visual mode you are in right now.

## Incrementing numbers

You can increment columns of numbers with `Ctrl-a/Ctrl-x` in vim. Here's how you can apply it to multiple lines.

Suppose you have a text:  

```
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
```

It's not a good practice to have multiple ids with same name. Let's increment them. Put your cursor on the _second_ line, on number "1" on "app-1" text:

- Start block-wise visual mode, go down 3 more lines (`Ctrl-v 3j`). You should be selecting all the remaining "1"'s.
- Type `g Ctrl-a`.

That's it! Now everything is incremented:  

```
<div id="app-1"></div>
<div id="app-2"></div>
<div id="app-3"></div>
<div id="app-4"></div>
<div id="app-5"></div>
```

**Extra**: `Ctrl-x/Ctrl-a` can increments letters too, with:  

```
:set nrformats+=alpha
```

So if you have:  

```
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
```

Using the same technique as above (`Ctrl-v 3j g Ctrl-a`) to increment the "id"'s:  

```
<div id="app-a"></div>
<div id="app-b"></div>
<div id="app-c"></div>
<div id="app-d"></div>
<div id="app-e"></div>
```

## Selecting the last visual mode area

You learned that `gv` can select the last visual mode selection. But did you know that you can go to the _location_ of the start and the end cursor of last visual mode?  

```
`<        " go to the last place of last visual mode selection
`>        " go to the first place of last visual mode selection
```

## Entering visual mode from insert mode

Normal mode is not the only mode you can enter visual mode from. You can enter visual mode from insert mode. 

While you're in insert mode, do:  

```
Ctrl-o v
```

- In insert mode, you can execute a normal mode command with `Ctrl-o`.
- While you are in this normal-mode-ready, run visual mode key `v`.
- If you notice, on the bottom left, it will say `--(insert) VISUAL--`.

This works with any visual mode key: `v/V/Ctrl-v`. 

## Extra: Select mode

In addition to visual mode, vim has a _select mode_. Like visual mode, select mode has three modes:  

```
gh        " character-wise select mode
gH        " line-wise select mode
gCtrl-h   " block-wise select mode
```

Select mode emulates regular editor's text highlighting behavior closer than visual mode. In regular editor, after you highlight a block of text, if you type letter "a", it will delete and replace the highlighted block of text with letter "a". Select mode does that: after you made your selection in select mode, if you type letter "a", it will immediately replace your selection with letter "a". 

I personally never used it, but it's good to know it exists.

## Conclusion

We made it to the end. We covered a lot of grounds. 

We learned how to enter and exit visual mode. We learned that there are 3 types of visual modes in Vim: character-wise, line-wise, and block-wise. We saw that visual mode shares many operators with normal mode. We learned that visual mode operation is repeatable with dot command (`.`). We learned to increment multiple rows of numbers/ letters. We learned how to enter visual mode not only from normal mode, but also from insert mode. Lastly, we learned the existence of select mode.

Thank you for reading this far. Hope you learned a new thing or two.

Happy coding!