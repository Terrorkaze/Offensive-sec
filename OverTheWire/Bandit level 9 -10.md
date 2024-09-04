With this one i noticed one of my habits ,i jumped to use `cat` command because i saw the file extension and just concluded file was a text file(ASCII).This is a bad habit as it can make one to execute unknown files by being mislead by the extension.Although it is also a technique in malware analysis called dynamic analysis ,study of a file by executing it.

The safe way is to always use static analysis...
I then used `file` command to determine the nature of the file ,it was a data file type.
then i used the `strings` command.

i found the password and since the output wasn't a lot i just browsed through.

but ,you can pair it with grep to extract the pattern which was provided :
==preceded by several ‘=’ characters.==

Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
