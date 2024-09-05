[https://app.hackthebox.com/challenges/Cat]()

I did this challenge it was an android backup under mobile category

I did some research on how to open .ab file and i found this on stackoverflow

==Let's say, you've got a file test.ab, which is not password-protected, you're using Windows and want the resulting .tar-Archive to be called test.tar. Then your command should be:==

==java.exe -jar abe.jar unpack test.ab test.tar ""==

i downloaded the tool mention there here  [https://github.com/nelenkov/android-backup-extractor/releases/tag/master-20221109063121-8fdfc5e]() and used it as follows: 

`java -jar abe.jar unpack cat.ab cat.tar ""`      

the command there basically unpacks the backup then output it to an achieve.
I then uncompressed the achieve,inside there was two folders ==shared== and ==apps==  
under shared there was one folder populated with data which was ==shared/0/Pictures==
there were pictures of cat but one photo stood out ==shared/0/Pictures/IMAG0004.jpg==  

below is the picture but i redacted the flag which is at the bottom

![[IMAG0004.jpg]]