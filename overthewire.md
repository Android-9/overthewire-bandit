## Bandit
#### Level 0
In order to login to the game using SSH, you need to use the `ssh` command.

> Host: **bandit.labs.overthewire.org** <br>
> Port: 2220 <br>
> Username: **bandit0** <br>
> Password: **bandit0**

`ssh` has various optional parameters. The first parameter (mandatory) is the host.

To connect to a particular port you attach `-p portnumber` to the end.

To specify a login username, you attach `-l username` to the end.

`ssh bandit.labs.overthewire.org -p 2220 -l bandit0`

#### Level 1
Use the `ls` command to list all files in the current working directory.

You can always see the current working directory from the beginning prompt like `bandit0@bandit:~$` where the `~$` indicates the root directory.

In order to read the contexts of a text file, you can use `cat`. There are also various optional parameters you can attach but for now using just the base `cat` is sufficient.

`cat readme`

Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

> Make sure to exit out of the ssh connection via 
> `logout` and then connect again but with the username 
> `bandit1`.

#### Level 2
`-` is a special character in linux bash. Therefore if you try and read it through the usual means `cat -`, this would not work because the `-` is reserved for attaching optional parameters to commands.

The way you circumvent this is to either use redirection:

`cat < -`

Or another common approach is to specify the full location of the file using `./`:

`cat ./-`

Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

#### Level 3
Spaces are also interpreted by linux bash as extra parameters, you could say. So reading the file using `cat spaces in the filename` would not work. It would only register the first parameter `cat` but not recognize the other words that are part of the file name.

An easy fix for this is to put quote around it like this:

`cat "spaces in the filename"`

Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

#### Level 4
Hidden files are denoted with a `.` at the start of their filename which causes them to be invisible through the usual `ls` command.

However, `ls` has plenty of optional parameters, one being `-a` which makes it so that it lists all entries including ones that start with `.`.

`ls -a`

> In order to change to a different directory, use `cd 
> directoryname`.

Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

#### Level 5
To find out about the type of a file, use the `file` command.

For example, `file ./-file00` will print the output `./-file00: data` which shows that this file is in some 'data' format.

What we really want though is a human-readable file like a text file.

To display the types of each file in the current working directory, you can use:

`file ./*`

where `*` is a wildcard character that matches everything.

Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

#### Level 6
We need to find a file in the inhere directory that has all the following properties:

- human-readable
- 1033 bytes in size
- not executable

Going through the [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html) manual, we can find the optional parameters that allow us to specify the type of file we are looking for.

Human-readable is very vague but we can assume that this is referring to a regular file. We can attach the optional parameter `-type` along with what type we are looking for. In this case, `f` refers to regular files.

`-type f`

Finding a file with a particular size can be done using the `-size` option. Add a number and a suffix after to denote the particular size and measure. To find files that are exactly 1033 bytes in size, use (where `c` denotes bytes):

`-size 1033c`

Lastly, to find files that are not executable, scouring through the manual, we can find the `-executable` command. This looks for all files that are executable, however what we want is files that are **not** executable. You can use the `!` expression or `-not` to find the opposite.

> The `!` usually will need protection from interpretation by the shell
> so it is better to escape it using `\!`

`\! -executable`

Therefore, the full line to find the file we are looking for is:

`find -type f -size 1033c \! -executable`

Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

#### Level 7
We need to find a file that has all the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

As per usual, we must use the `find` command and to make sure this looks everywhere, you can use `find /*` which searches everything from the root directory.

Two new optional parameters must be attached; `-group groupname` and `-user username` which looks for anything that is owned by `groupname` and `username` respectively.

The other necessary components have already been covered in the previous level. Thus the full line to find the file we are looking for is:

`find /* -type f -size 33c -group bandit6 -user bandit7 2>/dev/null`

> The `2>/dev/null` here removes all error messages as
> if you tried to run this line without it, there would
> be lots of 'Permission Denied' messages for files that
> we do not have access for so to make things easier to
> find the file that is accessable, you can hide all the
> unnecessary information.

Password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

#### Level 8
We need to find the password in the file `data.txt` next to the word **millionth**.

In order to match strings within a file, we must use the `grep` command.

The `grep` command is used to search for patterns within files.

We actually do not need to use any optional parameters to complete this challenge.

Simply use the `grep` command by adding a string pattern in double quotes, and the file.

`grep "millionth" data.txt`

Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

#### Level 9
We need to find the password in the file `data.txt` and is the only line of text that occurs only once.

There are two necessary commands to find the correct password; `sort` and `uniq`.

The `uniq` command finds repeated lines adjacent to each other in a file. So if one line and the line right after that are the same, this would count as a repeated line. The optional parameter `-u` would find all unique lines.

The `sort` command simply sorts all lines in ascending order in a file.

We also need another special operator known as the pipe or `|`. This allows you to use the output of one command for the input of another.

`command1 file1 | command2`

What this will do is run `command1` on file1, and then take the output of that as the input of `command2`.

So in order to print all unique lines, we would need to sort them first so that `uniq` correctly identifies all duplicate lines and gets rid of them, then we can apply `uniq -u` to search for unique lines.

`sort data.txt | uniq -u`

Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

#### Level 10
We are again tasked with finding the password in the file `data.txt` and we are told that it is in one of the few human-readable strings, preceded by several '=' characters.

A new command that will come in very handy for this level is the `strings` command. This prints strings of printable characters in a file. So this works great for our purpose because if you were to try and read the `data.txt` file, you would find that it is full of non-human-readable text.

After we eliminate the unnecessary data, we are left with a more digestable number of lines that we can go through.

But, we can do better than that to narrow our search down to only a couple lines. We are told that the password is preceded by several '=' characters, so we can simply use `grep` to look for lines that start with or just have those characters. By default, `grep` finds all matches including partial matches so as long as the line has the pattern somewhere, it will be registered as a match.

Therefore, what we can do is search for lines that have, say, at least three '=' characters.

`grep "==="`

Like the previous level, we can pass the output from the `strings` command onto `grep` to find the matches.

`strings data.txt | grep "==="`

We will end up with a few matches, with the last one being the password for bandit10.

Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

#### Level 11

