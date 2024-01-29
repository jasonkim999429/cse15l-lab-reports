# **Lab Report 2**
Jason Kim, A16953241

In this week's lab, I created a web server called `ChatServer` that allows users to input new chat messages and stores the entire chatlog for that session. In lab, I also explored setting up SSH keys for faster logging in onto `ieng6`.

## Part 1: `ChatServer`

Based on the skeleton code provided by the `wavelet` directory and `NumberServer` we explored in Week 2's lab, I created a new web server called `ChatServer` with the following code:

```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    // The one bit of state on the server: a String that will be updated
    // as new chat messages are added.
    String chat = "";

    public String handleRequest(URI url) {
        if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("[&=]");
            String username = parameters[3];
            String user_message = parameters[1];                                                                                        
            chat = chat + username + ": " + user_message + "\n";
            return chat;
        }
        return "To begin chatting, append the URL with: /add-message?s=<string>&user=<string>";
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

In the above code, we take the query portion of the URL and separate it into Strings that we can concatenate to form a chat message.
Then we concatenate the new chat message with a String containing all of the previous chat messages (our chatlog).

Adding Message 1
---
We added a message to our chatlog using the following path: `/add-message?s=hello%20world!&user=jason`. As we can see from the screenshot below, we got the message to display as expected.

![Message 1](lab-report-2-images/ChatServerMessage1.png)

In this usage of `/add-message`, we called the following methods:
1. `.getPath()`: Calling this method on `url` (which is a `URI`) returns the path of the `URI` as a `String`. Our `URI` was `http://localhost:4000/add-message?s=hello%20world!&user=jason`, so this method returned only the path: `/add-message?s=hello%20world!&user=jason`.
2. `.contains()`: We called this method on the `String` path of the `URI` with `/add-message` as a `String` argument to check if the argument is a substring of the path. Since `/add-message` is a substring of the path, the method returns true for the conditional statement.
3. `.getQuery()`: Calling this method on `url` returns the query portion of the `URI` as a `String`. Our  `URI` was `http://localhost:4000/add-message?s=hello%20world!&user=jason`, so this method returned only the query: `s=hello%20world!&user=jason`.
4. `.split()`: We called this method on the `String` query of the `URI` with `"[&=]"` as the argument. These are the two delimiters that allow us to separate the `String` into multiple `Strings` that we store in a `String` array named `parameters`. Our `String` array now has four `String` elements: `"s"` at index 0, `"hello%20world!"` at index 1, `"user"` at index 2, and `"jason"` at index 3. Then we created two new local `String` variables, `username` and `user_message`, and assigned them with `"jason"` and `"hello%20world!"` respectively.

Now we have all of our methods called and variables assigned values. We also have an empty `String` `chat` that was initialized at the very beginning. We update this `String` with a concatenation of the previous `chat` value, our `username` variable, a `String` `": "`, our `user_message` variable, and a new line denoted by `"\n"`. This results in the new value of `chat`, which is the variable that stores our chatlog:
```
jason: hello world!
```


