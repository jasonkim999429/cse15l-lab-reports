**Lab Report 1**
---

In this week's lab, I explored three commands: `cd`, `ls`, and `cat`. I tested these commands to see what sort of outputs and errors result from varying arguments (or lack of!). 

# Testing `cd`

No args
---

We first tested the `cd` command with no arguments. In this example, we started in the lecture1 directory. After inputting the `cd` command without any additional arguments, we received no output. However, the next line in the terminal shows that the prompt indicates that the current working directory is now the root directory, so it appears that the using `cd` without any argument changes the working directory to the root directory.

```
[user@sahara ~/lecture1]$ cd
[user@sahara ~]$
```

I also tested it by changing into the messages directory, located in the lecture1 directory, and inputted `cd` without any arguments and received the same results: no output in terminal, working directory changed to root directory.

```
[user@sahara ~/lecture1/messages]$ cd
[user@sahara ~]$ 
```




![Image](cd directory arg.png)

![Image](cd file arg.png)


Testing `ls`

![Image](ls no arg.png)

![Image](ls directory arg.png)

![Image](ls file arg.png)


Testing `cat`

![Image](cat no arg.png)

![Image](cat directory arg.png)

![Image](cat file arg.png)
