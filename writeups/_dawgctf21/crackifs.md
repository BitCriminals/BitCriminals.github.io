---
title: Crack IFS
layout: post
author: D4rkDemian
date: 2021-05-09 12:36:00 
type: Fwn (Forensics/Web/Network)
difficulty: Easy
prompt: >
    

The accounts in this QNX IFS have insecure passwords. Crack them to assemble the flag.

https://www.qnx.com/developers/docs/7.0.0/#com.qnx.doc.neutrino.building/topic/intro/intro_ifs.html

DawgCTF.ifs: https://drive.google.com/file/d/1imS0_LQTWg67bwZucoSSa9US28C1uAI2/view?usp=sharing

Author: Percival

---

we were provided an .ifs file which is a file system image of a Blackberry device so we need to dump the image i used this github tool to dump the files 
[dumpifs](https://github.com/askac/dumpifs)
so i dumped using following command:

```python
./dumpifs ../DawgCTF.ifs -d ../dump -x -b ```
![](ifs1.png)

On searching i found the shadow file in the dump and as from reading the prompt it is clear that the passwords are very weak...
So i used John the ripper to crack the hash but firstly we need to create our own wordlist.
On enumerating i found that all the passwords are 4 character long so i wrote a script to make a wordlist:

```python 
import string
s = string.printable
print(s)
with open("wordlist.txt","w") as f:
    letters = string.printable
    for i in letters:
        for j in letters:
            for k in letters:
                for l in letters:
                    word=i+j+k+l

                    f.write(word + '\n') ```

Now i ran john 
`john --rules shadow -w=wordlist.txt`
![](ifs2.png)

And hence we got our flag!! 
`DawgCTF{un_scramble}`