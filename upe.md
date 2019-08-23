# THE UNIX PROGRAMMING ENVIRONMENT

- this is an old book but it's held in high esteem by many. Seeing how many linux commands resemble those of unix and having enjoyed the C book, co-written by Kernighan, I thought I'd skim over this one and see what it still has to offer.
- These are my notes on what I understand of the book and what I manage to read of it.
- I will use macOS 10.x to test the scripts provided in the book and probably report if they are still relevant.

## PREFACE/TRIVIA:
- Unix was created in 1969 in Bell Labs and rewritten in C in 1973. In 1974, it was licensed to universities (the so called BSD).
- Unix is successful because of its portability, the fact that it's written in C, a high level language. 
- The **Unix philosophy** states that the relationship among different programs is what makes a great system. Even trivial programs can be combined to create great software, if they work well together.
- An important theme of the book, is how programs work together and how they are used to create other programs, and how they fit in the environment where they run.
- Use existing unix tools instead of writing programs that the same task clumsily. 

## CHAPTER 1: UNIX FOR BEGINNERS
- Unix is a time sharing operating system _kernel_. A **kernel** is program that let users run their programs, controls resources and provides a file system. Time sharing means that the the use of the CPU is rapidly switched among different users and programs that seem to be running simultaneously. 
- Unix can also refer to the kernel along with other system tools such as editors, compilers, command languages.. etc.

### 1.1 Getting started:
- Unix is _full duplex_, meaning when you type on the keyboard, characters are sent to the system which _echoes_ them (send them back) to the screen. This echoing functionality is turned off when you type a password, for example. 
- Most keyboard characters are printing characters, but there are also control characters such as the **RETURN** key which signifies the end of input line. Control characters are invisible characters that control some aspect on the terminal.
- Return has a key of its own, but most other control characters are typed by holding the control (CTL/CTRL) character and some other character. RETURN, for example, is is composed of ctl-m, backspace is ctl-h..etc (I don't know if these)







