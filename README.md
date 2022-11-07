# Bash

## Introduction

The best way to describe what bash is comes straight from it's [official site](https://www.gnu.org/software/bash/)

> Bash is the GNU Project's shell—the Bourne Again SHell. This is an sh-compatible shell that incorporates useful features from the Korn shell (ksh) and the C shell (csh). It is intended to conform to the IEEE POSIX P1003.2/ISO 9945.2 Shell and Tools standard. It offers functional improvements over sh for both programming and interactive use. In addition, most sh scripts can be run by Bash without modification.

Basically, it is an improved version of the standard `sh` shell. Don't know what a shell is? That's alright. It is just a program that allows a user to interface more directly with the underlying operating system of a computer.

Shells are essential for the work of computer scientists. They allow for a fine-grain control of a computer and a high level of customization and automation. Today we will go over just some of the most basic features of bash to give you an understanding of the sort of things that are possible with this amazing and powerful tool.

## Prerequisite

Before getting into the lesson it would be a good idea to discuss some of the vocabulary and concepts that will be used. You can think in terms of layers. The computer itself is what does things, but to interface with that the user needs a shell, which is a just a program that knows how to talk to the computer on a lower level. To use a shell through a GUI, one must have a terminal, which is just another program that lets the user see the shell.

Another topic is SSH, which stands for Secure SHell. This is a protocol that allows a user to connect to a remote server (just a computer listening for connections) through a shell. This is done in a few ways, but no matter what, it involves a client (that's you) interacting with a server (the remote computer).

Now that we have that out of the way, let's get started.

## Installation

There are many ways to access a bash shell. 

- Windows
	- [Git for Windows](https://gitforwindows.org/) provides a bash emulation (and comes with git, another essential tool).
	- [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install) provides a fuller Unix-like experience on Windows.
- Mac
	- Run `chsh -s /bin/bash` in your terminal and restart it (not tested).
- Any
	- SSH allows a user to connect to a remote computer and interact with it through a shell. This is how CS2 and CS3 connect to Kent's servers Wasp and Hornet. There are many ways to SSH into a server.
		- [PuTTY](https://www.putty.org/) is a rather outdated, but fully featured, dedicated SSH client.
		- Most shells have an SSH client as well, but if you are in a shell it's probably bash.

For consistency, we will ssh into `wasp.cs.kent.edu` for this lesson, but any of the above methods should work; bash is bash. Every student should have an account. The credentials are the same as your Flashline credentials.

Type `ssh YOUR_USERNAME@wasp.cs.kent.edu` into your terminal and hit enter. You might be prompted about fingerprints, type `yes` and hit enter. When prompted, enter your password and hit enter.

Once connected and logged in, you will be able to navigate around the computer that you are connected to remotely.

## Basics

If you are used to the graphical user interfaces, you might feel like you are in over your head seeing this blinking cursor in front of you on a black background. Trust me, it is not too hard to learn how to navigate effectively with some practice. 

Once you are logged in, you will be placed in your `home` directory (another word for folder). This is a directory that is just for you. Anything you do on a shared machine like this will be done within your `home` directory and its subdirectories. You can think of this directory structure like a tree. There is a root directory `/`, and it has branches like a real tree does (typically folders such as `/home`, `/etc`, `/var`, etc.).

To see where you are in the tree, type `pwd` and hit enter. `pwd` stands for "(p)rint (w)orking (d)irectory". Your "working directory" is just the directory that you are currently in. It should print `/users/kent/YOUR_USERNAME`. If you were to create a new directory (which we will), it would create it inside the directory you are currently in.

To make a new directory, use the `mkdir` command. `mkdir` stands for "(m)a(k)e (dir)ectory". Type `mkdir DIRNAME` where `DIRNAME` is what you want to name the directory. For simplicity, let's use `testdir`. Hit enter, and it might look like nothing happened. That is good, because it means nothing went wrong.

To see the new directory we just made, use the `ls` command, which stands for "(l)i(s)t". You should get some output for this one. `testdir` should show up in blue to signal that it is a directory. Great!

To change our current directory to the new one we just made we can use the `cd` command, which stands for "(c)hange (d)irectory". Type `cd DIRNAME` where `DIRNAME` is `testdir` to move to the new directory. Type `pwd` to verify that you are now in the new directory. It should print `/users/kent/YOUR_USERNAME/testdir`.

Finally, if we want to make a file, we can use the `touch FILENAME` command. Let's run that and make a file named `testfile`. If you type `ls`, you should see that file now. To remove this file, we can use the `rm FILENAME` command. Try to remove `testfile`. List the files again afterwards to make sure that it is gone.

Awesome! Now you have the basics of navigating around and doing basic directory and file manipulation. These commands that you just learned are not as simple as they seem. Commands often have arguments or options that can be passed to them to change their behavior. For example, run `ls -la /` to see the (a)ll contents of the root (/) directory in (l)ist form. To learn more about a command like `ls` or `rm`, run `man COMMAND` to open a (man)ual page about that command, featuring all sorts of useful information. Press `q` to exit the manual.

## Tools

There are some useful tools that are available to you in bash. `echo` is a command that takes a string and prints it to the standard output. `cat` is a command that takes a file and prints its contents to the standard output. If not redirected somehow, the standard output will just show up in the shell.

One useful tool is called `grep`, which stands for (g)lobal (r)egular (e)xpression (p)rint. It is a program that takes in lines from a file or the standard input, searches for a pattern, and prints the lines that match to the standard output.

The standard input is a way to feed data into a program. There are a few ways that you can accomplish this, but one of the most useful is the pipe operator `|`. You can use this to string commands, feeding the output of one command into another.

Let's give this a try. First, we want to put some text into a file, let's do that with `echo` and a redirection operator `>`, which is a shell built-in that writes from the standard input to a file.

```
echo 'this is a test
another test
not
still not
one more test' > test.txt
```

We can check the contents of a file with the `cat` command short for con(cat)enate. It is intended to be used as a way to chain (concatenate) streams together, but it is often just used as a way to view the contents of a file.

There are many tools for viewing the contents of files (`less`, `more`, etc.), but because in this case we want to redirect it right to another program we can just use `cat`. 

The command `cat test.txt` will print the contents of `test.txt` to the standard output. You know what that means! We can pipe it into another program, like `grep`! Let's try to find every line that contains the word 'test'. To do this we can type `cat test.txt | grep 'test'`. The output should be these three lines.

```
this is a test
another test
one more test
```

The really cool thing about grep is that if you know [regular expressions](https://regexr.com/) you can use those to your advantage to have more control over your search. For instance, if you only want the lines that contain two words with a single space between them you can use `cat test.txt | grep '^\w\+ \w\+$'` which would of course print these two lines:

```
another test
still not
```

Regular expressions comes in super hand when scripting because they allow you to extract specific information from command output. One real-world example of this is a bash function (we will get to those later) I wrote to look through lines of code and search only those that I modified last. It uses another tool called `perl`, which is also good for command line parsing with regexes.

```
# Search My Lines: grep for a pattern in files that you have changed in this branch
sml() {
    if [ ! -z "$(gbranch)" ]; then
        export GIT_USERNAME=$(git config user.name);
        DIFF_FILES=($(gdiff | xargs));
        GIT_ROOT=$(groot)
        OLD_GREP_CONTEXT_SIZE=$GREP_CONTEXT_SIZE;
        GREP_CONTEXT_SIZE=0;
        for DIFF_FILE in ${DIFF_FILES[@]}; do
            git blame --line-porcelain $DIFF_FILE | perl -0777 -pe 's/[[:xdigit:]]{40} [[:digit:]]+ ([[:digit:]]+)(?: [[:digit:]]+)?\nauthor (.*)\n(?:.*\n){8,9}filename (.*)\n(?:\t| {8})(.*)/$2:$3:$1: $4/g' | sed -n "s|$GIT_USERNAME:| $GIT_ROOT/|p" | sgrep "$*"
        done;
        GREP_CONTEXT_SIZE=$OLD_GREP_CONTEXT_SIZE;
    else
        return 1;
    fi
}
```

This may look like magic, and that is ok. It is just an example of how tools like `grep` or `perl` are used.

Speaking of writing code, you can do that on the command line as well. There exist many text editors for the command line. Some of the most popular are `vim`, `emacs`, and `nano`.

`nano` is a very simple text editor for basic use. It does not have key-bindings to memorize, but its simplicity comes at a cost; when you want to do more complicated things, they may be out of `nano`'s reach. This is when you would naturally transition to `vim` or `emacs` (picking a side in the editor wars while your at it).

`vim` and `emacs` are both advanced text editors with very high customizability, control, and availability. I have less experience with `emacs`, so this next part will be about `vim`.

Let's edit our file that we made. We can do this by typing `vim test.txt`. This will start `vim` and load the file into the buffer. `vim` is what's called a 'multi-modal' editor, which means that the keys do different things based on the mode you are in. When you first start `vim`, you are put into 'normal mode' where shortcuts work and you can type vim commands. For example, to jump to the end of a file, type `G`. To go back to the start type `gg`. To move forward one word, type `w`. Great, those are called 'verbs'. They represent an action you can do. Most verbs can be prefixed with a number of times to repeat it. Try to type `gg` to go back to the start of the file, and then type `3w`. This should bring you to the start of the word 'test' on the first line. 

To actually edit the text, type `i` to put yourself into '(i)nsert mode'. From here you can make any changes to the file you want as you would expect. To exit insert mode, press `escape`. Now here comes some real computer scientist secret knowledge that will officially endow you the title of 'computer person': how to exit vim.

To save and quit we use commands, which are a vim feature that allow you to run functions that are not necessarily bound to keys. To (w)rite the file, type `:w`. And to quit vim, type `:q`. You can also chain these together as `:wq` to write and quit in one step. There are so many more features to `vim` (and probably `emacs` as well), so if you are curious be sure to research. Vim has a built-in tutorial `vimtutor` to teach the basics. The rest is up to reading the documentation and Stack Overflow.

Whew, that was a lot. But now that we know all of these awesome tools, we can start using them to our advantage in a way that is truly unique to the shell: scripting.

## Scripting

One of the main advantage to using the shell is that you can script tasks that you perform often. This is the main reason that I love using bash. We will cover 3 features that are unique to the shell that you can use to make your work a lot easier.

### Environment variables

Bash can store variables for later use. Just like in a C++ or python program, bash can store commonly used values in a variable so that you can type the variable later and have it evaluate to its assigned value.

Bash variables are typically UPPER CASE. They can be set using `VARIABLE_NAME="VARIABLE_VALUE"`. Make sure you don't use spaces. If running this from inside a script, make sure to prefix it with `export` to make it persist outside of the script.

Speaking of scripts, there is one script that runs every time you log in to a shell: `.bashrc` in your home directory. If you want something to run every time you log in, you put it in here. The lines in this file will be run as if you typed them out yourself in bash.

One use that I found for environment variables is saving URLs. If you are in CSII or CSIII, you have to use SVN to submit your work. It can be a real pain to type out `https://svn.cs.kent.edu/courses/course_number/svn/your_username` each time, so why don't we export that as an environment variable? To do this, navigate to your home directory by typing `cd ~` or just `cd`. Once there, open your `.bashrc` file in the text editor of your choice. Then, at the end of that file, add a line similar to

```
export CS3SVN="https://svn.cs.kent.edu/courses/cs44001/svn/jraber11"
```

changing the variable's name and the URL accordingly. Now if you type `source .bashrc` that variable should be available to you!

This is nice and all, but how can we access that variable? Well, to access bash variables you must type the variable's name preceded by a `$`, so in my case it would be `$CS3SVN`. Bash will see this and say "Hey, I have a variable with that name, let me replace it with that variable's value!". This is known as [parameter expansion](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html) and it has a bunch of rules and nuances to it that we won't go over.

To make sure that the variable really is available to us, let's try to print it to the standard output. We can do this by typing `echo "$CS3SVN"`. After performing the expansion, bash will see this as `echo "https://svn.cs.kent.edu/courses/cs44001/svn/jraber11"` and print that accordingly. This can be used in commands, like `svn checkout` or `svn mkdir` as in `svn mkdir $CS3SVN/example_project`.

### Aliases

The command we typed earlier to reload our `.bashrc` is a command that will be run a lot if you decide to get into scripting, so it is a good candidate for the second feature we will be covering: aliases! Aliases are similar to environment variables in that they map one string to another, but they have a different purpose and different usage. Aliases used for shortening commonly-used commands and do not require a `$` to expand. 

For example, to bind `source ~/.bashrc` to the alias `rebash`, you could add the following line to your `.bashrc`:

```
alias rebash="source ~/.bashrc"
```

And now if we run `source .bashrc` again, we should have that alias available to us! Typing `rebash` should run that command for you now. This is also good for adding default command line arguments to commands. A popular set of aliases are 

```
alias ll="ls -la"
alias la="ls -a"
```

which do what we demonstrated earlier: listing all files as a long list.

### Functions

The final and most powerful feature is bash functions. These allow you to chain commands, take arguments to change behavior, and return values. A simple example would be a function that allows you to make a directory and instantly change your current directory to it. This is often called `mkcd` for `make and change directory`. It looks like this:

```
mkcd() {
	mkdir -p "$1" && cd "$1";
}
```

This function will first call `mkdir` with the `-p` flag, which just tells it to make whatever directories it has to to get to the final one, and the directory we pass it is `"$1"`, which expands to the first argument we pass it. Then, the `&&` signifies that the next command should only run if the current one ran successfully. The next command is just to `cd` to the given directory. So in use, it would look like `mkcd some/new/directory`. 

Another useful function that I created during CS3 is `comp`. It will take in a list of files, and compile them to an executable. For example, `comp apples.cpp` would produce an executable `apples` file in the same directory. The function looks like this:

```
comp() {
	IFS='.';
	read -a file1 <<< "$1";
	clear && clang++ -std=c++11 "$@" -o "file1" && ./"$file1"
}
```

The first two lines are for getting rid of the file extensions. The third line is where the real magic happens. It will clear your screen, compile the source files, and then run the executable. This will save you a lot of time typing this chain of commands yourself. The `$@` expands to a list of all the arguments given, so this supports multiple files! 

There is so much more to bash than can be covered in a one hour lesson, but it is definitely a skillset worth knowing. I encourage you to do your own research on it. When working with bash, if you ever find yourself thinking "I wonder if I could do blah", there is a good chance that the answer is "yes". 