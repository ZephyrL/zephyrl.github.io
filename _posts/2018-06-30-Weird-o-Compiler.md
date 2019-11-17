---
layout: post
title: A Weird Compiler and It's Visualization
date: 2018-6-30
Author: Zephyrl
categories: [Course_Project]
tags: [C, Compiler]
comments: true
---

![img](/images/woc/tree.png)

A [[Fast Access]](https://celphi-misc.github.io/woc-visualization/) to the visualization.

Weird-o-Compiler is the outcome of the course Compiler Theory which I took in my third year of bachelor's education. The work is collaborated with Yehang Yin, a reliable and respectable friend of mine. We also take the Numerical Analysis course together.

Here's another [[LINK]](https://github.com/celphi-misc/weird-o-compiler) to the source repository.

## What WoC Supports and what not?

WoC is a C-like and Javasricpt-like compiler with weird restrictions

Supports:
* functions, nested functions
* all loop kinds of C, break & continue
* branching (if & else)
* label and goto
* variable
* all logical and numerical operations of C

Not supports:
* No MACRO
* No struct & union
* No pointers
* No dynamic strong types (but have built-in types)

## What is the function of WoC? 

You could imagine WoC as something like GCC (ROFL), it can

* Compile codes writen in WoC syntax, to intermediate representation / Assembly

* Help you locate errors in your WoC program

* Create a visualization of IR tree for your code

## How to work with it?

To test and use this project, you should 

1. Build the project on your platform (Linux 64-bit suggested), 

2. Write a piece of code as if you are programming in C, but be careful with the rules

3. Run the following command to **Compile with WoC**

```
woc [file] [options]
Options:
        -a      Generating AST in JSON
        -i      Generating IR tree in JSON
        -s      Generating symbol table for scopes in JSON
        -c      Generating assemble code, in WOCASM 
        -d      Debug mode, programmer should define usage in parse.y
```

## What did we use to create WoC?

The tools are introduced and learnt via the labs of this course, basically, **Bison and YAML**. We design our tokens, and rules to interpret statements, then make use of these tools to complete the first several steps to build a compiler. As for the construction of IR tree, the re-coding to assembly, and the visualization of the tree, these are totally carried out on our own.

## Two successful and one unsuccessful example

### Example #1
```
var __var = 3;
var val = __var;

function foo() {
    var bar = val << 2;
    __var += val * bar > 0 ? __var : val >> __var;
    return bar + __var;
}

function main(argc, argv)
{
    var a = "hello world";
    function hello(para1, para2) { return null; }
    if(a == "hello") {
        for(var i = 0; i < 10; i++)
        {
            print(a);
        }
    } else if(a[0] == 'h')
    {
        print(a);
    } else {
        print("foo");
    }
    return 0;
}
```
You could simply compile this piece of code with WoC, and check the outcomes either with our [visualization tool](https://celphi-misc.github.io/woc-visualization/), or look into the output called xxx.wocasm. This example shows you that WoC shares basic syntax with C.

### Example #2 
```
function main(argc, argv) {
    function max(a, b) {
        return a > b ? a : b;
    }
    return max(1, max(2, 3));
}
```
Nested functions, C no longer supports this feature, while you could still do this in WoC, as you declare a member function for a class, noted that the function can only be used within that scope.

### Error
```
var __var = 3;
var val = __var;

function foo() {
    var bar = val << 2;
    __var += val * bar > 0 ? __var : val >> __var // Semicolon 
    return bar + __var;                           // missed
}

function main(argc, argv)
{
    var a = "hello world";
    else print(4); // Mismatched else statement
    function hello(para1, para2) { return null; }
    if(a == "hello") {
    for(var i = 0; i < 10; i++)
    {
        print(1 + 1 = 3); // Non-left-value
    }
    } else if(a[0] == 'h')
    {
        print(a);
    } else {
        print("foo") // Yet another semicolon missed
    }
    return 0;
}

```
This is similar to example 1, while it contains some mindless mistakes, WoC would help you locate them:

![img](/images/woc/error.png)

## Visualization Tool

The [Visualization Tool](https://celphi-misc.github.io/woc-visualization/), introduced by Yehang, could display a tree structure, which is represented by a JSON file. So it actually not only works for our program, you could even use it to visualize tree structured somewhere else. 

Where should the JSON file come from? Our WoC prepares this function to you, see in the options, with "-i" you would easily get it. And by clicking the red archieve button on the bottom-left corner, you could select which JSON file to load in. 


### Side Notes

You might wonder where the name of this project comes from, actually, we just thought the syntax we designed is kind of weird, so we would like to name it Weird Compiler, however then we realize the abbreviation of it is not supposed to be WC anyway, so we add an emoji like "**-o-**" between the words, which at the same time represents our emotions developing such a compiler, yarning in the midnights -- it seems not a easy task to do with anyway. Then Eventually, the compiler is call WoC.

2018-06-30