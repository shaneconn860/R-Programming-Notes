---
title: "RProgramming Swirl Lesson 4-6"
output: 
  html_document:
    keep_md: true
---

## Vectors

The simplest and most common data structure in R is the vector.

Vectors come in two different flavors: atomic vectors and lists. An atomic vector contains exactly one data type, whereas a list may contain multiple data types. We'll explore atomic vectors further before we get to lists.

In previous lessons, we dealt entirely with numeric vectors, which are one type of atomic vector. Other types of atomic vectors include logical, character, integer, and complex. In this lesson, we'll take a closer look at logical and character vectors.

Logical vectors can contain the values TRUE, FALSE, and NA (for 'not available'). These values are generated as the result of logical 'conditions'. Let's experiment with some simple conditions.

First, create a numeric vector num_vect that contains the values 0.5, 55, -10, and 6.


```r
num_vect <- c(0.5, 55,-10, 6)
```

Now, create a variable called tf that gets the result of num_vect < 1, which is read as 'num_vect is less than 1'.


```r
tf <- num_vect < 1
```

What do you think tf will look like?

1: a vector of 4 logical values

2: a single logical value

**Selection: 1**

Print the contents of tf now.


```r
tf
```

```
## [1]  TRUE FALSE  TRUE FALSE
```

The statement num_vect < 1 is a condition and tf tells us whether each corresponding element of our numeric vector num_vect satisfies this condition.

The first element of num_vect is 0.5, which is less than 1 and therefore the statement 0.5 < 1 is TRUE. The second element of num_vect is 55, which is greater than 1, so the statement 55 < 1 is FALSE. The same logic applies for the third and fourth elements.

Let's try another. Type num_vect >= 6 without assigning the result to a new variable.


```r
num_vect >= 6
```

```
## [1] FALSE  TRUE FALSE  TRUE
```

This time, we are asking whether each individual element of num_vect is greater than OR equal to 6. Since only 55 and 6 are greater than or equal to 6, the second and fourth elements of the result are TRUE and the first and third elements are FALSE.

The `<` and `>=` symbols in these examples are called 'logical operators'. Other logical operators include `>`, `<=`, `==` for exact equality, and `!=` for inequality.

If we have two logical expressions, A and B, we can ask whether at least one is TRUE with A | B (logical 'or' a.k.a. 'union') or whether they are both TRUE with A & B (logical 'and' a.k.a. 'intersection'). Lastly, !A is the negation of A and is TRUE when A is FALSE and vice versa.

It's a good idea to spend some time playing around with various combinations of these logical operators until you get comfortable with their use. We'll do a few examples here to get you started.

Try your best to predict the result of each of the following statements. You can use pencil and paper to work them out if it's helpful. If you get stuck, just guess and you've got a 50% chance of getting the right answer!
        
(3 > 5) & (4 == 4)

1: FALSE

2: TRUE

**Selection: 1**

(TRUE == TRUE) | (TRUE == FALSE)

1: FALSE

2: TRUE

**Selection: 2**

((111 >= 111) | !(TRUE)) & ((4 + 1) == 5)

1: FALSE

2: TRUE

**Selection: 2**

Don't worry if you found these to be tricky. They're supposed to be. Working with logical statements in R takes practice, but your efforts will be rewarded in future lessons (e.g. subsetting and control structures).

Character vectors are also very common in R. Double quotes are used to distinguish character objects, as in the following example.

Create a character vector that contains the following words: "My", "name", "is". Remember to enclose each word in its own set of double quotes, so that R knows they are character strings. Store the vector in a variable called my_char.


```r
my_char <- c("My", "name", "is")
```

Print the contents of my_char to see what it looks like.


```r
my_char
```

```
## [1] "My"   "name" "is"
```

Right now, my_char is a character vector of length 3. Let's say we want to join the elements of my_char together into one continuous character string (i.e. a character vector of length 1). We can do this using the paste() function.

Type paste(my_char, collapse = " ") now. Make sure there's a space between the double quotes in the `collapse` argument. You'll see why in a second.


```r
paste(my_char, collapse = " ")
```

```
## [1] "My name is"
```

The `collapse` argument to the paste() function tells R that when we join together the elements of the my_char character vector, we'd like to separate them with single spaces.

It seems that we're missing something.... Ah, yes! Your name!

To add (or 'concatenate') your name to the end of my_char, use the c() function like this: c(my_char, "your_name_here"). Place your name in double quotes where I've put "your_name_here". Try it now, storing the result in a new variable called my_name.


```r
my_name <- c(my_char, "Billy")
```


```r
my_name
```

```
## [1] "My"    "name"  "is"    "Billy"
```

Now, use the paste() function once more to join the words in my_name together into a single character string. Don't forget to say collapse = " "!
        

```r
paste(my_name, collapse = " ")
```

```
## [1] "My name is Billy"
```

In this example, we used the paste() function to collapse the elements of a single character vector. paste() can also be used to join the elements of multiple character vectors.

In the simplest case, we can join two character vectors that are each of length 1 (i.e. join two words). Try paste("Hello", "world!", sep = " "), where the `sep` argument tells R that we want to separate the joined elements with a single space.


```r
paste("Hello", "world!", sep = " ")
```

```
## [1] "Hello world!"
```

For a slightly more complicated example, we can join two vectors, each of length 3. Use paste() to join the integer vector 1:3 with the character vector c("X", "Y", "Z"). This time, use sep = "" to leave no space between the joined elements.


```r
paste(1:3, c("X", "Y", "Z"), sep="")
```

```
## [1] "1X" "2Y" "3Z"
```

What do you think will happen if our vectors are of different length? (Hint: we talked about this in a previous lesson.)

Vector recycling! Try paste(LETTERS, 1:4, sep = "-"), where LETTERS is a predefined variable in R containing a character vector of all 26 letters in the English alphabet.


```r
paste(LETTERS, 1:4, sep = "-")
```

```
##  [1] "A-1" "B-2" "C-3" "D-4" "E-1" "F-2" "G-3" "H-4" "I-1" "J-2" "K-3" "L-4"
## [13] "M-1" "N-2" "O-3" "P-4" "Q-1" "R-2" "S-3" "T-4" "U-1" "V-2" "W-3" "X-4"
## [25] "Y-1" "Z-2"
```

Since the character vector LETTERS is longer than the numeric vector 1:4, R simply recycles, or repeats, 1:4 until it matches the length of LETTERS.

Also worth noting is that the numeric vector 1:4 gets 'coerced' into a character vector by the paste() function.

We'll discuss coercion in another lesson, but all it really means is that the numbers 1, 2, 3, and 4 in the output above are no longer numbers to R, but rather characters "1", "2", "3", and "4".

## Missing Values

Missing values play an important role in statistics and data analysis. Often, missing values must not be ignored, but rather they should be carefully studied to see if there's an underlying pattern or cause for their missingness.

In R, NA is used to represent any value that is 'not available' or 'missing' (in the statistical sense). In this lesson, we'll explore missing values further.

Any operation involving NA generally yields NA as the result. To illustrate, let's create a vector c(44, NA, 5, NA) and assign it to a variable x.


```r
x <- c(44, NA, 5, NA)
```

Now, let's multiply x by 3.


```r
x*3
```

```
## [1] 132  NA  15  NA
```

Notice that the elements of the resulting vector that correspond with the NA values in x are also NA.

To make things a little more interesting, lets create a vector containing 1000 draws from a standard normal distribution with y <- rnorm(1000).


```r
y <- rnorm(1000)
```

Next, let's create a vector containing 1000 NAs with z <- rep(NA, 1000).


```r
z <- rep(NA, 1000)
```

Finally, let's select 100 elements at random from these 2000 values (combining y and z) such that we don't know how many NAs we'll wind up with or what positions they'll occupy in our final vector -- my_data <- sample(c(y, z), 100).


```r
my_data <- sample(c(y, z), 100)
```

Let's first ask the question of where our NAs are located in our data. The is.na() function tells us whether each element of a vector is NA. Call is.na() on my_data and assign the result to my_na.


```r
my_na <- is.na(my_data)
```

Now, print my_na to see what you came up with.


```r
my_na
```

```
##   [1]  TRUE  TRUE FALSE FALSE FALSE  TRUE  TRUE FALSE  TRUE FALSE FALSE  TRUE
##  [13] FALSE  TRUE  TRUE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE  TRUE  TRUE
##  [25] FALSE FALSE  TRUE  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE  TRUE
##  [37] FALSE FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE FALSE FALSE  TRUE FALSE
##  [49] FALSE FALSE  TRUE FALSE  TRUE  TRUE FALSE  TRUE FALSE  TRUE  TRUE  TRUE
##  [61] FALSE  TRUE FALSE FALSE  TRUE FALSE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE
##  [73] FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE  TRUE FALSE FALSE FALSE  TRUE
##  [85]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE  TRUE
##  [97]  TRUE  TRUE  TRUE  TRUE
```

Everywhere you see a TRUE, you know the corresponding element of my_data is NA. Likewise, everywhere you see a FALSE, you know the corresponding element of my_data is one of our random draws from the standard normal distribution.

In our previous discussion of logical operators, we introduced the `==` operator as a method of testing for equality between two objects. So, you might think the expression my_data == NA yields the same results as is.na(). Give it a try.


```r
my_data == NA
```

```
##   [1] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
##  [26] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
##  [51] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
##  [76] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
```

The reason you got a vector of all NAs is that NA is not really a value, but just a placeholder for a quantity that is not available. Therefore the logical expression is incomplete and R has no choice but to return a vector of the same length as my_data that contains all NAs.

Don't worry if that's a little confusing. The key takeaway is to be cautious when using logical expressions anytime NAs might creep in, since a single NA value can derail the entire thing.

So, back to the task at hand. Now that we have a vector, my_na, that has a TRUE for every NA and FALSE for every numeric value, we can compute the total number of NAs in our data.

The trick is to recognize that underneath the surface, R represents TRUE as the number 1 and FALSE as the number 0. Therefore, if we take the sum of a bunch of TRUEs and FALSEs, we get the total number of TRUEs.

Let's give that a try here. Call the sum() function on my_na to count the total number of TRUEs in my_na, and thus the total number of NAs in my_data. Don't assign the result to a new variable.


```r
sum(my_na)
```

```
## [1] 54
```

Pretty cool, huh? Finally, let's take a look at the data to convince ourselves that everything 'adds up'. Print my_data to the console.


```r
my_data
```

```
##   [1]          NA          NA  0.95107088  0.51737317  0.57984505          NA
##   [7]          NA  0.60311652          NA  1.70734309 -0.65550960          NA
##  [13] -1.17506875          NA          NA  0.58127315  1.73622088 -2.51894142
##  [19]          NA  0.63274519 -0.28506689  0.24900674          NA          NA
##  [25] -0.53181164  0.57750565          NA          NA -1.66494424 -1.07582888
##  [31]          NA          NA          NA  0.92905639 -1.55468176          NA
##  [37] -0.01358975  0.14747461 -0.35564793          NA          NA -0.25069162
##  [43]          NA          NA  0.87324211 -1.31787194          NA -0.56565990
##  [49]  0.86814036  0.70598709          NA  0.87666619          NA          NA
##  [55]  0.66653603          NA  1.14348211          NA          NA          NA
##  [61] -0.26335206          NA  0.75952208 -1.10683588          NA  2.14115941
##  [67]          NA          NA -0.67053004          NA          NA          NA
##  [73]  0.63232854 -0.65810984  1.53672339  0.74814952          NA  1.34002592
##  [79]          NA          NA -0.56383168 -0.01033865 -0.44981743          NA
##  [85]          NA          NA          NA          NA          NA          NA
##  [91]          NA  0.37401959  0.73080214          NA          NA          NA
##  [97]          NA          NA          NA          NA
```

Now that we've got NAs down pat, let's look at a second type of missing value -- NaN, which stands for 'not a number'. To generate NaN, try dividing (using a forward slash) 0 by 0 now.


```r
0/0
```

```
## [1] NaN
```

Let's do one more, just for fun. In R, Inf stands for infinity. What happens if you subtract Inf from Inf?


```r
Inf-Inf
```

```
## [1] NaN
```

## Subsetting Vectors

In this lesson, we'll see how to extract elements from a vector based on some conditions that we specify.

For example, we may only be interested in the first 20 elements of a vector, or only the elements that are not NA, or only those that are positive or correspond to a specific variable of interest. By the end of this lesson, you'll know how to handle each of these scenarios.

I've created for you a vector called x that contains a random ordering of 20 numbers (from a standard normal distribution) and 20 NAs. Type x now to see what it looks like.


```r
x <- rnorm(n=40)
y <- which(x %in% sample(x, 20))
x[y] <- NA
```


```r
x
```

```
##  [1] -0.73063446          NA -1.16753808  0.14637887          NA          NA
##  [7]          NA -0.49591811 -0.05857883  0.65473910  0.53330157          NA
## [13]          NA  1.22492538 -0.52732965          NA  0.02512317          NA
## [19]          NA          NA  0.21245589          NA -0.73669234          NA
## [25]  0.37744541          NA -0.45367206          NA          NA -0.89876120
## [31]          NA          NA -0.48343491 -0.27182534 -0.91219286          NA
## [37] -0.65748730          NA          NA  0.55271879
```

The way you tell R that you want to select some particular elements (i.e. a 'subset') from a vector is by placing an 'index vector' in square brackets immediately following the name of the vector.

For a simple example, try x[1:10] to view the first ten elements of x.


```r
x[1:10]
```

```
##  [1] -0.73063446          NA -1.16753808  0.14637887          NA          NA
##  [7]          NA -0.49591811 -0.05857883  0.65473910
```

Index vectors come in four different flavors -- logical vectors, vectors of positive integers, vectors of negative integers, and vectors of character strings -- each of which we'll cover in this lesson.

Let's start by indexing with logical vectors. One common scenario when working with real-world data is that we want to extract all elements of a vector that are not NA (i.e. missing data). Recall that is.na(x) yields a vector of logical values the same length as x, with TRUEs corresponding to NA values in x and FALSEs corresponding to non-NA values in x.

What do you think x[is.na(x)] will give you?

1: A vector of length 0

2: A vector of all NAs

3: A vector of TRUEs and FALSEs

4: A vector with no NAs

**Selection: 2**

Prove it to yourself by typing x[is.na(x)].


```r
x[is.na(x)]
```

```
##  [1] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
```

Recall that `!` gives us the negation of a logical expression, so !is.na(x) can be read as 'is not NA'. Therefore, if we want to create a vector called y that contains all of the non-NA values from x, we can use y <- x[!is.na(x)]. Give it a try.


```r
y <- x[!is.na(x)]
```

Print y to the console.


```r
y
```

```
##  [1] -0.73063446 -1.16753808  0.14637887 -0.49591811 -0.05857883  0.65473910
##  [7]  0.53330157  1.22492538 -0.52732965  0.02512317  0.21245589 -0.73669234
## [13]  0.37744541 -0.45367206 -0.89876120 -0.48343491 -0.27182534 -0.91219286
## [19] -0.65748730  0.55271879
```

Now that we've isolated the non-missing values of x and put them in y, we can subset y as we please.

Recall that the expression y > 0 will give us a vector of logical values the same length as y, with TRUEs corresponding to values of y that are greater than zero and FALSEs corresponding to values of y that are less than or equal to zero. What do you think y[y > 0] will give you?

1: A vector of all the positive elements of y

2: A vector of all NAs

3: A vector of length 0

4: A vector of TRUEs and FALSEs

5: A vector of all the negative elements of y

**Selection: 1**

Type y[y > 0] to see that we get all of the positive elements of y, which are also the positive elements of our original vector x.


```r
y[y > 0]
```

```
## [1] 0.14637887 0.65473910 0.53330157 1.22492538 0.02512317 0.21245589 0.37744541
## [8] 0.55271879
```

You might wonder why we didn't just start with x[x > 0] to isolate the positive elements of x. Try that now to see why.


```r
x[x > 0]
```

```
##  [1]         NA 0.14637887         NA         NA         NA 0.65473910
##  [7] 0.53330157         NA         NA 1.22492538         NA 0.02512317
## [13]         NA         NA         NA 0.21245589         NA         NA
## [19] 0.37744541         NA         NA         NA         NA         NA
## [25]         NA         NA         NA 0.55271879
```

Since NA is not a value, but rather a placeholder for an unknown quantity, the expression NA > 0 evaluates to NA. Hence we get a bunch of NAs mixed in with our positive numbers when we do this.

Combining our knowledge of logical operators with our new knowledge of subsetting, we could do this -- x[!is.na(x) & x > 0]. Try it out.


```r
x[!is.na(x) & x > 0]
```

```
## [1] 0.14637887 0.65473910 0.53330157 1.22492538 0.02512317 0.21245589 0.37744541
## [8] 0.55271879
```

In this case, we request only values of x that are both non-missing AND greater than zero.

I've already shown you how to subset just the first ten values of x using x[1:10]. In this case, we're providing a vector of positive integers inside of the square brackets, which tells R to return only the elements of x numbered 1 through 10.

Many programming languages use what's called 'zero-based indexing', which means that the first element of a vector is considered element 0. R uses 'one-based indexing', which (you guessed it!) means the first element of a vector is considered element 1.

Can you figure out how we'd subset the 3rd, 5th, and 7th elements of x? Hint -- Use the c() function to specify the element numbers as a numeric vector.


```r
x[c(3, 5, 7)]
```

```
## [1] -1.167538        NA        NA
```

It's important that when using integer vectors to subset our vector x, we stick with the set of indexes {1, 2, ..., 40} since x only has 40 elements. What happens if we ask for the zeroth element of x (i.e. x[0])? Give it a try.


```r
x[0]
```

```
## numeric(0)
```

As you might expect, we get nothing useful. Unfortunately, R doesn't prevent us from doing this. What if we ask for the 3000th element of x? Try it out.


```r
x[3000]
```

```
## [1] NA
```

Again, nothing useful, but R doesn't prevent us from asking for it. This should be a cautionary tale. You should always make sure that what you are asking for is within the bounds of the vector you're working with.

What if we're interested in all elements of x EXCEPT the 2nd and 10th? It would be pretty tedious to construct a vector containing all numbers 1 through 40 EXCEPT 2 and 10.

Luckily, R accepts negative integer indexes. Whereas x[c(2, 10)] gives us ONLY the 2nd and 10th elements of x, x[c(-2, -10)] gives us all elements of x EXCEPT for the 2nd and 10 elements.  Try x[c(-2, -10)] now to see this.


```r
x[c(-2, -10)]
```

```
##  [1] -0.73063446 -1.16753808  0.14637887          NA          NA          NA
##  [7] -0.49591811 -0.05857883  0.53330157          NA          NA  1.22492538
## [13] -0.52732965          NA  0.02512317          NA          NA          NA
## [19]  0.21245589          NA -0.73669234          NA  0.37744541          NA
## [25] -0.45367206          NA          NA -0.89876120          NA          NA
## [31] -0.48343491 -0.27182534 -0.91219286          NA -0.65748730          NA
## [37]          NA  0.55271879
```

A shorthand way of specifying multiple negative numbers is to put the negative sign out in front of the vector of positive numbers. Type x[-c(2, 10)] to get the exact same result.


```r
x[-c(2, 10)]
```

```
##  [1] -0.73063446 -1.16753808  0.14637887          NA          NA          NA
##  [7] -0.49591811 -0.05857883  0.53330157          NA          NA  1.22492538
## [13] -0.52732965          NA  0.02512317          NA          NA          NA
## [19]  0.21245589          NA -0.73669234          NA  0.37744541          NA
## [25] -0.45367206          NA          NA -0.89876120          NA          NA
## [31] -0.48343491 -0.27182534 -0.91219286          NA -0.65748730          NA
## [37]          NA  0.55271879
```

So far, we've covered three types of index vectors -- logical, positive integer, and negative integer. The only remaining type requires us to introduce the concept of 'named' elements.

Create a numeric vector with three named elements using vect <- c(foo = 11, bar = 2, norf = NA).


```r
vect <- c(foo = 11, bar = 2, norf = NA)
```

When we print vect to the console, you'll see that each element has a name. Try it out.


```r
vect
```

```
##  foo  bar norf 
##   11    2   NA
```

We can also get the names of vect by passing vect as an argument to the names() function. Give that a try.


```r
names(vect)
```

```
## [1] "foo"  "bar"  "norf"
```

Alternatively, we can create an unnamed vector vect2 with c(11, 2, NA). Do that now.


```r
vect2 <- c(11, 2, NA)
```

Then, we can add the `names` attribute to vect2 after the fact with names(vect2) <- c("foo", "bar", "norf"). Go ahead.


```r
names(vect2) <- c("foo", "bar", "norf")
```

Now, let's check that vect and vect2 are the same by passing them as arguments to the identical() function.


```r
identical(vect, vect2)
```

```
## [1] TRUE
```

Indeed, vect and vect2 are identical named vectors.

Now, back to the matter of subsetting a vector by named elements. Which of the following commands do you think would give us the second element of vect?

1: vect[bar]

2: vect["bar"]

3: vect["2"]

**Selection: 2**


```r
vect["bar"]
```

```
## bar 
##   2
```

Likewise, we can specify a vector of names with vect[c("foo", "bar")]. Try it out.


```r
vect[c("foo", "bar")]
```

```
## foo bar 
##  11   2
```

Now you know all four methods of subsetting data from vectors. Different approaches are best in different scenarios and when in doubt, try it out!
