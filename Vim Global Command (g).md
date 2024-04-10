---
tags: 
- neovim
- terminal
- coding
---

The global command (`:h :global`) is used to run Ex-commands on multiple lines.

If you want to delete a line, Vim has the `:d` (`:h :delete`) command (or you can press `dd`).

Wait a second - if I want to run the delete command on multiple lines, can't I just run `:%d` to delete all the lines in my buffer? Yes, you definitely can.

Where the global command shines, is that it lets you to execute the delete command on all the lines _matching the pattern_. So if you want to delete all the lines containing "foo", without the global command, you may need to first _find_the lines with foo (`/foo`), then delete them (`dd` or `:d`). Repeat the process as many times as there are lines containing foo in your buffer. This can get tedious if there are tens, if not hundreds of lines. With the global command, you can run `:g/foo/d` once and - voila! All foo lines are deleted.

This is the power of the global command. It lets you to apply a command to multiple lines at once. In this post, I will show you how to unlock some of the global command's potentials.

## What Commands Can I Run With the Global Command?

When you go to the help section for the global command, `:h :global`, it says something like:

> Execute the Ex command cmd (default ":p") on the lines within range where {pattern} matches.

So what are the Ex commands that I can run with the global command?

You can find a list of Vim's ex commands with `:h ex-cmd-index` (warning: there are hundreds of them). I've been using Vim for years and frankly, I only use very, very few of them. I believe the trick here is to use the few tools that you have well.

For the sake of brevity, based on my personal experience, I found the following 8 commands useful to work with the global commands (if you have a command that you use often and it is not listed here, please let me know!)

- Delete
- Substitute
- Normal
- Print
- Move
- Put
- Copy
- Sort

### CMD: Delete

The delete command deletes line(s). Type `:d` to delete the current line, `:1,5d` to delete lines 1-5, `:%d` to delete the whole buffer, and `:+3d` to delete the third line below your cursor.

With the global command, you can delete multiple lines.

To delete all lines containing `console`: `:g/console/d`.

To delete all lines that do **not** end with a number 0-9: `:g/[^0-9]$/d`.

When we run the delete command, Vim stores the deleted text in the numbered register. If you prefer not to modify your register, you can run the blackhole (`_`) delete command instead: `:g/console/d_`. It is a little more verbose but it preserves your registers! If you don't use the number register that often, you can just use the regular delete command.

### CMD: Substitute

The substitute command (`:h :substitute`) substitutes a string with a new string. Type `:s/foo/bar` to sub foo with bar.

To substitute all to_s with to_f on all lines containing puts: `:g/puts/s/to_s/to_f`.

### CMD: Normal

Vim has a `:normal` command to execute normal mode commands. If you type `:normal Ahello`, it's similar to pressing `A` (append text at the end of the line) then typing "hello". Think of it like running a brief normal-mode script.

If you want to insert "//" before all `import` texts, you can run: `:g/import/normal I//`. This will effectively comment out all the import commands (even better, if you have a comment plugin and `gcc` is set to toggle comment, you can also run `:g/import/normal gcc`).

You can also execute macro (`:h macro`) using the normal command. Assuming you've recorded a macro in the register `a`, to execute your macro on all lines containing `console`: `:g/console/normal @a`.

### CMD: Put

The `:put` command (`:h :put`) lets you to put a text from the Vim register.

Recall that registers are what Vim uses to store yanked (copied) or deleted files. For more, check out `:h registers`.

To put (paste) a text from register a after the lines containing const, run `:g/const/pu a`.

You also don't have to always paste from the register. You can tell `put` to output your own text with `:g/const/pu ='hello vim'`.

### CMD: Print

The `:print` command (`:h :print`) prints the current line. By itself it is not too useful, but with the global command, you can print all the lines containing the given pattern.

To look at all the Javascript functions in the current buffer, you can print them with: `:g/() {/p` (assuming you are using the `myFunction() { ... }` syntax and not the arrow function).

If you want to list all the import your file has, run `:g/import/p`.

By default, the global command runs the print command if you don't pass it any command after the pattern. So instead of running `:g/foo/p`, you can just run `:g/foo`.

One cool trivia, the default global command syntax, `:g/re/p` (`re` stands for Regex), spells "grep", the very same `grep` that lives in the command-line universe. This is not a coincidence, because the `g/re/p` command originally came from the ed editor (which is probably still in your terminal right now - try it! Just remember, if you need to quit it, type `q`).

### CMD: Move

The `:move` command (`:h :move`) moves the current line to the line below the given address. Address is usually the line number. For example, to move the current line to below line 10, you can run `:m 10`.

With the global command, you can do neat trick like reversing the entire file with `:g/^/m 0`. Note that `^` is the pattern for the beginning of a line (use it to match all lines, including empty lines)

If you want to gather all your `require` lines to the top of the file, you can run `:g/require/m 0` (keep in mind that their order will be reversed).

### CMD: Copy

The copy command (`:h :t` - not sure why it's the letter t) can copy the current line into the line below the given address (similar to move, except it keeps the original line). To copy the current line to below line 10, run `:t 10`.

To copy `const` to the line below them, run `:g/const/t .` - note that `.`represents the current line. This effectively duplicates all lines containing const.

## Range in The Global Command

There are two range types that you can pass to the global command: address range and the pattern range.

### Address Range

The address range has a syntax of: `:n,mg/pattern/cmd` (`n` and `m` are the starting and ending addresses)

To delete the lines containing "puts" between lines 1 and 5, run `:1,5g/puts/d`(anything outside lines 1-5 aren't affected).

To delete the lines containing "import" between the current line to the end of the file, run `:.,$g/import/d`.

To delete all lines containing "require" between the 2 lines above and below the current line, run `:-2,+2g/require/d`.

### Pattern Range

Up to now, we have been passing one pattern to the global command: `:g/pattern/command`.

To pass two patterns, pass it another one: `:g/pattern1/,/pattern2/command`. When given two patterns, the global command will take effect only _between_the two pattern matches.

To delete everything between the line containing foo and the line containing bar, including the lines containing foo and bar themselves, run: `:g/foo/,/bar/d`. Anything outside of the foo-bar sandwich will not be deleted (ie: any line before foo and any line after bar).

The command above deletes all lines sandwiched between foo and bar _including_ the foo and bar lines; if you want to preserve the foo and bar lines, run `:g/foo/+1,/bar/-1d`. `/foo/+1` tells the global command to run the command one line _after_ foo; retrospectively, `/bar/-1` executes the command one line _before_ bar.

### CMD: Sort

Vim has a `:sort` command (`:h :sort`) to sort lines. I find the sort command useful combined with the pattern range of the global command.

If you just run the `:sort` command, Vim sorts the entire file. Often this is not the effect you want. What usually happens is that there would be times when I need to sort the content inside an array or object/hash/dict (I'll refer this data structure `{foo: bar}` as object from now on in this article) that goes over a few lines and I'd like to sort them. 

To sort only the elements inside each array that spans over several lines, run `:g/\[/+1,/\]/-1sort`.  

```
arr1 = [
  "i",
  "g",
  "e",
  "c",
  "a",
  "h",
  "b",
  "f",
  "d"
]

arr2 = [
  "b",
  "d",
  "c",
  "a",
  "e"
]
```

The result:  

```
arr1 = [
  "a",
  "b",
  "c",
  "d"
  "e",
  "f",
  "g",
  "h",
  "i",
]

arr2 = [
  "a",
  "b",
  "c",
  "d",
  "e"
]
```

The command `:g/\[/+1,/\]/-1sort` looks intimidating with all these symbols, let's break it down.

When you look at the command `:g/\[/+1,/\]/-1sort`, it follows the pattern `:g/pattern1/,/pattern2/cmd`. It starts with the first pattern, followed by the second pattern, followed by a command.

The first pattern is `/\[/+1`. The pattern is enclosed inside a pair of forward-slashes `/`. The pattern itself is `\[`, which is an opening square bracket `[`. Note that in regex, `[` is a special character for [character class](https://www.regular-expressions.info/charclass.html), so we can't just do `/[/`. We have to escape it to match a literal `[`. That's why we have `/\[/`. `+1` matches one line after the square bracket. We don't want to sort that line containing `[` itself, but the line _after_ that, that's why we add the `+1`modifier.

The second pattern is `/\]/-`. It is similar to the first pattern. I won't break it down here, but basically it matches one line before the closing square bracket `]`.

The `/\[/+1,/\]/-1` then captures anything one line after `[` and one line before `]`.

Finally, the last part of our command `:g/\[/+1,/\]/-1sort` is the `sort`command itself. Any line one line after `[` and one line before `]` will be sorted. Hey, that was not too bad!

Let's try another one. Can you sort only the keys in the array of objects below? I'll give the solution down below, but spend a few minutes attempting it yourself before scrolling down!  

```
[
  {
    one: 1,
    two: 2,
    three: 3,
    four: 4
  },
  {
    def: 2,
    abc: 1,
    jkl: 4,
    ghi: 3,
  }
]
```

My command is `:g/{/+1,/}/-1 sort`. Does yours work?

Result:  

```
[
  {
    four: 4
    one: 1,
    three: 3,
    two: 2,
  },
  {
    abc: 1,
    def: 2,
    ghi: 3,
    jkl: 4,
  }
]
```

## Conclusion

Vim's global command augments Vim's Ex commands so that you can apply them on multiple lines. With this, you only need to run a command once and Vim will do the rest for you. Why run a command multiple times if you can just do it once?