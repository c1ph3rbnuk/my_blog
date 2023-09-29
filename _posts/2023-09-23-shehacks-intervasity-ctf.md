---
layout: article
title: SheHacks Intervasity CTF
article_type: BlogPosting
date: 2023-09-23
cover: "/assets/images/ctf/shehacks/shehacks.jpeg"
permalink: /:title.html
tags: "ctf web forensics"
show_subscribe: true
author: c1ph3rbnuk
---

This year i attended the SheHacks Annual InterVasity CTF, a two day event hosted at USIU-Africa. It was a fun experience except the tough CTF challenges :fearful:. This is a write-up of the few i managed to solve. :relieved:

![](assets/images/ctf/shehacks/duration.png)

### Graph1
![](assets/images/ctf/shehacks/graph1.png)  

The link took us to this page which says `"Graphql running..."`   

![](assets/images/ctf/shehacks/graphql.png)   

With basic paths check for the graphql instance i found it runs on `graphql`.  

![](assets/images/ctf/shehacks/graphinstance.png)

The next step is to enumerate all available types anf fields with the introspection query `query={__schema{types{name,fields{name}}}}` to discover the schema information.   

![](assets/images/ctf/shehacks/getflag.png)

With the introspection we uncover `getFlag` field which likely retrieves the contents of the Flagtable.   

![](assets/images/ctf/shehacks/flagtable.png)

Sending `query={getFlag {id flag}}` returns a base64 encoded flag value. 

![](assets/images/ctf/shehacks/flagbase.png)

Decoding... Voila!:open_mouth:

![](assets/images/ctf/shehacks/flag.png)

### Adress
* [books.xls](ctf/shehacks/books.xls)   
* [Python Oletools](https://github.com/decalage2/oletools)

![](assets/images/ctf/shehacks/adress.png)

This was an easy challenge that required knowledge in analyzing macro-based source code.    
`Macro based malware are scripts(js, powershell,vbs) that are embedded inside Ms office documents to abuse the macro feature.`

To view if there exists any macro emmbeded scripts in a document we use `oledump.py` or `olevba` tools. 

![](assets/images/ctf/shehacks/macro.png)

The letter `M` next to the stream indicates that the stream contains Macro emmbeded code. We could use  `oledump.py books.xls -s 8 -d` to read the code but `olevba books.xls` does a better job.

![](assets/images/ctf/shehacks/modulemacro.png)

We can see the macro source codes for module 1 ans 2. Module 1 is a Visual Script that checks the document properties and executes the value for the property named comments. If we examine the file properties, comments has a value that spawns the command prompt and output `Oops`.

![](assets/images/ctf/shehacks/exif.png)

The second script is a powershell code that executes what seems to be a base64 encoded string. let's uncover what that is. 

![](assets/images/ctf/shehacks/ps.png)

It seems to be a basic PowerShell reverse shell that listens for commands from a local server(127.0.0.1) and executes those command. The challege asked for the Ip adress and the port, `SHCTF{127.0.0.1:9001}`:smiley:.