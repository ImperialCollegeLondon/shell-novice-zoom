---
title: "Pipes and Filters - Part 1"
teaching: 25
exercises: 10
questions:
- "How can I combine existing commands to do new things?"
objectives:
- "Redirect a command's output to a file."
- "Process a file instead of keyboard input using redirection."
- "Construct command pipelines with two or more stages."
- "Explain what usually happens if a program or pipeline isn't given any input to process."
- "Explain Unix's 'small pieces, loosely joined' philosophy."
keypoints:
- "`cat` displays the contents of its inputs."
- "`head` displays the first 10 lines of its input."
- "`tail` displays the last 10 lines of its input."
- "`sort` sorts its inputs."
- "`wc` counts lines, words, and characters in its inputs."
- "`command > file` redirects a command's output to a file (overwriting any existing content)."
- "`command >> file` appends a command's output to a file."
- "`first | second` is a pipeline: the output of the first command is used as the input to the second."
- "The best way to use the shell is to use pipes to combine simple single-purpose programs (filters)."
---

Now that we know a few basic commands,
we can finally look at the shell's most powerful feature:
the ease with which it lets us combine existing programs in new ways.
We'll start with the directory called `data-shell/molecules`
that contains six files describing some simple organic molecules.
The `.pdb` extension indicates that these files are in Protein Data Bank format,
a simple text format that specifies the type and position of each atom in the molecule.

~~~
$ ls molecules
~~~
{: .language-bash}

~~~
cubane.pdb    ethane.pdb    methane.pdb
octane.pdb    pentane.pdb   propane.pdb
~~~
{: .output}

Let's go into that directory with `cd` and run the command `wc cubane.pdb`:

~~~
$ cd molecules
$ wc cubane.pdb 
~~~
{: .language-bash}

~~~
20  156 1158 cubane.pdb
~~~
{: .output}

`wc` is the 'word count' command:
it counts the number of lines, words, and characters in files (from left to right, in that order).

If we run the command `wc *.pdb`, the `*` in `*.pdb` matches zero or more characters,
so the shell turns `*.pdb` into a list of all `.pdb` files in the current directory:

~~~
$ wc *.pdb
~~~
{: .language-bash}

~~~
  20  156  1158  cubane.pdb
  12  84   622   ethane.pdb
   9  57   422   methane.pdb
  30  246  1828  octane.pdb
  21  165  1226  pentane.pdb
  15  111  825   propane.pdb
 107  819  6081  total
~~~
{: .output}

Note that `wc *.pdb` also shows the total number of all lines in the last line of the output.

If we run `wc -l` instead of just `wc`,
the output shows only the number of lines per file:

~~~
$ wc -l *.pdb
~~~
{: .language-bash}

~~~
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
~~~
{: .output}

The `-m` and `-w` options can also be used with the `wc` command, to show 
only the number of characters or the number of words in the files.

> ## Why Isn't It Doing Anything?
>
> What happens if a command is supposed to process a file, but we
> don't give it a filename? For example, what if we type:
>
> ~~~
> $ wc -l
> ~~~
> {: .language-bash}
>
> but don't type `*.pdb` (or anything else) after the command?
> Since it doesn't have any filenames, `wc` assumes it is supposed to
> process input given at the command prompt, so it just sits there and waits for us to give
> it some data interactively. From the outside, though, all we see is it
> sitting there: the command doesn't appear to do anything.
>
> If you make this kind of mistake, you can escape out of this state by holding down
> the control key (<kbd>Ctrl</kbd>) and typing the letter <kbd>C</kbd> once and letting go of the <kbd>Ctrl</kbd> key.
> <kbd>Ctrl</kbd>+<kbd>C</kbd>
{: .callout}

Which of these files contains the fewest lines?
It's an easy question to answer when there are only six files,
but what if there were 6000?
Our first step toward a solution is to run the command:

~~~
$ wc -l *.pdb > lengths.txt
~~~
{: .language-bash}

The greater than symbol, `>`, tells the shell to **redirect** the command's output
to a file instead of printing it to the screen. (This is why there is no screen output:
everything that `wc` would have printed has gone into the
file `lengths.txt` instead.)  The shell will create
the file if it doesn't exist. If the file exists, it will be
silently overwritten, which may lead to data loss and thus requires
some caution.
`ls lengths.txt` confirms that the file exists:

~~~
$ ls lengths.txt
~~~
{: .language-bash}

~~~
lengths.txt
~~~
{: .output}

We can now send the content of `lengths.txt` to the screen using `cat lengths.txt`.
The `cat` command gets its name from 'concatenate' i.e. join together,
and it prints the contents of files one after another.
There's only one file in this case,
so `cat` just shows us what it contains:

~~~
$ cat lengths.txt
~~~
{: .language-bash}

~~~
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
~~~
{: .output}

> ## Output Page by Page
>
> We'll continue to use `cat` in this lesson, for convenience and consistency,
> but it has the disadvantage that it always dumps the whole file onto your screen.
> More useful in practice is the command `less`,
> which you use with `less lengths.txt`.
> This displays a screenful of the file, and then stops.
> You can go forward one screenful by pressing the spacebar,
> or back one by pressing `b`.  Press `q` to quit.
{: .callout}

Now let's use the `sort` command to sort its contents.

> ## What Does `sort -n` Do?
>
> If we run `sort` on a file containing the following lines:
>
> ~~~
> 10
> 2
> 19
> 22
> 6
> ~~~
> {: .source}
>
> the output is:
>
> ~~~
> 10
> 19
> 2
> 22
> 6
> ~~~
> {: .output}
>
> If we run `sort -n` on the same input, we get this instead:
>
> ~~~
> 2
> 6
> 10
> 19
> 22
> ~~~
> {: .output}
>
> Explain why `-n` has this effect.
>
> > ## Solution
> > The `-n` option specifies a numerical rather than an alphanumerical sort.
> {: .solution}
{: .challenge}

We will also use the `-n` option to specify that the sort is
numerical instead of alphanumerical.
This does *not* change the file;
instead, it sends the sorted result to the screen:

~~~
$ sort -n lengths.txt
~~~
{: .language-bash}

~~~
  9  methane.pdb
 12  ethane.pdb
 15  propane.pdb
 20  cubane.pdb
 21  pentane.pdb
 30  octane.pdb
107  total
~~~
{: .output}

We can put the sorted list of lines in another temporary file called `sorted-lengths.txt`
by putting `> sorted-lengths.txt` after the command,
just as we used `> lengths.txt` to put the output of `wc` into `lengths.txt`.
Once we've done that,
we can run another command called `head` to get the first few lines in `sorted-lengths.txt`:

~~~
$ sort -n lengths.txt > sorted-lengths.txt
$ head -n 1 sorted-lengths.txt
~~~
{: .language-bash}

~~~
  9  methane.pdb
~~~
{: .output}

Using `-n 1` with `head` tells it that
we only want the first line of the file;
`-n 20` would get the first 20,
and so on.
Since `sorted-lengths.txt` contains the lengths of our files ordered from least to greatest,
the output of `head` must be the file with the fewest lines.

> ## Redirecting to the same file
>
> It's a very bad idea to try redirecting
> the output of a command that operates on a file
> to the same file. For example:
>
> ~~~
> $ sort -n lengths.txt > lengths.txt
> ~~~
> {: .language-bash}
>
> Doing something like this may give you
> incorrect results and/or delete
> the contents of `lengths.txt`.
{: .callout}

> ## What Does `>>` Mean?
>
> We have seen the use of `>`, but there is a similar operator `>>` which works slightly differently.
> We'll learn about the differences between these two operators by printing some strings.
> We can use the `echo` command to print strings e.g.
>
> ~~~
> $ echo The echo command prints text
> ~~~
> {: .language-bash}
> ~~~
> The echo command prints text
> ~~~
> {: .output}
>
> Now test the commands below to reveal the difference between the two operators:
>
> ~~~
> $ echo hello > testfile01.txt
> ~~~
> {: .language-bash}
>
> and:
>
> ~~~
> $ echo hello >> testfile02.txt
> ~~~
> {: .language-bash}
>
> Hint: Try executing each command twice in a row and then examining the output files.
>
> > ## Solution
> > In the first example with `>`, the string 'hello' is written to `testfile01.txt`,
> > but the file gets overwritten each time we run the command.
> >
> > We see from the second example that the `>>` operator also writes 'hello' to a file
> > (in this case`testfile02.txt`),
> > but appends the string to the file if it already exists (i.e. when we run it for the second time).
> {: .solution}
{: .challenge}

> ## Appending Data
>
> We have already met the `head` command, which prints lines from the start of a file.
> `tail` is similar, but prints lines from the end of a file instead.
>
> Consider the file `data-shell/data/animals.txt`.
> After these commands, select the answer that
> corresponds to the file `animals-subset.txt`:
>
> ~~~
> $ head -n 3 animals.txt > animals-subset.txt
> $ tail -n 2 animals.txt >> animals-subset.txt
> ~~~
> {: .language-bash}
>
> 1. The first three lines of `animals.txt`
> 2. The last two lines of `animals.txt`
> 3. The first three lines and the last two lines of `animals.txt`
> 4. The second and third lines of `animals.txt`
>
> > ## Solution
> > Option 3 is correct.
> > For option 1 to be correct we would only run the `head` command.
> > For option 2 to be correct we would only run the `tail` command.
> > For option 4 to be correct we would have to pipe the output of `head` into `tail -n 2` by doing `head -n 3 animals.txt | tail -n 2 > animals-subset.txt`
> {: .solution}
{: .challenge}
