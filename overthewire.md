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
> so it is better to use escape it using `\!`

`\! -executable`

Therefore, the full line to find the file we are looking for is:

`find -type f -size 1033c \! -executable`

Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

#### Level 7

