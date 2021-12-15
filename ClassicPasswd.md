https://tryhackme.com/room/classicpasswd

#############################################################

This is a walkthrough using Ghidra ( pre-installed with Kali )

Download the file Challenge.Challenge

Open it up with Ghidra in a new project and double click the file and analyze with decompiler box checked

I am checking the Functions tab found on the left side.

![Functions](https://github.com/Ipssisimus/THMCTFs/blob/ceaecfb6d9ce947888253423f3d23ad315718739/Images/Functions.png)

![Search](https://github.com/Ipssisimus/THMCTFs/blob/ceaecfb6d9ce947888253423f3d23ad315718739/Images/SearchFunctions.png)

The "strcpy" and "strcmp" look like the right ones. "Strcpy" will copy a string as another string to use it later. Usually this will be a user imput string copied as another string. "Strcmp" will be used to compare the user input copied string ( from "strcpy" ) to the actual password that is hardcoded into the program.

I'm searching for some extra info in the Search for strings option from ghidra.

![Search for strings](https://github.com/Ipssisimus/THMCTFs/blob/ceaecfb6d9ce947888253423f3d23ad315718739/Images/flag.png)

We found a THM{} type of flag. The two "%d%d" indicate that we can probably find in the code our flag ( with some conversion into text ) without even finding out the username. But I still want to find the username and see if the username can also lead us to the flag.

Digging some more into strings.

![Strings](https://github.com/Ipssisimus/THMCTFs/blob/ceaecfb6d9ce947888253423f3d23ad315718739/Images/combination.png)

This is a typical scenario with hardcoded passwords. You have to look for the sequence that denies you access ( prompting you wiht a "failed to authenticate" or some error message that doesn't allow you to enter ). The sequence here is "Insert user>if correct>Welcome" and "Insert user> if wrong > Authentication Error".

The sequence where it ends with "Welcome" should be the one showing the right password in the clear ( but not human-readable ).

Searching for the hexadecimals that would provide the password in the decompiler

![Password in the clear](https://github.com/Ipssisimus/THMCTFs/blob/ceaecfb6d9ce947888253423f3d23ad315718739/Images/codecombinatiopn.png)

There it is! Now, we just have to figure out the variables. I marked in the next picture the logic behind the code.

![Logic](https://github.com/Ipssisimus/THMCTFs/blob/ceaecfb6d9ce947888253423f3d23ad315718739/Images/password.png)

The function "prints" ( prompts the user ) with a "Insert your username". That will be stored in memory as "local_238". 
Then, "local_238" is copied as "local_2c8" with the strcpy command.
Finally, the strcmp command will compare "local_2c8" to the actual password, which is stored in "local_246" ( but not too fast, because I got stuck here, and it was a bit silly )

I stumbled across this and got stuck for a while here.

![Password](https://github.com/Ipssisimus/THMCTFs/blob/ceaecfb6d9ce947888253423f3d23ad315718739/Images/logic.png)

The conversion of the value attributed to "local_246" gives us just a part of the username, sadly, so, I tried to figure out the rest of it and it took me a while.

In the last screenshot you can see some lines that have a plus sign (+).

The [RPB + local_246], RAX and [RPB + local_23e],0x476b6439 and [RBP + local_23a],0x37 are the password broken down in 3 parts. 

I did some googling to figure this out and I found the answer pretty fast ( I googled "MOV C++ ) and got to this link:
https://stackoverflow.com/questions/35471997/mov-instruction-for-registers

Now, running the program and inputting the correct user name that we just found, we get a Welcome msg followed by the THM{} flag.

![Flag](https://github.com/Ipssisimus/THMCTFs/blob/8e038307e15f402778f25a83596d03f829f2fd19/Images/flagfinal.png)


There are simpler ways to do this like ltrace and other ways using other debuggers like gdb. With ltrace you'll finish very fast, but I like doing research more than just solving it fast.

Thanks!

