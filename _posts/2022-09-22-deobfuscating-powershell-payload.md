---
layout: article
title: Powershell Payload De-obfuscation
article_type: BlogPosting
date: 2022-09-22
cover: "/assets/images/cobalt.png"
permalink: /:title.html
tags: "malware obfuscation powershell"
author: c1ph3rbnuk
---

**Code obfuscation** is a common technique that malware authors implement through splitting and formatting command strings, randomization of variables, encoding or even more often adding garbage to their code by including pieces of code that do useless things like functions and variables that are entirely  never used throughout the program. All these just as a means of trying to evade and bypass Anti-virus and all the detection techniques in place.

In this blog I'll be going through deobfuscating a malicious powershell script a friend(@Oste) shared with me to play with and learn from. This was the original script [powershell_obfuscated](https://github.com/c1ph3rbnuk) 
![](assets/images/original_pwsh.png)  

The script is very obfuscated with strings broken down into individual chunks of characters concatenated with **+** signs. Knowing it's a powershell code we can save it with the powershell **.ps1** extension to get sublime text syntax higlighting. I'll be using sublime text beacuse i find it easy especially for quick search and replace operations. If the code is not syntaxed just press **CTRL + SHIFT + P** and type powershell to install the powershell package.  

The first line of the script is going to launch powershell without loading the powershell profile (-nop) also non interactively(-noni) while hiiding the window (-w __hidden__). The __-c__ option specifies the commands to be executed. 
![](assets/images/line1.png)  

Let's have a look at those commands. We have n `if statement` which basically checks the pointer size of the of the executing process. The value of this property is 4 in a 32-bit process, and 8 in a 64-bit process. If the value is 4 it knows it's running on a 32 bit machine so it saves the name of the powershell executable( **powershell.exe**) to the variable **$b** .If not it grabs the 32bit powershell binary from the **SysWOW64**. The **SysWOW64** folder stores 32 bit dll files on 64bit windows machine.
![](assets/images/check_bit.png)

The program then starts a process using the **ProcessStartInfo** class by specifying the **FileName** property as the variable name holding the powershell executable,the **Arguments** property which takes command-line arguments to pass to the application, **UseShellExecute** set to false to specify that the process should not start using the system shell, **RedirectStandardOutput** causing the process to return the output to a file or another device and the **WindowStyle** with **CreateNoWindow** set to restrict the process from creating any window.  
![](assets/images/process.png)

We are now going to focus on the arguments and see what's being executed. It is again running non interactively ith no profile. We have an `if statement` which checks whether the powershell version is greater than 3 and execute more code.
![](assets/images/ifstatement.png)

This is where the fun part begins. You can see strings are being splitted, formatted and assigned to variables with random names. If you look at the strings you can see a pattern with the single quotations. Two single quotation are used to represent one single quotation. So we can quick replace of all double single quotaton with just a single quotation. **CTRL + H** is a shortcut in sublimetext. Properly formating the first variable **$lANI** results to **EnableScriptBlockLogging**. Well, we can't go through the pain of figuring out what each splitted string result to, powershell can do it for us. Just slap that piece of string on powershell it will format it. But be careful when doing this because some strings may contain **IEX** (InvokeExpression), a command for powershell to run the strings as commands on the fly.  
![](assets/images/kalipwsh.png)

I have powershell installed on my kali, so these are all the strings. We can now rename the variables to something we easy to understand.