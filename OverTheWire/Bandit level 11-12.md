For this one i read the description and picked up that rot13 is at play so i copied the contents and decoded it at https://www.dcode.fr/rot-13-cipher, and found the password which is below :

The password is ==7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4==

Technically i didn't solve the challenge the way its supposed to but the tool that contains rot13 that i know of is ==hxtools== ,so i had to use man on commands listed there to see which one i could possible use to solve the lab.

i did some digging and found this https://medium.com/@piyush.kochhar1/rot13-a-missing-guide-c811427cfe6e  ,i can use tr command like this tr a-zA-Z n-za-mN-ZA-M ,this is the algorithm for the rot13.