#Superheroes and Software Development. 20+ Best Practices for Logging (including C and C++ Specials)

![[DALL·E 2023-04-20 01.19.13 - Rich beutiful Superman sees debug logs.png]]

> Superman sat at his desk, surrounded by piles of notebooks and papers. He had just finished analyzing the logs of his latest battle with Lex Luthor and was feeling pretty good about himself.
> As he looked over his notes, he realized something important. Consistent logging was crucial for any superhero. Without it, how would he keep track of all his amazing feats?
> 
> He decided to write an article about it for the Daily Planet, titled "Why Every Superhero Needs to Keep a Log". Lois Lane was skeptical at first, but once Superman explained his reasoning, she was convinced.
> 
> After the article was published, other superheroes started taking notice. Batman even asked Superman for some tips on how to keep better logs, and before long, the Justice League was all logging their battles like pros.
> 
> Superman smiled to himself, knowing that his meticulous logging would help future generations of superheroes. He couldn't help but feel a little smug, knowing that his superpower wasn't just strength and speed, but also organizational skills.

Logging is an essential part of software development that helps developers troubleshoot issues and debug applications. However, logging can become a challenge when it produces too many or too few logs, has inconsistent or unclear formatting, or includes sensitive information. To avoid these issues, it is important to follow best practices when logging.

## Log message content best practices 
1. Use log levels correctly when logging messages in your code. These levels include `DEBUG`, `INFO`, `WARN`, `ERROR`, and `FATAL`, which can be represented by numbers, verbal strings, or both. The different levels serve different purposes, such as providing detailed debugging information, indicating potential issues that should be addressed, or reporting critical errors. Here are some examples of when to use each level:
	- TRACE: Use for highly detailed messages that are mainly used for debugging purposes. For example, logging the function and parameter used in a specific call: `Function foo() called with parameter [bar]`
	- DEBUG: Use for general debugging information. For example, logging the value of a variable: `Value of variable 'x' is [10]"`
	- INFO: Use to provide informational messages about the state of the application. For example, logging when the server started: `"Server started on port [8080]`
	- WARN: Use to indicate a potential problem or issue that should be addressed. For example, logging a failed attempt to open a file: `"Failed to open file: [example.txt]`
	- ERROR: Use to indicate an error that has occurred but is not critical. For example, logging a failed attempt to connect to a database: `Could not connect to database: [mydb]`
	- FATAL: Use to indicate a critical error that has caused the application to fail. For example, logging when the program runs out of memory: `Fatal error: Out of memory`

2. It is important to selectively enable logs that are relevant to the current runtime situation. For instance, during development, you may want to turn on all logs to aid in detecting any potential issues. During debugging, it is advisable to enable `DEBUG` and higher logs to provide a more detailed view of the program's execution. However, during production, it is best to limit the logs to `INFO` level or higher to reduce unnecessary clutter.

3. Use consistent log format that includes at least the following: Time, log level, source file, source line, function, and message. One of recommended format is `[Time][Log level] - [File]:[Line] [Function] - [Text]`. This will help ensure clarity and consistency in your logs.
> [!Example]
> `[2023-04-19 12:34:56][DEBUG] - [main.cpp:23 main] - This is a debug message`

4. For multithreaded/modular applications, consider adding the options `[Module][Thread]` to your log format. To make it more readable, understand cross-modules and cross-threads interactions.
> [!Example]
> `[2023-04-19 12:34:56][DEBUG][module01][thread01][main.cpp:23 main]This is a debug message`

5. Use placeholders enclosed in square brackets `[]` to format your log messages and prevent misaunderstanding of variables (spaces, zeros, etc).
> [!Example]
>  `[2023-04-19 12:34:56][DEBUG] - [main.cpp:23 main] - This is a debug message, user [user01], try count [3]`

6. Add relevant context in your logs:
	1.  Ensure to always add general context that is relevant to your application, such as User ID and Session ID/Transaction ID.
	2.  Additionally, include more context information in your log messages, such as parameters in your own format. This will make it easier to understand the log messages.
> [!Example]
> `2023-04-19 12:34:56 #DEBUG #main.cpp:23 #main #user01 #session01 #Cannot open db [file.db] in mode [READ_WRITE], db error [14]`

7. To provide better context understanding, consider including the name of the function that calls the current function.
  > [!Example]
  > `[2023-04-19 12:34:56][DEBUG] - [file.cpp:56 functionA functionThatCallsFunctionA] - user [user01] logged in`

8.  In order to provide better insight into crashes and errors, it is recommended to use call stack print in all cases where crashes occur, as well as in relevant error logs. This can help to identify the source of the problem more easily and provide developers with the information they need to fix the issue quickly.
> [!Example]
> `[2023-04-19 12:34:56][ERROR] - [main.cpp:23 main] - An error occurred: File not found\nCall stack: main.cpp:23 main\n file.cpp:56 functionA\n file.cpp:78 functionB`

9. In order to enhance the comprehensibility and problem-solving capabilities of a program's control flow, it is highly advisable to implement DEBUG or TRACE logs at every switch point, such as function starts and ends, `if` statements, `switch cases`, and `return` statements. This practice enables you to track the direction of control flow in your code and quickly detect any potential errors or issues that arise during development. Additionally, you can select different log levels based on the switch point's importance and relevance to your code.
> [!Example]
> `[2023-04-19 12:34:56][DEBUG] - [file.cpp:56 functionA] - <Started>`
>
>`[2023-04-19 12:34:56][WARN] - [file.cpp:78 functionB] - If condition met`

10. To improve the organization and management of log messages, it is advisable to assign a unique identifier to each message. This will help distinguish one message from another and make it easier to track and analyze specific logs when needed.
> [!Example]
> `[LID:1234]`   

11. Avoid logging sensitive information like passwords, credit card numbers, and social security numbers. Instead, use tokens or hashes to mask such data.

P.S. How to get stack or function name that call current function in C++:
- [Library Boost.Stacktrace](https://www.boost.org/doc/libs/master/doc/html/stacktrace.html)
-  [C++ - How to automatically generate a stacktrace when my program crashes - Stack Overflow](https://stackoverflow.com/questions/77005/how-to-automatically-generate-a-stacktrace-when-my-program-crashes)
Here's a possible rewrite: 

Additional information for getting the stack or function name that called the current function in C++: 
- The [Boost.Stacktrace library](https://www.boost.org/doc/libs/master/doc/html/stacktrace.html) provides a stack getting (and caller function name getting).
- How to handle crashes and how to get stacktrace without Boost library provided in this Stack Overflow post: ["C++ - How to automatically generate a stacktrace when my program crashes"](https://stackoverflow.com/questions/77005/how-to-automatically-generate-a-stacktrace-when-my-program-crashes).
- Some C++ logging libraries also offer stack trace dumps for crashes, although not many of them do.

## Log message format best practices 
1. To ensure consistency and ease of understanding for all members of your development team and tools, use logs only in English and in UTF-8 encoding.
2. Consider using a standard or structured log format that is both machine-readable and human-readable. While JSON is a good option for both, it may not be the best for human-reading due to its nesting. Avoid using XML as it is not optimal for automation tools and human-reading. An example of a JSON log format is:
> [!Example]
>`{"timestamp": "2023-04-19 12:34:56", "level": "DEBUG", "message": "This is a debug message"}`   
3. If you choose not to use JSON, avoid printing line breaks in log messages. Instead, replace them with special strings like `^M`, `\n`, etc. or remove them altogether. An example of a log message with line breaks replaced by special strings is:
> [!Example]
>  `[2023-04-19 12:34:56][DEBUG] - [main.cpp:23 main] - xml [<xml>^M <field>^M field msg^M </field>^M</xml>]`

## Log file organization best practice
1. Implement log rotation to prevent log files from becoming excessively large. This ensures that your logs are easier to manage and analyze.
2. Employ log compression to conserve disk space, especially when dealing with large volumes of logs.

## Log coding best practice
1. Consider utilizing a logging library to optimize time and effort invested in logging. This approach can offer a more efficient and standardized way of handling logging in your software.
3. Make a decision between configuring the logger at runtime for enhanced flexibility and control, or excluding logging calls for log levels that are not relevant to your situation to improve performance, memory usage, and file size.
4. Utilize asynchronous logs for improved performance.
5. Avoid using string concatenation in log messages to optimize performance.
6. Test your logs to ensure they are being generated and captured as expected. This includes testing for log levels, log format, and log content.

## Log viewing and analisyng best practices
1. Use good special tool for log reading. I think best tool for most non-distributed logs is [LogViewPlus](https://www.logviewplus.com/) (it is not ad, it is my opinion)
2. Write a script in your log viewer that opens your code editor to the file and line where a log message was printed. This can help you quickly locate and fix issues.
3. Use log aggregation tools to analyze logs from multiple sources. This can help you identify patterns and issues across your entire software ecosystem.

By following these best practices, you can create well-structured and informative logs that help you quickly troubleshoot issues and improve the overall quality of your software. Additionally, implementing logging best practices can help you identify and fix issues before they become major problems, saving you time and effort in the long run.

