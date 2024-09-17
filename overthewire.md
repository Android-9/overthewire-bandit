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

#### Level 3
