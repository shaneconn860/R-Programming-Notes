---
title: "RProgramming Swirl Lesson 1-3"
output: 
  html_document:
    keep_md: true
---

## Basic Vectors

Create a vector containing the numbers 1.1, 9, and 3.14, type c(1.1, 9, 3.14). Try it now and store the result in a variable called z.


```r
z <- c(1.1, 9, 3.14)
```

Create a new vector that contains z, 555, then z again in that order. Don't assign this
| vector to a new variable, so that we can just see the result immediately.


```r
c(z, 555, z)
```

```
## [1]   1.10   9.00   3.14 555.00   1.10   9.00   3.14
```

Numeric vectors can be used in arithmetic expressions. Type the following to see what happens: z * 2 + 100.


```r
z * 2 + 100
```

```
## [1] 102.20 118.00 106.28
```

First, R multiplied each of the three elements in z by 2. Then it added 100 to each element to get the result you see above.

Other common arithmetic operators are `+`, `-`, `/`, and `^` (where x^2 means 'x squared'). To take the square root, use the sqrt() function and to take the absolute value, use the abs() function.

Take the square root of z - 1 and assign it to a new variable called my_sqrt.


```r
my_sqrt <- sqrt(z-1)
```

Before we view the contents of the my_sqrt variable, what do you think it contains?

1: a vector of length 3

2: a single number (i.e a vector of length 1)

3: a vector of length 0 (i.e. an empty vector)

Print the contents of my_sqrt.


```r
my_sqrt
```

```
## [1] 0.3162278 2.8284271 1.4628739
```

As you may have guessed, R first subtracted 1 from each element of z, then took the square root of each element. This leaves you with a vector of the same length as the original vector z.

Now, create a new variable called my_div that gets the value of z divided by my_sqrt.


```r
my_div <- z/my_sqrt
```

Which statement do you think is true?

1: my_div is undefined

2: The first element of my_div is equal to the first element of z divided by the first element of my_sqrt, and so on...

3: my_div is a single number (i.e a vector of length 1)

Selection: 2

Go ahead and print the contents of my_div.


```r
my_div
```

```
## [1] 3.478505 3.181981 2.146460
```

When given two vectors of the same length, R simply performs the specified arithmetic operation (`+`, `-`, `*`, etc.) element-by-element. If the vectors are of different lengths, R 'recycles' the shorter vector until it is the same length as the longer vector.

When we did z * 2 + 100 in our earlier example, z was a vector of length 3, but technically 2 and 100 are each vectors of length 1.

Behind the scenes, R is 'recycling' the 2 to make a vector of 2s and the 100 to make a vector of 100s. In other words, when you ask R to compute z * 2 + 100, what it really computes is this: z * c(2, 2, 2) + c(100, 100, 100).

To see another example of how this vector 'recycling' works, try adding c(1, 2, 3, 4) and c(0, 10). Don't worry about saving the result in a new variable.


```r
c(1, 2, 3, 4) + c(0, 10)
```

```
## [1]  1 12  3 14
```

If the length of the shorter vector does not divide evenly into the length of the longer vector, R will still apply the 'recycling' method, but will throw a warning to let you know something fishy might be going on.

Try c(1, 2, 3, 4) + c(0, 10, 100) for an example.


```r
c(1, 2, 3, 4) + c(0, 10, 100)
```

```
## Warning in c(1, 2, 3, 4) + c(0, 10, 100): longer object length is not a multiple
## of shorter object length
```

```
## [1]   1  12 103   4
```

## Sequences of Numbers

The simplest way to create a sequence of numbers in R is by using the `:` operator. Type 1:20 to see how it works.


```r
1:20
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
```

That gave us every integer between (and including) 1 and 20. We could also use it to create a sequence of real numbers. For example, try pi:10.


```r
pi:10
```

```
## [1] 3.141593 4.141593 5.141593 6.141593 7.141593 8.141593 9.141593
```

The result is a vector of real numbers starting with pi (3.142...) and increasing in increments of 1. The upper limit of 10 is never reached, since the next number in our sequence would be greater than 10.

What happens if we do 15:1? Give it a try to find out.


```r
15:1
```

```
##  [1] 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1
```

It counted backwards in increments of 1! It's unlikely we'd want this behavior, but nonetheless it's good to know how it could happen.

Remember that if you have questions about a particular R function, you can access its documentation with a question mark followed by the function name: ?function_name_here. However, in the case of an operator like the colon used above, you must enclose the symbol in backticks like this: ?`:`. (NOTE: The backtick (`) key is generally located in the top left corner of a keyboard, above the Tab key. If you don't have a backtick key, you can use regular quotes.)

Pull up the documentation for `:` now.


```r
?`:`
```

Often, we'll desire more control over a sequence we're creating than what the `:` operator gives us. The seq() function serves this purpose.

The most basic use of seq() does exactly the same thing as the `:` operator. Try seq(1, 20) to see this.


```r
seq(1, 20)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
```

This gives us the same output as 1:20. However, let's say that instead we want a vector of numbers ranging from 0 to 10, incremented by 0.5. seq(0, 10, by=0.5) does just that. Try it out.


```r
seq(0, 10, by=0.5)
```

```
##  [1]  0.0  0.5  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0  6.5  7.0
## [16]  7.5  8.0  8.5  9.0  9.5 10.0
```

Or maybe we don't care what the increment is and we just want a sequence of 30 numbers between 5 and 10. seq(5, 10, length=30) does the trick. Give it a shot now and store the result in a new variable called my_seq.


```r
my_seq <- seq(5, 10, length=30)
```

To confirm that my_seq has length 30, we can use the length() function. Try it now.


```r
length(my_seq)
```

```
## [1] 30
```

Let's pretend we don't know the length of my_seq, but we want to generate a sequence of integers from 1 to N, where N represents the length of the my_seq vector. In other words, we want a new vector (1, 2, 3, ...) that is the same length as my_seq.

There are several ways we could do this. One possibility is to combine the `:` operator and the length() function like this: 1:length(my_seq). Give that a try.


```r
1:length(my_seq)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
## [26] 26 27 28 29 30
```

Another option is to use seq(along.with = my_seq). Give that a try.


```r
seq(along.with = my_seq)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
## [26] 26 27 28 29 30
```

However, as is the case with many common tasks, R has a separate built-in function for this purpose called seq_along(). Type seq_along(my_seq) to see it in action.


```r
seq_along(my_seq)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
## [26] 26 27 28 29 30
```

There are often several approaches to solving the same problem, particularly in R. Simple approaches that involve less typing are generally best. It's also important for your code to be readable, so that you and others can figure out what's going on without too much hassle.

If R has a built-in function for a particular task, it's likely that function is highly optimized for that purpose and is your best option. As you become a more advanced R programmer, you'll design your own functions to perform tasks when there are no better options. We'll explore writing your own functions in future lessons.

One more function related to creating sequences of numbers is rep(), which stands for 'replicate'. Let's look at a few uses.

If we're interested in creating a vector that contains 40 zeros, we can use rep(0, times = 40). Try it out.


```r
rep(0, times=40)
```

```
##  [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [39] 0 0
```

If instead we want our vector to contain 10 repetitions of the vector (0, 1, 2), we can do rep(c(0, 1, 2), times = 10). Go ahead.


```r
rep(c(0, 1, 2), times = 10)
```

```
##  [1] 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2
```

Finally, let's say that rather than repeating the vector (0, 1, 2) over and over again, we want our vector to contain 10 zeros, then 10 ones, then 10 twos. We can do this with the `each` argument. Try rep(c(0, 1, 2), each = 10).


```r
rep(c(0, 1, 2), each = 10)
```

```
##  [1] 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2
```

## Workspace and Files

List all the files in your working directory using list.files() or dir().


```r
dir()
```

```
##  [1] "flag_data.csv"             "mytest2.R"                
##  [3] "mytest3.R"                 "R-Programming-Notes.Rproj"
##  [5] "README.md"                 "RProg1-3.html"            
##  [7] "RProg1-3.Rmd"              "RProg1.html"              
##  [9] "RProg10-12.html"           "RProg10-12.Rmd"           
## [11] "RProg13-15.html"           "RProg13-15.Rmd"           
## [13] "RProg4-6.html"             "RProg4-6.Rmd"             
## [15] "RProg7-9.html"             "RProg7-9.Rmd"             
## [17] "testdir"                   "testdir2"
```

Use the args() function to determine the arguments to list.files().


```r
args(list.files)
```

```
## function (path = ".", pattern = NULL, all.files = FALSE, full.names = FALSE, 
##     recursive = FALSE, ignore.case = FALSE, include.dirs = FALSE, 
##     no.. = FALSE) 
## NULL
```

Assign the value of the current working directory to a variable called "old.dir".


```r
old.dir <- getwd()
```

We will use old.dir at the end of this lesson to move back to the place that we started. A lot of query functions like getwd() have the useful property that they return the answer to the question as a result of the function.

Use dir.create() to create a directory in the current working directory called "testdir".


```r
dir.create("testdir")
```

```
## Warning in dir.create("testdir"): 'testdir' already exists
```

We will do all our work in this new directory and then delete it after we are done. This is the R analog to "Take only pictures, leave only footprints."

Set your working directory to "testdir" with the setwd() command.


```r
setwd("testdir")
```

Create a file in your working directory called "mytest.R" using the file.create() function.


```r
file.create("mytest.R")
```

```
## [1] TRUE
```

This should be the only file in this newly created directory. Let's check this by listing all the files in the current directory.


```r
dir()
```

```
##  [1] "flag_data.csv"             "mytest.R"                 
##  [3] "mytest2.R"                 "mytest3.R"                
##  [5] "R-Programming-Notes.Rproj" "README.md"                
##  [7] "RProg1-3.html"             "RProg1-3.Rmd"             
##  [9] "RProg1.html"               "RProg10-12.html"          
## [11] "RProg10-12.Rmd"            "RProg13-15.html"          
## [13] "RProg13-15.Rmd"            "RProg4-6.html"            
## [15] "RProg4-6.Rmd"              "RProg7-9.html"            
## [17] "RProg7-9.Rmd"              "testdir"                  
## [19] "testdir2"
```

Check to see if "mytest.R" exists in the working directory using the file.exists() function.


```r
file.exists("mytest.R")
```

```
## [1] TRUE
```

These sorts of functions are excessive for interactive use. But, if you are running a program that loops through a series of files and does some processing on each one, you will want to check to see that each exists before you try to process it.

Access information about the file "mytest.R" by using file.info().


```r
file.info("mytest.R")
```

```
##          size isdir mode               mtime               ctime
## mytest.R    0 FALSE  644 2020-04-18 14:54:05 2020-04-18 14:54:05
##                        atime uid gid            uname grname
## mytest.R 2020-04-18 14:54:05 501  20 shaneconnaughton  staff
```

You can use the $ operator --- e.g., file.info("mytest.R")$mode --- to grab specific items.

Change the name of the file "mytest.R" to "mytest2.R" by using file.rename().


```r
file.rename("mytest.R", "mytest2.R")
```

```
## [1] TRUE
```

Your operating system will provide simpler tools for these sorts of tasks, but having the ability to manipulate files programatically is useful. You might now try to delete mytest.R using file.remove('mytest.R'), but that won't work since mytest.R no longer exists. You have already renamed it.

Make a copy of "mytest2.R" called "mytest3.R" using file.copy().


```r
file.copy("mytest2.R", "mytest3.R")
```

```
## [1] FALSE
```

You now have two files in the current directory. That may not seem very interesting. But what if you were working with dozens, or millions, of individual files? In that case, being able to programatically act on many files would be absolutely necessary. Don't forget that you can, temporarily, leave the lesson by typing play() and then return by typing nxt().

Provide the relative path to the file "mytest3.R" by using file.path().


```r
file.path("mytest3.R")
```

```
## [1] "mytest3.R"
```

You can use file.path to construct file and directory paths that are independent of the operating system your R code is running on. Pass 'folder1' and 'folder2' as arguments to file.path to make a platform-independent pathname.


```r
file.path("folder1", "folder2")
```

```
## [1] "folder1/folder2"
```

Create a directory in the current working directory called "testdir2" and a subdirectory for it called "testdir3", all in one command by using dir.create() and file.path().


```r
dir.create(file.path('testdir2', 'testdir3'), recursive = TRUE)
```

```
## Warning in dir.create(file.path("testdir2", "testdir3"), recursive = TRUE):
## 'testdir2/testdir3' already exists
```

 
