# GREP  

[The Treacherous Optimization](https://ridiculousfish.com/blog/posts/old-age-and-treachery.html)  

(From : https://stackoverflow.com/questions/7136899/how-does-grep-work/7137032)  

As a reference here is the UNIX man page for grep: compute.cnr.berkeley.edu/cgi-bin/man-cgi?grep Here is the FreeBSD version: freebsd.org/cgi/man.cgi?query=grep and here is the Linux version: linux.die.net/man/1/grep – Devin M Aug 21 '11 at 7:11

The power of grep is the magic of automata theory. GREP is an abbreviation for Global Regular Expression Print. And it works by constructing an automaton (a very simple "virtual machine": not Turing Complete); it then "executes" the automaton against the input stream.

The automaton is a graph or network of nodes or states. The transition between states is determined by the input character under scrutiny. Special automatons like + and * work by having transitions that loop back to themselves. Character classes like [a-z] are represented by a fan: one start node with branches for each character out to the "spokes"; and usually the spokes have a special "epsilon transition" to a single final state so it can be linked up with the next automaton to be built from the regular expression (the search string). The epsilon transitions allow a change of state without moving forward in the string being searched.

Edit: It appears I didn't read the question very closely.

When you type a command-line, it is first pre-processed by the shell. The shell performs alias substitutions and filename globbing. After substituting aliases (they're like macros), the shell chops up the command-line into a list of arguments (space-delimited). This argument list is passed to the main() function of the executable command program as an integer count (often called argc) and a pointer to a NULL-terminated ((void *)0) array of nul-terminated ('\0') char arrays.

Individual commands make use of their arguments however they wish. But most Unix programs will print a friendly help message if given the -h argument (since it begins with a minus-sign, it's called an option). GNU software will also accept a "long-form" option --help.

Since there are a great many differences between different versions of Unix programs the most reliable way to discover the exact syntax that a program requires is to ask the program itself. If that doesn't tell you what you need (or it's too cryptic to understand), you should next check the local manpage (man grep). And for gnu software you can often get even more info from info grep.



