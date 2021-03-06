---
title: "Working With Files and Directories - Part 1"
teaching: 15
exercises: 5
questions:
- "How can I create, copy, and delete files and directories?"
- "How can I edit files?"
objectives:
- "Create a directory hierarchy that matches a given diagram."
- "Create files in that hierarchy using an editor or by copying and renaming existing files."
- "Delete, copy and move specified files and/or directories."
keypoints:
- "`cp old new` copies a file."
- "`mkdir path` creates a new directory."
- "`mv old new` moves (renames) a file or directory."
- "`rm path` removes (deletes) a file."
- "`*` matches zero or more characters in a filename, so `*.txt` matches all files ending in `.txt`."
- "`?` matches any single character in a filename, so `?.txt` matches `a.txt` but not `any.txt`."
- "Use of the Control key may be described in many ways, including `Ctrl-X`, `Control-X`, and `^X`."
- "The shell does not have a trash bin: once something is deleted, it's really gone."
- "Most files' names are `something.extension`. The extension isn't required, and doesn't guarantee anything, but is normally used to indicate the type of data in the file."
- "Depending on the type of work you do, you may need a more powerful text editor than Nano."
---

> ## Video
>
> [https://youtu.be/ZMSUWmMhyZQ](https://youtu.be/ZMSUWmMhyZQ)
{: .callout}


## Creating directories
We now know how to explore files and directories,
but how do we create them in the first place?

### Step one: see where we are and what we already have
Let's go back to our `data-shell` directory on the Desktop
and use `ls -F` to see what it contains:

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/data-shell
~~~
{: .output}

~~~
$ ls -F
~~~
{: .language-bash}

~~~
creatures/  data/  molecules/  north-pacific-gyre/  notes.txt  pizza.cfg  solar.pdf  writing/
~~~
{: .output}

### Create a directory

Let's create a new directory called `thesis` using the command `mkdir thesis`
(which has no output):

~~~
$ mkdir thesis
~~~
{: .language-bash}

As you might guess from its name,
`mkdir` means 'make directory'.
Since `thesis` is a relative path
(i.e., does not have a leading slash, like `/what/ever/thesis`),
the new directory is created in the current working directory:

~~~
$ ls -F
~~~
{: .language-bash}

~~~
creatures/  data/  molecules/  north-pacific-gyre/  notes.txt  pizza.cfg  solar.pdf  thesis/  writing/
~~~
{: .output}

> ## Two ways of doing the same thing
> Using the shell to create a directory is no different than using a file explorer.
> If you open the current directory using your operating system's graphical file explorer,
> the `thesis` directory will appear there too.
> While the shell and the file explorer are two different ways of interacting with the files,
> the files and directories themselves are the same.
{: .callout}

> ## Good names for files and directories
>
> Complicated names of files and directories can make your life painful
> when working on the command line. Here we provide a few useful
> tips for the names of your files.
>
> 1. Don't use spaces.
>
>    Spaces can make a name more meaningful,
>    but since spaces are used to separate arguments on the command line
>    it is better to avoid them in names of files and directories.
>    You can use `-` or `_` instead (e.g. `north-pacific-gyre/` rather than `north pacific gyre/`).
>
> 2. Don't begin the name with `-` (dash).
>
>    Commands treat names starting with `-` as options.
>
> 3. Stick with letters, numbers, `.` (period or 'full stop'), `-` (dash) and `_` (underscore).
>
>    Many other characters have special meanings on the command line.
>    We will learn about some of these during this lesson.
>    There are special characters that can cause your command to not work as
>    expected and can even result in data loss.
>
> If you need to refer to names of files or directories that have spaces
> or other special characters, you should surround the name in quotes (`""`).
{: .callout}

Since we've just created the `thesis` directory, there's nothing in it yet:

~~~
$ ls -F thesis
~~~
{: .language-bash}

### Create a text file
Let's change our working directory to `thesis` using `cd`,
then run a text editor called Nano to create a file called `draft.txt`:

~~~
$ cd thesis
$ nano draft.txt
~~~
{: .language-bash}

> ## Which Editor?
>
> When we say, '`nano` is a text editor' we really do mean 'text': it can
> only work with plain character data, not tables, images, or any other
> human-friendly media. We use it in examples because it is one of the
> least complex text editors. However, because of this trait, it may
> not be powerful enough or flexible enough for the work you need to do
> after this workshop. On Unix systems (such as Linux and macOS),
> many programmers use [Emacs](http://www.gnu.org/software/emacs/) or
> [Vim](http://www.vim.org/) (both of which require more time to learn),
> or a graphical editor such as
> [Gedit](http://projects.gnome.org/gedit/). On Windows, you may wish to
> use [Notepad++](http://notepad-plus-plus.org/).  Windows also has a built-in
> editor called `notepad` that can be run from the command line in the same
> way as `nano` for the purposes of this lesson.
>
> No matter what editor you use, you will need to know where it searches
> for and saves files. If you start it from the shell, it will (probably)
> use your current working directory as its default location. If you use
> your computer's start menu, it may want to save files in your desktop or
> documents directory instead. You can change this by navigating to
> another directory the first time you 'Save As...'
{: .callout}

Let's type in a few lines of text.
Once we're happy with our text, we can press <kbd>Ctrl</kbd>+<kbd>O</kbd>
(press the <kbd>Ctrl</kbd> or <kbd>Control</kbd> key and, while
holding it down, press the <kbd>O</kbd> key) to write our data to disk
(we'll be asked what file we want to save this to:
press <kbd>Return</kbd> to accept the suggested default of `draft.txt`).

<div style="width:80%; margin: auto;"><img alt="Nano in Action" src="../fig/nano-screenshot.png"></div>

Once our file is saved, we can use <kbd>Ctrl</kbd>+<kbd>X</kbd> to quit the editor and
return to the shell.

> ## Control, Ctrl, or ^ Key
>
> The Control key is also called the 'Ctrl' key. There are various ways
> in which using the Control key may be described. For example, you may
> see an instruction to press the <kbd>Control</kbd> key and, while holding it down,
> press the <kbd>X</kbd> key, described as any of:
>
> * `Control-X`
> * `Control+X`
> * `Ctrl-X`
> * `Ctrl+X`
> * `^X`
> * `C-x`
>
> In nano, along the bottom of the screen you'll see `^G Get Help ^O WriteOut`.
> This means that you can use `Control-G` to get help and `Control-O` to save your
> file.
{: .callout}

`nano` doesn't leave any output on the screen after it exits,
but `ls` now shows that we have created a file called `draft.txt`:

~~~
$ ls
~~~
{: .language-bash}

~~~
draft.txt
~~~
{: .output}

> ## Creating Files a Different Way
>
> We have seen how to create text files using the `nano` editor.
> Now, try the following command:
>
> ~~~
> $ touch my_file.txt
> ~~~
> {: .language-bash}
>
> 1.  What did the `touch` command do?
>     When you look at your current directory using the GUI file explorer,
>     does the file show up?
>
> 2.  Use `ls -l` to inspect the files.  How large is `my_file.txt`?
>
> 3.  When might you want to create a file this way?
>
> > ## Solution
> > 1.  The `touch` command generates a new file called `my_file.txt` in
> >     your current directory.  You
> >     can observe this newly generated file by typing `ls` at the
> >     command line prompt.  `my_file.txt` can also be viewed in your
> >     GUI file explorer.
> >
> > 2.  When you inspect the file with `ls -l`, note that the size of
> >     `my_file.txt` is 0 bytes.  In other words, it contains no data.
> >     If you open `my_file.txt` using your text editor it is blank.
> >
> > 3.  Some programs do not generate output files themselves, but
> >     instead require that empty files have already been generated.
> >     When the program is run, it searches for an existing file to
> >     populate with its output.  The touch command allows you to
> >     efficiently generate a blank text file to be used by such
> >     programs.
> {: .solution}
{: .challenge}

> ## What's In A Name?
>
> You may have noticed that all of Nelle's files are named 'something dot
> something', and in this part of the lesson, we always used the extension
> `.txt`.  This is just a convention: we can call a file `mythesis` or
> almost anything else we want. However, most people use two-part names
> most of the time to help them (and their programs) tell different kinds
> of files apart. The second part of such a name is called the
> **filename extension**, and indicates
> what type of data the file holds: `.txt` signals a plain text file, `.pdf`
> indicates a PDF document, `.cfg` is a configuration file full of parameters
> for some program or other, `.png` is a PNG image, and so on.
>
> This is just a convention, albeit an important one. Files contain
> bytes: it's up to us and our programs to interpret those bytes
> according to the rules for plain text files, PDF documents, configuration
> files, images, and so on.
>
> Naming a PNG image of a whale as `whale.mp3` doesn't somehow
> magically turn it into a recording of whalesong, though it *might*
> cause the operating system to try to open it with a music player
> when someone double-clicks it.
{: .callout}

## Moving files and directories
Returning to the `data-shell` directory,

```
cd ~/Desktop/data-shell/
```
{: .language-bash}

In our `thesis` directory we have a file `draft.txt`
which isn't a particularly informative name,
so let's change the file's name using `mv`,
which is short for 'move':

~~~
$ mv thesis/draft.txt thesis/quotes.txt
~~~
{: .language-bash}

The first argument tells `mv` what we're 'moving',
while the second is where it's to go.
In this case,
we're moving `thesis/draft.txt` to `thesis/quotes.txt`,
which has the same effect as renaming the file.
Sure enough,
`ls` shows us that `thesis` now contains one file called `quotes.txt`:

~~~
$ ls thesis
~~~
{: .language-bash}

~~~
quotes.txt
~~~
{: .output}

One has to be careful when specifying the target file name, since `mv` will
silently overwrite any existing file with the same name, which could
lead to data loss. An additional option, `mv -i` (or `mv --interactive`),
can be used to make `mv` ask you for confirmation before overwriting.

Note that `mv` also works on directories.

Let's move `quotes.txt` into the current working directory.
We use `mv` once again,
but this time we'll use just the name of a directory as the second argument
to tell `mv` that we want to keep the filename,
but put the file somewhere new.
(This is why the command is called 'move'.)
In this case,
the directory name we use is the special directory name `.` that we mentioned earlier.

~~~
$ mv thesis/quotes.txt .
~~~
{: .language-bash}

The effect is to move the file from the directory it was in to the current working directory.
`ls` now shows us that `thesis` is empty:

~~~
$ ls thesis
~~~
{: .language-bash}

Further,
`ls` with a filename or directory name as an argument only lists that file or directory.
We can use this to see that `quotes.txt` is still in our current directory:

~~~
$ ls quotes.txt
~~~
{: .language-bash}

~~~
quotes.txt
~~~
{: .output}

> ## Moving Files to a new folder
>
> After running the following commands,
> Jamie realizes that she put the files `sucrose.dat` and `maltose.dat` into the wrong folder. 
> The files should have been placed in the `raw` folder.
>
> ~~~
> $ ls -F
>  analyzed/ raw/
> $ ls -F analyzed
> fructose.dat glucose.dat maltose.dat sucrose.dat
> $ cd analyzed
> ~~~
> {: .language-bash}
>
> Fill in the blanks to move these files to the `raw/` folder
> (i.e. the one she forgot to put them in) 
>
> ~~~
> $ mv sucrose.dat maltose.dat ____/____
> ~~~
> {: .language-bash}
> > ## Solution
> > ```
> > $ mv sucrose.dat maltose.dat ../raw
> > ```
> > {: .language-bash}
> > Recall that `..` refers to the parent directory (i.e. one above the current directory)
> > and that `.` refers to the current directory.
> {: .solution}
{: .challenge}

## Copying files and directories

The `cp` command works very much like `mv`,
except it copies a file instead of moving it.
We can check that it did the right thing using `ls`
with two paths as arguments --- like most Unix commands,
`ls` can be given multiple paths at once:

~~~
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .language-bash}

~~~
quotes.txt   thesis/quotations.txt
~~~
{: .output}

We can also copy a directory and all its contents by using the
[recursive](https://en.wikipedia.org/wiki/Recursion) option `-r`,
e.g. to back up a directory:

```
$ cp -r thesis thesis_backup
```
{: .language-bash}

We can check the result by listing the contents of both the `thesis` and `thesis_backup` directory:

```
$ ls thesis thesis_backup
```
{: .language-bash}

```
thesis:
quotations.txt

thesis_backup:
quotations.txt
```
{: .output}

> ## Moving and Copying
>
> What is the output of the closing `ls` command in the sequence shown below?
>
> ~~~
> $ pwd
> ~~~
> {: .language-bash}
> ~~~
> /Users/jamie/data
> ~~~
> {: .output}
> ~~~
> $ ls
> ~~~
> {: .language-bash}
> ~~~
> proteins.dat
> ~~~
> {: .output}
> ~~~
> $ mkdir recombine
> $ mv proteins.dat recombine/
> $ cp recombine/proteins.dat ../proteins-saved.dat
> $ ls
> ~~~
> {: .language-bash}
>
> 1.   `proteins-saved.dat recombine`
> 2.   `recombine`
> 3.   `proteins.dat recombine`
> 4.   `proteins-saved.dat`
>
> > ## Solution
> > We start in the `/Users/jamie/data` directory, and create a new folder called `recombine`.
> > The second line moves (`mv`) the file `proteins.dat` to the new folder (`recombine`).
> > The third line makes a copy of the file we just moved.  The tricky part here is where the file was
> > copied to.  Recall that `..` means 'go up a level', so the copied file is now in `/Users/jamie`.
> > Notice that `..` is interpreted with respect to the current working
> > directory, **not** with respect to the location of the file being copied.
> > So, the only thing that will show using ls (in `/Users/jamie/data`) is the recombine folder.
> >
> > 1. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
> > 2. Yes
> > 3. No, see explanation above.  `proteins.dat` is located at `/Users/jamie/data/recombine`
> > 4. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
> {: .solution}
{: .challenge}

