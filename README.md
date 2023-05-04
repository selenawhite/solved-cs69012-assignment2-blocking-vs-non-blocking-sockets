Download Link: https://assignmentchef.com/product/solved-cs69012-assignment2-blocking-vs-non-blocking-sockets
<br>
The protocol between the client and the server is as follows:

<ol>

 <li>The client connects to the server and then asks the user for input. The user enters a simple arithmetic expression string in infix form (e.g., “1 + 2”, “(5 + 6) * 8.9”). The user’s input is sent to the server via the connected socket.</li>

 <li>The server reads the user’s input from the client socket, converts it into a postfix expression and then evaluates the postfix expression, and sends the result and the converted postfix expression back to the client as well as writes the following in a file named “server_records.txt” (at the beginning create an empty file).</li>

</ol>

<em>&lt;client_id&gt; &lt;infix_expr&gt; &lt;converted_expr&gt;&lt;answer&gt; &lt;time_elapsed&gt; </em><strong>Note:</strong> The time elapsed is the time elapsed (in seconds) from when the server​            connected to this client to returning the response to this query.

<ol start="3">

 <li>The client should display the server’s reply to the user, and prompt the user for the next input until the user terminates the client program.</li>

</ol>

At the very minimum, the server should be able to handle addition, multiplication, subtraction, division, modulus, and power operations.

Here the server should be able to simultaneously chat with (i.e. serve requests for) multiple clients. Since “accept” is a blocking system call, it is not possible to wait for other connections while serving the requests for another. This has to be handled in 2 ways:




<strong>Problem 1: Using the “fork” system call </strong>

Write two separate C programs, one for a TCP server (handles requests for multiple users) and another one for a client.

The server program will “fork” a process for every new client it receives, such that each child process is responsible for handling the request of one client.




<strong>Problem 2: Using the “select” system call </strong>

Write two separate C programs, one for a TCP server (handles requests for multiple users) and another one for a client.

The server uses the “select” call to see which clients are making the requests (it can be a connection request or read/write request) and serve the requests of multiple clients.




<strong>Sample inputs:</strong>

User types: 1 + 2, server replies 1 2 +, 3

User types: 2 * 3, server replies 2 3 *, 6

User types: 4 + 7 – 3, server replies 4 7 + 3 -, 8

User types: 30 / 1.0, server replies 30 1.0 /, 30.0

<strong>Below is a sample run of the client. </strong>

$ gcc client.c -o client

$ ./client

Connected to server

Please enter the message to the server: 22 + 44

Server replied: 22 44 +, 66

Please enter the message to the server: 3 * 4

Server replied: 3 4 *, 12

…




In parallel, here is how the <strong>sample run of the server </strong>​            looks like this (you may choose to print​            more or less debug information).

$ gcc server.c -o server

$ ./server

Connected with client socket number 4

Client socket 4 sent message: 22 + 44

Sending reply: 22 44 +, 66

Client socket 4 sent message: 3 * 4

Sending reply: 3 4 *, 12

…




<h1>Algorithm for converting infix expression to postfix</h1>

<ol>

 <li>Scan the infix expression from left to right.​</li>

 <li>If the scanned character is an operand, output it.​</li>

 <li>Else,​</li>

</ol>

<strong>      1</strong> If the precedence of the scanned operator is greater than the precedence of the​        operator in the stack(or the stack is empty or the stack contains a ‘(‘ ), push it.  <strong>      2</strong> Else, Pop all the operators from the stack which are greater than or equal to in​  precedence than that of the scanned operator. After doing that Push the scanned operator to the stack. (If you encounter parenthesis while popping then stop there and push the scanned operator in the stack.)

<ol start="4">

 <li>If the scanned character is an ‘(‘, push it to the stack.​</li>

 <li>If the scanned character is an ‘)’, pop the stack and output it until a ‘(‘ is encountered, and​ discard both the parenthesis.</li>

 <li>Repeat steps 2-6 until infix expression is scanned.​</li>

 <li>Print the output​</li>

 <li>Pop and output from the stack until it is not empty.​</li>

</ol>




<h1>Algorithm for evaluating postfix expression</h1>

<ul>

 <li>Create a stack to store operands (or values).</li>

 <li>Scan the given expression and do the following for every scanned element.

  <ol>

   <li>If the element is a number, push it into the stack</li>

   <li>If the element is an operator, pop operands for the operator from the stack.</li>

  </ol></li>

</ul>

Evaluate the operator and push the result back to the stack

<ul>

 <li>When the expression is ended, the number in the stack is the final answer.</li>

</ul>

<h1><u>Marking Scheme</u></h1>

Handling concurrency:

<ol>

 <li>Using “fork”: 20 marks</li>

 <li>Using “select”: 20 marks</li>

</ol>

Conversion from infix to postfix: 15 marks

Evaluation of postfix expression: 15 marks Contents of “server_records.txt”: 10 marks Error handling strategies: 10 marks Coding style – 10 Marks.