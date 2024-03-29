# shuntingyard
Shay Spitzer
shay.spitzer@gmail.com

My implementation of the shunting-yard algorithm uses 5 classes and 2 interfaces to read infix expressions from a file, convert to postfix, and
solve them. 

My InfixCalculator method includes the main method and calls upon the other classes to solve expressions. In this class, I used java.util.Scanner 
to read from a text file and java.util.Formatter to create and write to a file, as this is the File I/O approach that I learned in CSC 171. The program is used by running InfixCalculator.java with two arguments, both text files. The first is the text file from which expressions are read (I included one with my own examples in addition to the one provided in the assignment). The second argument is the name of the text file which the program will create and write to. I included two example outputs: my_eval1.txt and my_input_eval.txt, which were generated by running the program with infix_expr_short.txt and my_input.txt as the input file, respectively (program run as "java InfixCalculator infix_expr_short.txt my_eval1.txt" and "java InfixCalculator my_input.txt my_input_eval.txt", respectively). When run as such in the terminal, the program will create a new text file in the src folder with the solutions to the input, which can then be opened. Files with the same name should not be created. While the program has worked fine for me from the terminal, I found that running it in eclipse requires the input file to be moved outside the src folder into the project folder. Eclipse also seems reluctant to show newly created files; collapsing the project folder and refreshing usually does the trick. This is the only problem I have encountered with my program, but as I said it should run without problems as presented in the terminal. I used some exception handling to avoid errors when creating a Path to the text file to scan from and creating a new file to write to. I also wrote the class FileWriter in order to create custom formatting methods, mostly in order to avoid a newline at the end of my output file, and handle some exceptions outside of the main loop. I use this class to create, write to and close files using Formatter. 

The majority of the work is done by my InfixSolver class, which is instantiated and used in the main loop. It is constructed using a string that represents an expression in infix notation. It contains methods to parse, convert to postfix, and solve, as well as some supporting methods. The parse() method was probably the part of this project that I struggled the most with. I quickly realized that the input in infix_expr_short.txt would require some work to break it up into individual tokens. Tokens are not separated by spaces, and some tokens were represented by characters immediately adjacent to one another (like a parenthesis followed by a number without a space), while others were represented by a string of characters (like numbers with multiple digits and trig operations). My parse method basically splits each string expression into tokens by character index, but if a digit or letter is detected, the subsequent indexes are checked to see if a decimal number or something similar can be constructed. I'm not sure if there was a simpler way to handle the input, but this method seems to work for everything I've thrown at it. Each time a token is parsed within this method, the toPostFix() method is called in order to place the token in its correct place in the stack or queue. This method is just an implementation of the first part of the shunting-yard algorithm. It calls upon the isDouble(), getPrecedence(), and getAssociativity() methods that I wrote to evaluate each token appropriately. The isDouble() method is relatively straightforward, simply returning a boolean value. The getAssociativity() method only does anything if the token is right-associative, in which case the behavior of the algorithm needs to be altered. The getPrecedence() method ranks the precedence of operators using arbitrary integers. These latter two methods are called each time an operator is encountered in order to perform the proper steps. Overall, toPostFix() just follows the steps described in the project assignment. When an entire line has been processed by the parse() and toPostFix() methods, the loop in parse() ends and all of the stack elements are popped and enqueued. Back in the main loop, the solvePostFix() method is called to complete the algorithm. In this method, each element is dequeued, evaluated, and pushed to the stack in turn. Once everything has been dequeued, the stack contains the solution to the postfix expression. This solution can be accessed, formatted, and printed from the main loop through the getSol() method. I also added a printStack() and printQueue() method to print the contents of the stack and queue at any point. I mostly used these for debugging but they could potentially be useful to some client as well. 

My stack and queue implementations are largely based on examples from the textbook, as cited in the comments in the code. They both use a 
nested Node class to build a linked list and implement respective interfaces to perform the required methods (pop, push, queue, dequeue). 
They also include methods to print themselves.

My program also does handle the additional operations specified for extra credit. It can process exponents (with the ^ character), modulo, 
sine, cosine, and tangent. I included a file "my_input.txt" with some examples that demonstrate these capabilities.

Here is a list of files included in my submission:

Source code:
InfixCalculator.java (main loop)
InfixSolver.java (meat of the algorithm)
FileWriter.java (creates/writes file)
MyStack.java
MyQueue.java
Queue.java (interface)
Stack.java (interface)

Text files:
infix_expr_short.txt (from assignment)
my_input.txt (my additional test cases)
my_eval1.txt (example output from infix_expr_short.txt)
my_input_eval.txt (example output from my_input.txt)
README.txt (this file)
