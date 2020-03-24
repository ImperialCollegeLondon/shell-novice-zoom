---
title: "Pipes and Filters - Part 2"
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


If you think this is confusing,
you're in good company:
even once you understand what `wc`, `sort`, and `head` do,
all those intermediate files make it hard to follow what's going on.
We can make it easier to understand by running `sort` and `head` together:

~~~
$ sort -n lengths.txt | head -n 1
~~~
{: .language-bash}

~~~
  9  methane.pdb
~~~
{: .output}

The vertical bar, `|`, between the two commands is called a **pipe**.
It tells the shell that we want to use
the output of the command on the left
as the input to the command on the right.

Nothing prevents us from chaining pipes consecutively.
That is, we can for example send the output of `wc` directly to `sort`,
and then the resulting output to `head`.
Thus we first use a pipe to send the output of `wc` to `sort`:

~~~
$ wc -l *.pdb | sort -n
~~~
{: .language-bash}

~~~
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
~~~
{: .output}

And now we send the output of this pipe, through another pipe, to `head`, so that the full pipeline becomes:

~~~
$ wc -l *.pdb | sort -n | head -n 1
~~~
{: .language-bash}

~~~
   9  methane.pdb
~~~
{: .output}

This is exactly like a mathematician nesting functions like *log(3x)*
and saying 'the log of three times *x*'.
In our case,
the calculation is 'head of sort of line count of `*.pdb`'.


The redirection and pipes used in the last few commands are illustrated below:

![Redirects and Pipes](../fig/redirects-and-pipes.png)

> ## Piping Commands Together
>
> In our current directory, we want to find the 3 files which have the least number of
> lines. Which command listed below would work?
>
> 1. `wc -l * > sort -n > head -n 3`
> 2. `wc -l * | sort -n | head -n 1-3`
> 3. `wc -l * | head -n 3 | sort -n`
> 4. `wc -l * | sort -n | head -n 3`
>
> > ## Solution
> > Option 4 is the solution.
> > The pipe character `|` is used to connect the output from one command to
> > the input of another.
> > `>` is used to redirect standard output to a file.
> > Try it in the `data-shell/molecules` directory!
> {: .solution}
{: .challenge}


This idea of linking programs together is why Unix has been so successful.
Instead of creating enormous programs that try to do many different things,
Unix programmers focus on creating lots of simple tools that each do one job well,
and that work well with each other.
This programming model is called 'pipes and filters'.
We've already seen pipes;
a **filter** is a program like `wc` or `sort`
that transforms a stream of input into a stream of output.
Almost all of the standard Unix tools can work this way:
unless told to do otherwise,
they read from standard input,
do something with what they've read,
and write to standard output.

The key is that any program that reads lines of text from standard input
and writes lines of text to standard output
can be combined with every other program that behaves this way as well.
You can *and should* write your programs this way
so that you and other people can put those programs into pipes to multiply their power.


> ## Pipe Reading Comprehension
>
> A file called `animals.txt` (in the `data-shell/data` folder) contains the following data:
>
> ~~~
> 2012-11-05,deer
> 2012-11-05,rabbit
> 2012-11-05,raccoon
> 2012-11-06,rabbit
> 2012-11-06,deer
> 2012-11-06,fox
> 2012-11-07,rabbit
> 2012-11-07,bear
> ~~~
> {: .source}
>
> What text passes through each of the pipes and the final redirect in the pipeline below?
>
> ~~~
> $ cat animals.txt | head -n 5 | tail -n 3 | sort -r > final.txt
> ~~~
> {: .language-bash}
> Hint: build the pipeline up one command at a time to test your understanding
> > ## Solution
> > The `head` command extracts the first 5 lines from `animals.txt`.
> > Then, the last 3 lines are extracted from the previous 5 by using the `tail` command.
> > With the `sort -r` command those 3 lines are sorted in reverse order and finally,
> > the output is redirected to a file `final.txt`.
> > The content of this file can be checked by executing `cat final.txt`.
> > The file should contain the following lines:
> > ```
> > 2012-11-06,rabbit
> > 2012-11-06,deer
> > 2012-11-05,raccoon
> > ```
> > {: .source}
> {: .solution}
{: .challenge}

> ## Pipe Construction
>
> For the file `animals.txt` from the previous exercise, consider the following command:
>
> ~~~
> $ cut -d , -f 2 animals.txt
> ~~~
> {: .language-bash}
>
> The `cut` command is used to remove or 'cut out' certain sections of each line in the file,
> and `cut` expects the lines to be separated into columns by a <kbd>Tab</kbd> character.
> A character used in this way is a called a **delimiter**.
> In the example above we use the `-d` option to specify the comma as our delimiter character.
> We have also used the `-f` option to specify that we want to extract the second field (column).
> This gives the following output:
>
> ~~~
> deer
> rabbit
> raccoon
> rabbit
> deer
> fox
> rabbit
> bear
> ~~~
> {: .output}
>
> The `uniq` command filters out adjacent matching lines in a file.
> How could you extend this pipeline (using `uniq` and another command) to find
> out what animals the file contains (without any duplicates in their
> names)?
>
> > ## Solution
> > ```
> > $ cut -d , -f 2 animals.txt | sort | uniq
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## Which Pipe?
>
> The file `animals.txt` contains 8 lines of data formatted as follows:
>
> ~~~
> 2012-11-05,deer
> 2012-11-05,rabbit
> 2012-11-05,raccoon
> 2012-11-06,rabbit
> ...
> ~~~
> {: .output}
>
> The `uniq` command has a `-c` option which gives a count of the
> number of times a line occurs in its input.  Assuming your current
> directory is `data-shell/data/`, what command would you use to produce
> a table that shows the total count of each type of animal in the file?
>
> 1.  `sort animals.txt | uniq -c`
> 2.  `sort -t, -k2,2 animals.txt | uniq -c`
> 3.  `cut -d, -f 2 animals.txt | uniq -c`
> 4.  `cut -d, -f 2 animals.txt | sort | uniq -c`
> 5.  `cut -d, -f 2 animals.txt | sort | uniq -c | wc -l`
>
> > ## Solution
> > Option 4. is the correct answer.
> > If you have difficulty understanding why, try running the commands, or sub-sections of
> > the pipelines (make sure you are in the `data-shell/data` directory).
> {: .solution}
{: .challenge}

## Nelle's Pipeline: Checking Files

Nelle has run her samples through the assay machines
and created 17 files in the `north-pacific-gyre/2012-07-03` directory described earlier.
As a quick sanity check, starting from her home directory, Nelle types:

~~~
$ cd north-pacific-gyre/2012-07-03
$ wc -l *.txt
~~~
{: .language-bash}

The output is 18 lines that look like this:

~~~
300 NENE01729A.txt
300 NENE01729B.txt
300 NENE01736A.txt
300 NENE01751A.txt
300 NENE01751B.txt
300 NENE01812A.txt
... ...
~~~
{: .output}

Now she types this:

~~~
$ wc -l *.txt | sort -n | head -n 5
~~~
{: .language-bash}

~~~
 240 NENE02018B.txt
 300 NENE01729A.txt
 300 NENE01729B.txt
 300 NENE01736A.txt
 300 NENE01751A.txt
~~~
{: .output}

Whoops: one of the files is 60 lines shorter than the others.
When she goes back and checks it,
she sees that she did that assay at 8:00 on a Monday morning --- someone
was probably in using the machine on the weekend,
and she forgot to reset it.
Before re-running that sample,
she checks to see if any files have too much data:

~~~
$ wc -l *.txt | sort -n | tail -n 5
~~~
{: .language-bash}

~~~
 300 NENE02040B.txt
 300 NENE02040Z.txt
 300 NENE02043A.txt
 300 NENE02043B.txt
5040 total
~~~
{: .output}

Those numbers look good --- but what's that 'Z' doing there in the third-to-last line?
All of her samples should be marked 'A' or 'B';
by convention,
her lab uses 'Z' to indicate samples with missing information.
To find others like it, she does this:

~~~
$ ls *Z.txt
~~~
{: .language-bash}

~~~
NENE01971Z.txt    NENE02040Z.txt
~~~
{: .output}

Sure enough,
when she checks the log on her laptop,
there's no depth recorded for either of those samples.
Since it's too late to get the information any other way,
she must exclude those two files from her analysis.
She could delete them using `rm`,
but there are actually some analyses she might do later where depth doesn't matter,
so instead, she'll have to be careful later on to select files using the wildcard expression `*[AB].txt`.
As always,
the `*` matches any number of characters;
the expression `[AB]` matches either an 'A' or a 'B',
so this matches all the valid data files she has.

> ## Wildcard Expressions
>
> Wildcard expressions can be very complex, but you can sometimes write
> them in ways that only use simple syntax, at the expense of being a bit
> more verbose.
> Consider the directory `data-shell/north-pacific-gyre/2012-07-03` :
> the wildcard expression `*[AB].txt`
> matches all files ending in `A.txt` or `B.txt`. Imagine you forgot about
> this.
>
> 1.  Can you match the same set of files with basic wildcard expressions
>     that do not use the `[]` syntax? *Hint*: You may need more than one
>     command, or two arguments to the `ls` command.
>
> 2.  If you used two commands, the files in your output will match the
>     same set of files in this example. What is the small difference between the
>     outputs?
>
> 3.  If you used two commands, under what circumstances would your new
>     expression produce an error message where the original one would not?
>
> > ## Solution
> > 1. A solution using two wildcard commands:
> >     ~~~
> >     $ ls *A.txt
> >     $ ls *B.txt
> >     ~~~
> >     A solution using one command but with two arguments:
> >     ~~~
> >     $ ls *A.txt *B.txt
> >     ~~~
> >     {: .language-bash}
> > 2. The output from the two new commands is separated because there are two commands.
> > 3. When there are no files ending in `A.txt`, or there are no files ending in
> > `B.txt`, then one of the two commands will fail.
> {: .solution}
{: .challenge}

> ## Removing Unneeded Files
>
> Suppose you want to delete your processed data files, and only keep
> your raw files and processing script to save storage.
> The raw files end in `.dat` and the processed files end in `.txt`.
> Which of the following would remove all the processed data files,
> and *only* the processed data files?
>
> 1. `rm ?.txt`
> 2. `rm *.txt`
> 3. `rm * .txt`
> 4. `rm *.*`
>
> > ## Solution
> > 1. This would remove `.txt` files with one-character names
> > 2. This is correct answer
> > 3. The shell would expand `*` to match everything in the current directory,
> > so the command would try to remove all matched files and an additional
> > file called `.txt`
> > 4. The shell would expand `*.*` to match all files with any extension,
> > so this command would delete all files
> {: .solution}
{: .challenge}
