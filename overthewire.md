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
For encoding a string to base64 or decoding base64 encoded data, the `base64` command comes in handy.

The command by default encodes data to a base64 format in a file, not the other way around, so if you want to decode, the `base64` command has an option `-d` which does exactly that.

`base64 -d data.txt`

Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

#### Level 12
We are told that all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions. This is known as [ROT13](https://en.wikipedia.org/wiki/ROT13) and is in fact a variation of the famous Caesar cipher.

<div align="center"><img width="275" height="175" src="https://upload.wikimedia.org/wikipedia/commons/3/33/ROT13_table_with_example.svg" /></div>

The letter 'A' becomes 'N', 'B' becomes 'O' and so forth. You can see that instead of A-Z it becomes N-Z and then A-M.

The command `tr` helps to translate or delete characters from standard input. From the [manual page](https://man7.org/linux/man-pages/man1/tr.1.html) it can be found that in order to translate or map letters to a different set by shifting like in Caesar ciphers, you have to specify two strings; the characters you would like to map, and the new set of characters that you would like the original ones to map to.

So, to shift the text back 13 positions to decode the password, you need to map `N-ZA-M` to `A-Z`.

What this does is map 'N' to 'A', ... , 'Z' to 'M', ... , 'M' to 'Z'.

<table>
  <tr>
    <td>Input</td>
    <td>
        <kbd>
            <span style="color: darkred">ABCDEFGHIJKLM</span>
            <span style="color: darkblue">NOPQRSTUVWXYZ</span>
        </kbd>
    </td>
  </tr>
  <tr>
    <td>Output</td>
    <td>
        <kbd>
            <span style="color: darkred">NOPQRSTUVWXYZ</span>
            <span style="color: darkblue">ABCDEFGHIJKLM</span>
        </kbd>
    </td>
  </tr>
</table>

<br>

`cat data.txt | tr 'n-za-mN-ZA-M' 'a-zA-Z'`

> The duplicate lower case version is simply to capture lower and
> upper-case. Read the manual page for more information.

> A helpful StackExchange answer: [Changing Individual Letter Position 
> with Bash](https://askubuntu.com/questions/1097761/changing-individual-letter-position-with-bash)

Password: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

#### Level 13
We are told that the file `data.txt` is a [hexdump](https://en.wikipedia.org/wiki/Hex_dump) of a file that has been repeatedly compressed.

The first thing to do is to follow the hint given in the problem, create a directory under `/tmp` to work with files because new decompressed files will likely be created. You do not have access to create files in the root directory.

To do this, you can use the helpful command provided, `mktemp -d` which will create a directory with a random name under `/tmp`. Next, you should copy the file with `cp` and move it to the new temporary directory. Read the [cp](https://linux.die.net/man/1/cp) manual for more information.

```bash
bandit12@bandit:~$ mktemp -d
/tmp/tmp.Shcv8OwKyH
bandit12@bandit:~$ cd /tmp/tmp.Shcv8OwKyH
bandit12@bandit:/tmp/tmp.Shcv8OwKyH$ cp ~/data.txt abc
bandit12@bandit:/tmp/tmp.Shcv8OwKyH$ ls
abc
```

Now, we can begin.

`xxd` is a command that can create a hexdump from a file, or do the reverse. In this case, we need to reverse the process, so using the [manual](https://linux.die.net/man/1/xxd), we find that in order to convert a hexdump back to its original binary form, use the `-r` option.

`xxd -r abc new`
