# Discussion and Reflection

[ Question 1 ] 

For this task, you will three new .irv programs. These are
`ir-virtual?` programs in a pseudo-assembly format. Try to compile the
program. Here, you should briefly explain the purpose of ir-virtual,
especially how it is different than x86: what are the pros and cons of
using ir-virtual as a representation? You can get the compiler to to
compile ir-virtual files like so: 

racket compiler.rkt -v test-programs/sum1.irv 

(Also pass in -m for Mac)

  IR-Virtual refers to an Intermediate Representation (IR) tailored for virtual machines. While
  instruction set architectures such as x86 are closely tied to specific hardware capabilities, IR
  Virtual helps us combine high-level programming languages with low-level machine code. This
  makes it very useful because it allows programs to run on different types of computers without
  needing major changes. Using IR-Virtual has a great advantage, as it makes it easier to develop
  and manage software since it works the same way across all platforms. On the contrary, because
  it is a bit removed from the specific hardware details, it might not run as effectively as systems
  such as x86 that are directly designed for specific types of computers. For compiling and
  translating IR-Virtual code in this project, we use a tool that is developed with the racket
  programming language and run commands such as ‘racket compiler.rkt -m
  test-programs/sum1.irv to turn the IR-Virtual code into a form that our computer can run.

[ Question 2 ] 

For this task, you will write three new .ifa programs. Your programs
must be correct, in the sense that they are valid. There are a set of
starter programs in the test-programs directory now. Your job is to
create three new `.ifa` programs and compile and run each of them. It
is very much possible that the compiler will be broken: part of your
exercise is identifying if you can find any possible bugs in the
compiler.
  
For each of your exercises, write here what the input and output was
from the compiler. Read through each of the phases, by passing in the
`-v` flag to the compiler. For at least one of the programs, explain
carefully the relevance of each of the intermediate representations.
  
For this question, please add your `.ifa` programs either (a) here or
(b) to the repo and write where they are in this file.
  
  
  0newprogram1.ifa
    (if 1 (print (+ 1 (* 1 2))) (print (- 1 (+ 6 6))))
  
    Explanation: 
    IfArith: This is the initial representation of the code. It represents the basic arithmetic operations,
    conditionals (if), and printing. In this stage, the code is parsed and checked for syntax and
    basic semantics
  
    IfArithTiny: At this stage, the code is very similar to how it's presented in IfArith. So the “if” and
    primitives are unchanged since IfArithTiny supports conditional expressions and basic
    prims.
    
    Administrative Normal Form: In this stage there will be temporary bindings and a conditional transformation. For
    example, the expressions ‘1’, “+ 1 (* 1 2))” and “-1 (+ 6 6))” will be assigned to
    temporary variables.
    
    IR-Virtual: At this stage IR-Virtual represents the code in a simplified intermediate representation
    that resembles assembly language. All operations are expressed in terms of virtual
    registers and simple instructions. It's a low-level representation that abstracts away some
    complexities of the source language.
    
    x86: This stage translates the IR-Virtual code into actual x86 assembly language instructions.
    It maps virtual instructions and registers to their x86 equivalents. The resulting code is
    ready to be assembled and linked into a binary executable.


  0newprogram2.ifa
    (let* [x (+ 5 2)]
    [z (* x 2)]
    [y (+ (* z x) (+ x (+ (+ z 4) (+ z 2))))]
    (print y))
  
  0newprogram3.ifa 
    (let* [x (+ 1 2)]
    [y (* x x)]
    (print (% y x)))


[ Question 3 ] 

Describe each of the passes of the compiler in a slight degree of
detail, using specific examples to discuss what each pass does. The
compiler is designed in series of layers, with each higher-level IR
desugaring to a lower-level IR until ultimately arriving at x86-64
assembler. Do you think there are any redundant passes? Do you think
there could be more?

In answering this question, you must use specific examples that you
got from running the compiler and generating an output.

  We looked at the various steps the compiler takes to convert our written .ifa programs into
  machine-executable code through a sequence of stages. Lexical analysis is the initial step in the
  process, where the compiler examines the program and breaks it down into manageable chunks
  such as keywords, numbers, and symbols. Next comes syntax analysis, where the code is
  arranged through the programming language’s grammar (in this case racket) and then creates a
  structure that represents other programming constructs such as expressions and statements.
  Following this, the compiler performs semantic analysis where it checks for logical consistency
  in the program and makes sure that variables are declared before use and the operations are
  performed on data types that are compatible. This makes it so that the program’s instructions
  make sense in the context of the language’s rules. The compiler also goes through the
  intermediate code generation phase where it translates the high-level language constructs into a
  lower-level IR. At the code generation stage, the intermediate code is translated into the machine
  language of the target processor (the x86-64 assembly), and the instructions are finally
  commands that the CPU can execute. Through each of these passes, the compiler ensures that the
  program is accurately translated into a form that the computer can understand and execute. I
  think that every one of these passes is important to translate the code efficiently to the target
  language, it also isn’t necessary to add more steps.


[ Question 4 ] 

This is a larger project, compared to our previous projects. This
project uses a large combination of idioms: tail recursion, folds,
etc.. Discuss a few programming idioms that you can identify in the
project that we discussed in class this semester. There is no specific
definition of what an idiom is: think carefully about whether you see
any pattern in this code that resonates with you from earlier in the
semester.

  We can think of an idiom as a fixed pattern of code that efficiently performs tasks which would
  otherwise be expressed in multiple other ways. For example, we can use conditional branching
  (if/else statements) to check if a condition holds for a particular structure; suppose this structure
  is a list and we’re checking each item in that list. Instead of using multiple if/else statements for
  each item in that list, we can iterate through using a loop which would only contain one instance
  of conditional branching. Here the loop would be an idiom. Similarly, throughout this project,
  there are idioms that efficiently perform the tasks of if/else statements such as match cases and
  usages of and/or.
  
  Match cases allow you to take an expression, compare it with each case until it finds a case that
  fits the structure of the expression, and finally manipulate/process it. We can think of an
  expression matching to a case as the condition holding true, thus entering the body of the
  if-statement; similarly, we can think of the expression moving onto the next match case as the
  condition being false, and entering the else branch of the conditional. All the functions defined in
  the project (ifarith?, value?, label?, etc.) use match cases to effectively compare expressions and
  perform the appropriate operations afterward.
  
  Logical operators such as and & or can be used within if-statements, or on their own, as a way to
  evaluate expressions as true or false, as well as combining conditions. In the project these
  operators are used within match cases such as in “ir-virtual?” which returns true if “instrs” is a
  list and every element in the list satisfies the condition specified by “labeled-virtual-instr?”. This
  approach of using and simplifies the code by checking multiple conditions of a single
  expression, eventually evaluating that expression as true or false.
  
  Not all of the idioms used in this project are directly related to conditional branching, like the
  given examples, but match cases/logical operators are prominent here and have been used since
  earlier in the semester.


[ Question 5 ] 

In this question, you will play the role of bug finder. I would like
you to be creative, adversarial, and exploratory. Spend an hour or two
looking throughout the code and try to break it. Try to see if you can
identify a buggy program: a program that should work, but does
not. This could either be that the compiler crashes, or it could be
that it produces code which will not assemble. Last, even if the code
assembles and links, its behavior could be incorrect.

To answer this question, I want you to summarize your discussion,
experiences, and findings by adversarily breaking the compiler. If
there is something you think should work (but does not), feel free to
ask me.

Your team will receive a small bonus for being the first team to
report a unique bug (unique determined by me).

  We encountered a variety of errors while working on the project, some of which were minor
  syntax errors while running commands on the Mac terminal. Some of them were more major
  errors wherein we wrote entirely incorrect commands that resulted in the program failing to
  compile or run, or the output being different from the expected one. An example of an incorrect
  command used is: -Wl,-ld_classic test-programs/sum1.o -o
  test-programs/sum1-macosx_version_min 11.0 -L
  /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib -lSystem
  This resulted in the terminal stating “command not found”. Another example of a command,
  which in fact resulted in a warning appearing on the terminal, is ld test-programs/if2.o -o
  test-programs/if2 -macosx_version_min 11.0 -L
  /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib -lSystem
  The warning this command caused the terminal to give is “no platform load command found”.
  We discussed how to potentially fix these by making sure our commands were correct
  syntactically and logically. We also found that there were a few changes to be made in the
  original compiler.rkt program and we made those changes/ added new code to ensure the
  program compiled as expected.

[ High Level Reflection ] 

  In roughly 100-500 words, write a summary of your findings in working
  on this project: what did you learn, what did you find interesting,
  what did you find challenging? As you progress in your career, it will
  be increasingly important to have technical conversations about the
  nuts and bolts of code, try to use this experience as a way to think
  about how you would approach doing group code critique. What would you
  do differently next time, what did you learn?
  
  While working on this project, we learned and applied a wide variety of programming concepts -
  such as using commands on a Mac Terminal, implementing a compiler (for IfArith), using
  NASM, studying code output, and testing and debugging code. We used the Racket coding
  concepts we learned in class this semester and ensured that we worked as a team on our project,
  thereby enhancing our teamwork skills.
  
  We found various aspects of this project interesting, like programming and debugging as a team
  rather than individually. Making use of GitHub was also fascinating as it made it feel like a
  professional programming project. It was quite interesting to use new commands in this project
  and see the outputs they produced on the Terminal. There were some challenges as well, such as
  being able to figure out what commands to use, how to fix the given code so that it produced the
  expected output, and figuring out if we were compiling and running the code correctly. We made
  use of NASM and assembly code for the first time in a project for this class, so it was a bit tricky
  to be able to use these concepts correctly here. But thankfully, we were able to overcome these
  challenges for the most part.
  
  Overall, we learned that many programming concepts are interlinked, and it was really fun to be
  able to learn and apply these in our project as a group. Maybe something we would’ve done
  differently is a little more research on the NASM part so that we’d be more comfortable using
  those commands in this project. But it was a great learning experience!

