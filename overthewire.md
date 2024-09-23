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

Alternatively, you can also login via this approach:

`ssh bandit0@bandit.labs.overthewire.org -p 2220`

---

#### Level 1
Use the `ls` command to list all files in the current working directory.

You can always see the current working directory from the beginning prompt like `bandit0@bandit:~$` where the `~$` indicates the root directory.

In order to read the contexts of a text file, you can use `cat`. There are also various optional parameters you can attach but for now using just the base `cat` is sufficient.

`cat readme`

Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

> Make sure to exit out of the ssh connection via 
> `logout` and then connect again but with the username 
> `bandit1`.

---

#### Level 2
`-` is a special character in linux bash. Therefore if you try and read it through the usual means `cat -`, this would not work because the `-` is reserved for attaching optional parameters to commands.

The way you circumvent this is to either use redirection:

`cat < -`

Or another common approach is to specify the full location of the file using `./`:

`cat ./-`

Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

---

#### Level 3
Spaces are also interpreted by linux bash as extra parameters, you could say. So reading the file using `cat spaces in the filename` would not work. It would only register the first parameter `cat` but not recognize the other words that are part of the file name.

An easy fix for this is to put quote around it like this:

`cat "spaces in the filename"`

Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

---

#### Level 4
Hidden files are denoted with a `.` at the start of their filename which causes them to be invisible through the usual `ls` command.

However, `ls` has plenty of optional parameters, one being `-a` which makes it so that it lists all entries including ones that start with `.`.

`ls -a`

> In order to change to a different directory, use `cd 
> directoryname`.

Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

---

#### Level 5
To find out about the type of a file, use the `file` command.

For example, `file ./-file00` will print the output `./-file00: data` which shows that this file is in some 'data' format.

What we really want though is a human-readable file like a text file.

To display the types of each file in the current working directory, you can use:

`file ./*`

where `*` is a wildcard character that matches everything.

Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

---

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

---

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

---

#### Level 8
We need to find the password in the file `data.txt` next to the word **millionth**.

In order to match strings within a file, we must use the `grep` command.

The `grep` command is used to search for patterns within files.

We actually do not need to use any optional parameters to complete this challenge.

Simply use the `grep` command by adding a string pattern in double quotes, and the file.

`grep "millionth" data.txt`

Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

---

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

---

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

---

#### Level 11
For encoding a string to base64 or decoding base64 encoded data, the `base64` command comes in handy.

The command by default encodes data to a base64 format in a file, not the other way around, so if you want to decode, the `base64` command has an option `-d` which does exactly that.

`base64 -d data.txt`

Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

---

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

---

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

Where 'new' is just an arbitary name for the output file.

Now that we have converted the hexdump back to its original binary form, the next step is to start decompressing it. However before that, we need to know the type of the file / compression in order to determine what decompression to use.

To do this, look at the first few bytes in the hexdump to find the file signature. Every file type has an accompanying file signature, a whole list of them can be found [online](https://en.wikipedia.org/wiki/List_of_file_signatures).

Either you can:

`cat abc | head -n 1`

Or,

`xxd new | head -n 1`

> Note that the `head` command simply prints the first ten
> lines of a file but we can adjust it manually by adding the
> option `-n` followed by the number of lines you want to be
> printed.

Both do exactly the same thing, as the former just prints the hexdump, and the latter creates the hexdump of the just created original binary form of the hexdump and prints the data.

Regardless, you will find the result to be:

`00000000: 1f8b 0808 dfcd eb66 0203 6461 7461 322e  .......f..data2.`

If you search the first two bytes **1F 8B** in the list of file signatures, you will find that it corresponds to the GZIP compressed file format.

This is in fact what the `file` command does. It reads the hexdump of a file to determine what the format is. So, alternatively to make things easier, you can just use this command.

`file filename`

Given that we now know that it is a GZIP compressed file, we first need to rename the file by adding the extension `gz` before running the `gzip` command.

`mv new new.gz`

Next, run the `gzip` command with the option `-d` to decompress instead of compress. Read the [gzip](https://linux.die.net/man/1/gzip) manual for more information.

`gzip -d new.gz`

After decompressing, it can be found again by using `file` that the format is now a compressed file using Bzip2 algorithm. We can go through similar steps by renaming the file and then running the `bzip` command.

`mv new new.bz2`

`bzip2 -d new.bz2`

Read the [bzip2](https://linux.die.net/man/1/bzip2) manual for more information.

Follow the same process once more and then you will find that the file is now in the format of a POSIX tar archive. Rename the file to have the extension `.tar`, then we can use the `tar` command and the option `-xf` to extract all files from the archive. Read the [tar](https://linux.die.net/man/1/tar) manual for more information.

`tar -xf new`

This will extract a new file called `data5.bin`.

After that, it is just repeatedly finding the type of the file, renaming it, and then running the appropriate command.

Password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

---

#### Level 14
To login to bandit14, we can use the private [SSH](https://help.ubuntu.com/community/SSH/OpenSSH/Keys) key from the root directory. Use the `-i` flag in conjunction with the `ssh` command. Refer back to the [ssh](https://linux.die.net/man/1/ssh) manual for further information.

`ssh localhost -p 2220 -l bandit14 -i sshkey.private`

Or,

`ssh bandit14@localhost -p 2220 -i sshkey.private`

Because we are already on the remote server and would like to login as a different user, `localhost` is used, which refers to the hostname of the machine you are currently working on.

After successfully logging in, we can simply read the file that contains the password from the location given.

`cat /etc/bandit_pass/bandit14`

Password: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

---

#### Level 15
We are told that the password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

Going through some of the commands that may be useful, we find the `telnet` command which is used to communicate with another host using the TELNET protocol. The format follows like this:

`telnet [ip_address][port]`

So, in order to communicate with the localhost on port 30000:

`telnet localhost 30000`

Doing so will connect you to that port, and you can begin submitting messages.

Submitting the pasword for the previous level yields the next password.

Password: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

---
**Reading Material**
> [How the Internet Works in 5 Minutes](https://www.youtube.com/watch?v=7_LPdttKXPc) (Not entirely accurate but good enough) <br>
> [IP Addresses](https://computer.howstuffworks.com/web-server5.htm) <br>
> [Localhost](https://en.wikipedia.org/wiki/Localhost) <br>
> [Ports](https://computer.howstuffworks.com/web-server8.htm)

---

#### Level 16
We are told that the password for the next level can be retrieved by submitting the password of the current level (refer back to Level 15) to port 30001 on localhost using SSL/TLS encryption.

In order to connect to a remote host using SSL/TLS encryption, we can use the `openssl` command which is essentially a toolkit that implements SSL and TLS network protocols. It has an abundance of commands accompanied with a variety of options and arguments. Refer to the [openssl](https://linux.die.net/man/1/openssl) manual page for more information.

For our purpose, we need to use the `s_client` command to connect to a remote host using SSL/TLS. To connect to a particular host and port, use `-connect host:port`. For more information, refer to the [s_client](https://linux.die.net/man/1/s_client) manual page.

So, by following the instructions:

`openssl s_client -connect localhost:30001`

This creates a secure connection to the localhost on port 30001 and we can again proceed by entering last level's password. Lo and behold, the response will be the password to move onto the next level.

Password: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

---

**Reading Material**
> [What is SSL/TLS?](https://en.wikipedia.org/wiki/Transport_Layer_Security) <br>
> [OpenSSL](https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/index.html)

---

#### Level 17
We are told that the credentials for the next level can be found by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First, we should find which ports have a server listening on them, then find out which of those speak SSL/TLS. Then just test each server as only one of them will give the credentials.

To look for all ports that have a server listening on them, we can make use of a powerful tool called [Nmap](https://nmap.org/book/toc.html) which is at its core, a network scanner.

To scan all open ports of a host, simply use:

`nmap hostname`

To specify a port range to scan through, use the option `-p` followed by `somenumber-anothernumber`.

Putting this all together we have:

`nmap localhost -p 31000-32000`

Nmap will print a scan report:
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-21 07:12 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00019s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.10 seconds
```
Based on this report, we can see that only the ports 31046, 31518, 31691, 31790, and 31960 are open. However, all of their services are unknown.

We can use `nmap` again but this time enable [service and version detection](https://nmap.org/book/man-version-detection.html) via the option  `-sV`. Now that the possible port numbers have been narrowed down, we only need to specify those ports to be scanned to speed up the search.

`nmap -sV localhost -p 31046,31518,31691,31790,31960`

> While this is not necessary, we can make this process much faster by adding two additional parameters, `--version-light` and `-T4`. You can read up on the details of changing the version intensity via the link earlier on service and version detection, and the timing templates' documentation is linked [here](https://nmap.org/book/performance-timing-templates.html).
>
> `nmap -sV localhost -p 31046,31518,31691,31790,31960 --version-light -T4`

After the scan is done, you should see the report:
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-21 07:16 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00015s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

We can observe that only ports 31518 and 31790 speak SSL. Now this is just a matter of connecting to each one and submitting the current password to find the one that responds with the credentials.

Remember `openssl s_client -connect localhost:portnumber`.

> If you are encountering problems with submitting the password like "DONE", "RENOGOTIATING" or "KEYUPDATE", add an option `-quiet` at the end. For more information, read this [section](https://linux.die.net/man/1/s_client#:~:text=for%20all%20others.-,Connected%20Commands,-If%20a%20connection) in the `s_client` manual page.

What you will find is that port number 31790 is the correct server. The responese will be in the form of an RSA private key, however it is not really usable at the moment because we need it in a file.

So, we will have to create another temporary directory with `mktemp -d`. Navigate to this directory for easier access.

Now, repeat the same thing but this time, redirect the output to a new file.

`openssl s_client -connect localhost:31790 > private.key`

If we were to now try and use `ssh` with the private key as previously done in Level 14, we will be met with an error stating that the private key is too open, and private key files should not accessible by others. To fix this, change the permissions to be more restrictive.

`chmod 400 private.key`

Now try again, and you should be logged into bandit17.

Simiarly with Level 14, you can find the password with `cat /etc/bandit_pass/bandit17`.

Password: EReVavePLFHtFlFsjn3hyzMlvSuSAcRD

---

**Reading Material**
> [What is a Port Scanner](https://en.wikipedia.org/wiki/Port_scanner) <br>
> [chmod](https://www.geeksforgeeks.org/chmod-command-linux/)

---

#### Level 18
There are two files in the home directory: `passwords.old` and `passwords.new`. We are told the password for the next level is in `passwords.new` and is the only line that has been changed between the two files.

The `diff` command compares files line by line and by default prints the differences betweeen the files. For more information, refer to the [diff](https://linux.die.net/man/1/diff) manual page.

`diff passwords.new passwords.old`

The output will look like this:
```
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> ktfgBvpMzWKR5ENj26IbLGSblgUG9CzB
```
The first password corresponds to the first file (`passwords.new`) and the second password corresponds to the second file (`passwords.old`). Given that the password is in the `passwords.new` file, it must be the first one.

Password: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

#### Level 19
The password for the next level is simply in a file `readme` in the home directory. However, the issue is when you try and login normally with `ssh` it logs you out immediately with a message "Byebye!".

As mentioned in the level description, it has something to do with the **.bashrc** file being modified. This file is a shell script that runs whenever a new bash session is initialized. You can put commands just like you can in an interactive shell and the main purpose is to setup and/or personalize their shell environment. Read more information about [bashrc](https://ultahost.com/knowledge-base/bashrc-file-in-linux/#:~:text=bashrc%20file%20is%20a%20shell,environment%20variables%2C%20and%20executing%20commands.). 

There are actually a myriad of ways you can approach this problem.

##### Approach #1
One way is if you return to the [ssh](https://www.man7.org/linux/man-pages/man1/bash.1.html) manual page, you will find that you can actually input commands such as `ls` that will run when connected to the remote server before the .bashrc runs. So, you could do this:

`ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme`

##### Approach #2
You can make it so bash does not run the `.bashrc` file. There is a command `bash` that allows you to specify certain aspects of how you want the interactive session to be. One of the options is `--norc` which will change the behavior when starting the bash session to not read and execute the initialization file. Read more information about the [bash](https://man7.org/linux/man-pages/man1/bash.1.html) command.

`ssh bandit18@bandit.labs.overthewire.org -p 2220 bash --norc`

> Note that you won't see a prompt because the [ps1](https://wiki.archlinux.org/title/Bash/Prompt_customization#:~:text=Bash%20has%20five%20prompt%20strings,a%20multi%2Dline%20command) is not set but that is just a side effect of forcing the bash terminal to not execute the .bashrc file. You can input commands as normal but it will just be a bit strange.
>
> If you would like to have something that resembles more like a usual interactive shell, you can attach `-t` which forces a pseudo-terminal allocation.

Then you can just read the file using `cat`.

##### Approach #3
You can also do something similar to the first approach and invoke another bash shell like this:

`ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/bash`

Alternatively, you can use the flag `-t` like in the second approach to force a pseudo-terminal, that way you're able to run `/bin/sh`.

`ssh -t bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh`

<br>

Password: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

---

#### Level 20
Executable files can have an access rights flag **setuid** which allows users to run an executable with the file system permissions of the executable's owner or group. Read the [setuid](https://en.wikipedia.org/wiki/Setuid) wiki for more information.

When you run the setuid binary file `bandit20-do` without any arguments using:

`./bandit20-do`

It directly tells you that you can run commands as another user (in this case bandit20). So with this information, you might think to read the password file in `/etc/bandit_pass/bandit20` because you can act as bandit20 and thus have the permission to read it.

`./bandit20-do cat /etc/bandit_pass/bandit20`

Password: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

---

#### Level 21
In this level there is another setuid binary file but this time, it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level. If the password is correct, it transmits the password for the next level.

You first need to launch your own server that can be connected to. The command `netcat` or `nc` can function as a server, listening to inbound connections the specified port. For more information, read the [nc](https://www.commandlinux.com/man-page/man1/nc.1.html) manual.

To setup a simple listener, use:

`netcat -l -p 1234 &`

Or

`netcat -lp 1234 &`

1234 is just some arbitrary port number. Note that you can also use `nc`, they are interchangeable.

> Notice the `&` attached at the end is used to tell the system to run the command in the background. This is related to Unix 'job control' commands. You can always see all jobs using the `jobs` command.

**However**, this is not sufficient because what you want is when you connect to this server, it should output the previous level's password. In order to do this, use the `|` as learned in an earlier level and attach `echo "password"` to the start.

`echo "password" | nc -lp 1234 &`

> The `echo` command simply prints anything you put after it.

Once you have this running in the background, you can run the executable on the port 1234:

`./suconnect 1234`

This will then read the password that was returned from the listener, and compare it to the previous level's password. Assuming there were no errors, it should give back the password for the next level.

Password: EeoULMCra2q0dSkYj561DX7s1CpBuOBt

---

#### Level 22
[Cron](https://www.geeksforgeeks.org/cron-command-in-linux-with-examples/) is a utility that as described in the level description, is a time-based job scheduler. You can create what's known as a 'crontab' which is a file that tells Cron to run some command at some particular time and date in equal intervals. System admins often create their own crontabs to automate tedious processes that need to be run regularly.

If you go over to `/etc/cron.d/` you will find various cronjobs, in particular, one called `cronjob_bandit22`. If you read the file using `cat` you will get:

```
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

You can see that this cronjob runs the shell file `/usr/bin/cronjob_bandit22.sh` as bandit22 repeatedly every minute.

Navigate to this script to see what is being executed.

`cat /user/bin/cronjob_bandit.sh`

```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

This script creates a new file in the `/tmp` directory that gives read access to all users as given by the 4 at the end. Then, it prints the contents of the `bandit22` file which contains the password and outputs it into the new file.

Now, just go ahead and read the new file to obtain the password for the next level.

Password: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

---

#### Level 23
This level is very similar to the previous level.

Go over to `/etc/cron.d/` and you will find `cronjob_bandit23`. Read through the contents of the file:

```
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

Again, you can see that this cronjob runs the shell file `/usr/bin/cronjob_bandit23.sh` as bandit23 every minute, every day.

Navigate to this script to see what is being executed.

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

The first line sets a variable `myname` as the output of the command `whoami` which prints the user that ran the command. In this case, it would be bandit23.

> Note that in order to access the output of a command and store it in a variable you follow the format: `variable=$(command)`. The `$` is used to access a variable or the return of a command.

Next, it sets another variable `mytarget` as this entire line:

`echo I am user $myname | md5sum | cut -d ' ' -f 1`

To break it down, the first part simply prints 'I am user bandit23', as the variable `myname` would be set to bandit23.

Then this output gets passed onto another command `md5sum`. [md5sum](https://man7.org/linux/man-pages/man1/md5sum.1.html) is a command that computes and verifies MD5 hashes. It is a hashing algorithm.

If you were to run the first two commands...

`echo I am user $myname | md5sum`

You will get a cryptic string:

`8ca319486bfbbc3663ea0fbe81326349  -`

This is a hash of 'I am user bandit23' using MD5.

Lastly, the `cut` command is used to cut out sections from each line of a file. By default, the field delimiter is a TAB. You can specify your own delimiter by using `-d`. Looking at the command above, you can see that the delimiter has been specified to be a space. The `-f` option allows you to select only certain fields and prints any line that contains no delimiter character. The command specifies 1 which points to the first element. Refer to the [cut](https://man7.org/linux/man-pages/man1/cut.1.html) manual for more information.

The string will be split and only the first part selected, leaving you with:

`8ca319486bfbbc3663ea0fbe81326349`

Finally, on the last line it again reads the contents of the `bandit23` password file and outputs it to the `/tmp` directory under the name of the string above.

Reading that file with `cat /tmp/8ca319486bfbbc3663ea0fbe81326349` yields the password for the next level.

Password: 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

---

#### Level 24
Go over to `/etc/cron.d/` to find `cronjob_bandit24`.

```
@reboot bandit24 /usr/bin/cronjob_bandit24.sh  &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh  &> /dev/null
```

Again, navigate to the script.

```bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

The program first sets a variable `myname` as the user that runs the cronjob, in this case bandit24.

Then it navigates to the directory `/var/spool/bandit24/foo`.

A for loop is run for every file in the current working directory, first checking if the file is "." or ".." which actually represents the current and parent directory respectively. If it is not, it goes ahead and prints the file name, then sets a variable `owner` as the name of the owner of the file. If the owner is "bandit23", it then executes the file after waiting 60 seconds. Finally, it deletes the file.

Given this information, you could create a script that does something similar to the previous level, read the contents of the password file in `/etc/bandit_pass_bandit24` and output it to a file that you can access.

First, create a temporary directory and navigate to it.

```bash
bandit23@bandit:~$ mktemp -d
/tmp/tmp.xvOO8geGkn
bandit23@bandit:~$ cd /tmp/tmp.xvOO8geGkn
bandit23@bandit:/tmp/tmp.xvOO8geGkn$ nano bandit24_password
```

Next, use a text editor like Nano or Vim to create a new file for the script.

```bash
#!/bin/bash

password=$(cat /etc/bandit_pass/bandit24)
touch /tmp/tmp.1zwgRTjGlP/$password
```

Once you have saved the file using `CTRL+X` and `Y`, you need to change the permissions of the script and the tmp folder to ensure that when the cronjob is run, the script will be able to be executed, and can access the tmp folder to create the password file.

```bash
bandit23@bandit:/tmp/tmp.xvOO8geGkn$ chmod 777 /tmp/tmp.xvOO8geGkn
bandit23@bandit:/tmp/tmp.xvOO8geGkn$ chmod +rx bandit24_password
```

The last thing is to copy the `bandit24_password` file into the directory `/var/spool/bandit24/foo`.

`cp bandit24_password.sh /var/spool/bandit24/foo/bandit24_password.sh`

Wait approximately 60 seconds, then list the files in the current working directory. You should see a new file with the name of the password for the next level.

> Note that if the script is not working and the password file remains empty after following this process, try changing the script to this:
>
> `mkdir -p /tmp/tmp.xvOO8geGkn/$(cat /etc/bandit_pass/bandit24)`
>
> Courtesy of user [liviuspiroiu](https://github.com/liviuspiroiu) from [here](https://mayadevbe.me/posts/overthewire/bandit/level24/).

Password: gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

---

#### Level 25
You are told that a daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, calling brute-forcing. 

Create another temporary directory with `mktemp -d`, then navigate to it.

You can test the port by using `netcat localhost 30002`:

```bash
bandit24@bandit:~$ netcat localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
abc
Wrong! Please enter the correct current password and pincode. Try again.

```

After playing around with this, it makes sense to create a script that tests every possible combination.

However, using something like `echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0000" | netcat localhost 30002` only works for one password. If you wrote the script like this, it would have to repeatedly connect to the port which is not a great idea because you would end up with thousands of jobs.

What you need to do is create a separate text file that holds all possible combinations separated by a newline. Read [this](https://askubuntu.com/questions/1004901/connect-via-netcat-and-send-messages-in-bash-script).

First create a file called `passwords.txt`.

Next open up `nano` or `vim` to write a script `bruteforce.sh`:

```bash
#!/bin/bash

cd /tmp/tmp.ZCpD4POwi7

for i in {0000..9999};
do
  printf "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 %d\n" $i >> passwords.txt
done
```

This script navigates to the temporary directory and then runs a for loop from 0000 to 9999. Looping through a range of numbers follows the format `{num1..num2}`, for example if you want to loop through 1-100 you would write `{1..100}`. If you want to include extra zeros you can do `{000..100}`.

This writes every possible password combination into the `passwords.txt` file separated by a newline for each one.

> Make sure you do not forget to change the permisions of the script so that it can be executed.

Execute the script with `./bruteforce.sh`. You can check the `passwords.txt` file has changed with `head passwords.txt`.

Then you run:

`nc localhost 30002 < msg.txt`

This will connect to the port and feed each line as input sequentially. The process will stop when the password is correct.

Password: iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

---

#### Level 26
