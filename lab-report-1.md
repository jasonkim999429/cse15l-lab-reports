# **Lab Report 1**
Jason Kim, A16953241

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

In this example, I tested the `ls` command with no argument, starting in the lecture1 directory. The output was a list of all files and directories in the current working directory (the lecture1 directory). This included both files and other directories. This case is not an error, as the output matches the use case we learned in class.

```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
```


Directory path argument
---

This example starts once again in the lecture1 directory, and I provided the `ls` command with an argument: a path to another directory. We know from the previous example, the messages directory is a child of the lecture1 directory. Thus, we can use 'messages' as a relative path to that directory. We can see that the output is a list of all files in the messages directory. This is not an error; from this we can infer that giving the `ls` command a valid path to a directory as an argument will list all contents of that directory.

```
[user@sahara ~/lecture1]$ ls messages
en-us.txt  es-mx.txt  ko.txt  zh-cn.txt
```


File path argument
---

Starting in the lecture1 directory, I initiated the `ls` command with a path to a file as an argument. The output was the name of the file specified, which is the expected behavior as described by the documentation, so it is not an error. However, this output is not very helpful to us. This was repeated multiple times with different types of files (.class, .java, etc.) in order to ensure that the behavior was not specific to any one of these types of files.

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

We started in the lecture1 directory in this example, and used the `cat` command without any additional arguments. After entering the command, we can continue typing text, and after pressing 'return', the terminal prints back to us what we typed on that line, as shown below. I tried different examples, such as re-entering the `cat` command or with different arguments and path files, but the terminal only reprinted the typed line and prompted us for another input. The only way to break the input loop was to use ^C (control + C), similar to breaking an infinite while loop. This is not very helpful and can be considered an error.

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

We started again in the lecture1 directory, using the `cat` command with a path to a directory as an argument. Since messages is a child directory of our current working directory, we can use the relative path. The output indicates an error, stating that messages is a directory. This implies that we cannot use paths to directories as our argument for the `cat` command.

```
[user@sahara ~/lecture1]$ cat messages
cat: messages: Is a directory
[user@sahara ~/lecture1]$
```

File path argument
---

I start in the lecture1 directory, this time using the `cat` command with a file path as an argument. We specified the en-us.txt file in the messages directory, which is a child directory of the current working directory. The terminal printed the contents of the file as an output. I also tried the Hello.java file as an argument, which also resulted in the contents of the file being printed to the terminal. Finally, I used two file names as arguments, and the terminal outputted the contents of both files in the same order. These outputs were expected and matched the use cases we explored in class.

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
[user@sahara ~/lecture1]$ cat messages/en-us.txt messages/es-mx.txt
Hello World!
¡Hola Mundo!
```
