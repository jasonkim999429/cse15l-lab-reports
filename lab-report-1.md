# **Lab Report 1**
---

In this week's lab, I explored three commands: `cd`, `ls`, and `cat`. I tested these commands to see what sort of outputs and errors result from varying arguments (or lack of!). 

## Testing `cd`

No argument
---

I first tested the `cd` command with no arguments. In this example, I started in the lecture1 directory. After inputting the `cd` command without any additional arguments, I received no output. However, the next line in the terminal shows that the prompt indicates that the current working directory is now the root directory, so it appears that the using `cd` without any argument changes the working directory to the root directory.

```
[user@sahara ~/lecture1]$ cd
[user@sahara ~]$
```

I also tested it by changing into the messages directory, located in the lecture1 directory, and inputted `cd` without any arguments and received the same results: no output in terminal, working directory changed to root directory.

```
[user@sahara ~/lecture1/messages]$ cd
[user@sahara ~]$ 
```


Directory path argument
---

In this example, I tested the `cd` command with a path to a *directory* as an argument. Starting in the root directory, I used the lecture1 directory path as my argument for `cd`, and there was no output. However, the prompt in the next line of the terminal indicates that we changed our working directory to lecture1. Since lecture1 was a directory present in the root directory, we were able to change directory or `cd` into it, and successfully execute the command without error.

```
[user@sahara ~]$ cd lecture1
[user@sahara ~/lecture1]$ 
```


File path argument
---

In this example, I tested the `cd` command with a path to a *file* as an argument. Starting this time in the lecture1 directory, I used the Hello.java file as my argument for `cd`. There was an error output (although it did not clearly indicate that it is an error). It indicated that the Hello.java file is not a directory, implying that we cannot use it as an argument for this command. We can imply this from the prompt of the following line, which tells us that our current working directory is still the lecture1 directory. Thus, we did not change directory as we should have from usage of the `cd` command.

```
[user@sahara ~/lecture1]$ cd Hello.java
bash: cd: Hello.java: Not a directory
[user@sahara ~/lecture1]$ 
```


## Testing `ls`

No argument
---

```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
```


Directory path argument
---

```
[user@sahara ~/lecture1]$ ls messages
en-us.txt  es-mx.txt  ko.txt  zh-cn.txt
```


File path argument
---

```
[user@sahara ~/lecture1]$ ls Hello.java
Hello.java
[user@sahara ~/lecture1]$ ls Hello.class
Hello.class
[user@sahara ~/lecture1]$ ls README
README
[user@sahara ~/lecture1]$ ls messages/en-us.txt
messages/en-us.txt
```


## Testing `cat`

No argument
---

```
[user@sahara ~/lecture1]$ cat
test
test
cat
cat
cat messages/en-us.txt
cat messages/en-us.txt
cat Hello.java
cat Hello.java
^C
```

Directory path argument
---

```
[user@sahara ~/lecture1]$ cat messages
cat: messages: Is a directory
[user@sahara ~/lecture1]$
```

File path argument
---

```
[user@sahara ~/lecture1]$ cat messages/en-us.txt
Hello World!
[user@sahara ~/lecture1]$ cat Hello.java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class Hello {
  public static void main(String[] args) throws IOException {
    String content = Files.readString(Path.of(args[0]), StandardCharsets.UTF_8);    
    System.out.println(content);
  }
```
