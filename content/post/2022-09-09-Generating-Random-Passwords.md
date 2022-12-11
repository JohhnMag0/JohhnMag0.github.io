---
title: "Generating Random Passwords"
date: 2022-09-09T13:00:00-03:00
tables: [coding, technology]
---

# Generating Random Passwords

1. Introduction
2. Why Computers can't Generate Random Numbers?
3. /dev/urandom and /dev/random
4. Implementing a CLI on Linux

# 1.Introduction

I have been taking some classes at the university these last few weeks, and we started to get a grasp on the C programming language, and I really like the idea of seeing how things work under the hood. But I really think that the best way to see if you are really getting somewhere is to make some projects, and one of the things that I consider very useful but we don't have that by default on Linux is a random password generator, so I decided to create a simple CLI program that does exactly that generates random passwords. So for the rest of this article, we are going to see how this program is implemented and why I decided to make certain decisions.

# 2. Why Computers Can't Generate Random Numbers?

As you may know, a computer is basically a bloated calculator that was used by NASA to go to the moon and, not so distant by your grandmother to play candy crush. And because of this aspect of the computer, that is, it's always doing calculus of some sort, we can't generate random numbers, because if you think about it you can't pass to a calculator two values and expect a random answer (like 2+2=127), and thank god for that. Although it is very good for our computer not to return random numbers in its operations, this also ends up being a problem, because there are cases when we need this kind of thing to happen, like in cryptography, for example. Because of this, some very smart guys work on implementations that allow so-called pseudo-random numbers to be generated. Most programming languages and even the Operational System have some kind of implementation of an algorithm that generates pseudo-random numbers. In C we a have a very common function that is in the standard library, rand() and srand():

* rand(); this is the function that will return to us a random number. The source code of this function is this: 
  
  ```
  int
  rand_r (unsigned int *seed)
  {
  unsigned int next = *seed;
  int result;
  
  next *= 1103515245;
  next += 12345;
  result = (unsigned int) (next / 65536) % 2048;
  
  next *= 1103515245;
  next += 12345;
  result <<= 10;
  result ^= (unsigned int) (next / 65536) % 1024;
  
  next *= 1103515245;
  next += 12345;
  result <<= 10;
  result ^= (unsigned int) (next / 65536) % 1024;
  
  *seed = next;
  
  return result;
  }
  ```
  
  font: [What common algorithms are used for C&#39;s rand()? - Stack Overflow](https://stackoverflow.com/questions/1026327/what-common-algorithms-are-used-for-cs-rand)
  This is basically getting a seed that is a integer, and is executing some basic computational operations like sum, multipling, moving bits around and in the end of the operations it will generate a number that is so different from the seed that it "looks" random. 
  One think to in this function is that it will always generate the same set of numbers so be aware to use this as it is on production.

* srand(); this second function is actually a supplement of the other function, the 's' on srand means seed, different from rand where we don't pass any arguments here we're gonna pass a number to be or seed. Usually when we are working with this function we're gonna pass the present time as our random number, with can be done by using the 'time.h' library in C.
  As we pass a new time as our seed every time we call the program, a new random number is generated.

Although this is a good library for generating random numbers, it is not very good if you need something more robust which is the case in my password generator which requires a high entropy (ie completely different numbers from each other). As I had already said rand() will generate the same random numbers(which again is good for troubleshooting, but terrible in production), and srand despite being a way to get around this, it can cause problems if we don't give enough space between the execution of the program that is, it can pass the same seed twice generating the same password, we can't accept this kind of thing ins security.

That's why I decided to go in other route to generate the random numbers that I need on my program.

OBS.: If you are interested in the subject of generating random numbers, I will leave here three videos that talk about this subject.

* [Random Numbers (How Software Works) - YouTube](https://www.youtube.com/watch?v=aSlkVy3mbR0&list=TLPQMDkwOTIwMjIIH55doAV2ew&index=4)
* [Random Numbers with LFSR (Linear Feedback Shift Register) - Computerphile - YouTube](https://www.youtube.com/watch?v=Ks1pw1X22y4&list=TLPQMDkwOTIwMjIIH55doAV2ew&index=4)
* [NMCS4ALL: Random number generators - YouTube](https://www.youtube.com/watch?v=_tN2ev3hO14)

# 3. /dev/random and /dev/urandom

Before I get on more details of this topic, I need to say that the implementation that I'm using on my program is just one of hundreds of other ways of doing this kind of thing. If you go in a C forum you get a ton of librarys that have beautiful mathematical expressions that are used to generate random numbers, I'm just saying that is not because rand() is not the best way that you need to go my way. Even here is a wikipedia article that shows some of the algorithms you can find that do this random number generation: [List of random number generators - Wikipedia](https://en.wikipedia.org/wiki/List_of_random_number_generators)

I know that you want me to to comment the code but before that, you need to understand two files in the Linux system the random and the urandom. Both this files are tools that the OS use to generate random numbers, they are basically a pile of "noise" of the system write in a file, all this noise is collected from the driver and other sources since the boot.
You may be wondering why we have two files for this on the system? random is the first file and it is being built as the system is running, that is, it needs time to collect enough noise to be used, which can cause a call to this file to be blocked until it can return a answer.
In the case of urandom(unblocked), it stores some of the noise that already existed in the system, so it is a little less random, but it will be large enough to respond to requests that are made to it.

Let's see how this is done in my code

```c
void gendigit(int size){
    unsigned int randval;
    FILE *f;
    char list[] = NUM;
    char pass[size];

    f = fopen("/dev/urandom", "r");
    for (i=0; i<size; i++) {
        fread(&randval, sizeof(randval), 1, f);
        pass[i] = list[randval % 10];
        printf("%c", pass[i]);
    }
    fclose(f);
    printf("\n");
}
```

As you can see here, we have a function that prints a series of random digits. As you can see in the code, we have a pointer '*f' of type file. I will not go very deep on this kind of thing because pointers are really a long topic, but basically this pointer f to the file, is doing the same as your grandmother reading the menu of a restaurant, where she puts her finger on the first letter and is moving until it reads the whole word. This is exactly what is being done, the f opens the urandom file, reads one and pass this to the randval(random value) that is divided by 10, and the number that is returned is put in the array.

# 4. Implementing the CLI Program

As I have said before, this is about a CLI Program and because of that, we are going to need to work with the shell. I'm not going to explain in this article how shell works, or anything like that because for me that is just to much for one article, but I will give a brief introduction on how we can implement some functions in shell that are gonna make your CLI program a little better.

If you had already used Linux, you probably have used the terminal to do an action(even if it was just to look like a hacker to your friends), so I assume that you know if you are executing a command in the terminal and you press you complete this command. And this is a really nice feature in shell, because it not only allows you to write faster but also serves to show which commands the user can execute.

In shell we have variables that you can use or implement new ones for yourself. Shell works very much with the idea of a parent and child process, and it plays a lot with the idea of variables that only exist on the parent process or just in the child process, or coexist in both.

When you just execute a script in the way "./script.sh" only the variables that are specified in the script are executed, but if you use the command source, he executes the script in the process on which he was called, so you get all the variables that are inside and outside this script. When we source the script is like we are executing it on our current shell, this is why you always need to source your .bashrc, so your current session can get all the new information.

So let's go to the code to not confuse you anymore:

```bash
#!/bin/bash

_genp_completion()
{
    if [ "${#COMP_WORDS[@]}" != "2" ]; then
        return
    fi

    COMPREPLY=($(compgen -W "--alphanum --digit --symbol --help" -- "${COMP_WORDS[1]}"))

}

complete -F _genp_completion genp
```

This is the script that will be sourced, it will permit us to have auto-completion when executing the genp. This is basically using "complete" to call a function that verifies the number of arguments and starts some words that will be used in the autocompletion.

To implement that script you just need to go in your .bashrc and put the line:

```bash
source <dir>/genp_completion.bash
```

And voilà, you can now use <TAB> to complete the options that are allowed in your program.

Here are some useful links that give you a grasp of what was done here, and how the source command works in practice:

[linux - What is the difference between executing a Bash script vs sourcing it? - Super User](https://superuser.com/questions/176783/what-is-the-difference-between-executing-a-bash-script-vs-sourcing-it/176788#176788)

[Creating a bash completion script](https://iridakos.com/programming/2018/03/01/bash-programmable-completion-tutorial)

[How to Use Bash Programmable Completion](https://spin.atomicobject.com/2016/02/14/bash-programmable-completion/)

## END

So with that, we get to the end of this article. Here is the source code for this program on github and gitlab, as well as the guide to installing this autocompletion that I just talked about. It's a program that is working, but I know that it has a lot of room to get better, so if you see a bug or have an idea to transform the program, I'm all ears. So take care, guys, and see ya.
