---
title: "RProgramming Swirl Lesson 7-9"
output: 
  html_document:
    keep_md: true
---

## Matrices and Data Frames

In this lesson, we'll cover matrices and data frames. Both represent 'rectangular' data types, meaning that they are used to store tabular data, with rows and columns.

The main difference, as you'll see, is that matrices can only contain a single class of data, while data frames can consist of many different classes of data.

Let's create a vector containing the numbers 1 through 20 using the `:` operator. Store the result in a variable called my_vector.

Also, remember that you don't need the c() function when using `:`.


```r
my_vector <- 1:20
```

View the contents of the vector you just created.


```r
my_vector
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
```

The dim() function tells us the 'dimensions' of an object. What happens if we do dim(my_vector)? Give it a try.


```r
dim(my_vector)
```

```
## NULL
```

Clearly, that's not very helpful! Since my_vector is a vector, it doesn't have a `dim` attribute (so it's just NULL), but we can find its length using the length() function. Try that now.


```r
length(my_vector)
```

```
## [1] 20
```

Ah! That's what we wanted. But, what happens if we give my_vector a `dim` attribute? Let's give it a try. Type dim(my_vector) <- c(4, 5).


```r
dim(my_vector) <- c(4, 5)
```

It's okay if that last command seemed a little strange to you. It should! The dim() function allows you to get OR set the `dim` attribute for an R object. In this case, we assigned the value c(4, 5) to the `dim` attribute of my_vector.

Use dim(my_vector) to confirm that we've set the `dim` attribute correctly.


```r
dim(my_vector)
```

```
## [1] 4 5
```

Another way to see this is by calling the attributes() function on my_vector. Try it now.


```r
attributes(my_vector)
```

```
## $dim
## [1] 4 5
```

Just like in math class, when dealing with a 2-dimensional object (think rectangular table), the first number is the number of rows and the second is the number of columns. Therefore, we just gave my_vector 4 rows and 5 columns.

But, wait! That doesn't sound like a vector any more. Well, it's not. Now it's a matrix. View the contents of my_vector now to see what it looks like.


```r
my_vector
```

```
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    1    5    9   13   17
## [2,]    2    6   10   14   18
## [3,]    3    7   11   15   19
## [4,]    4    8   12   16   20
```

Now, let's confirm it's actually a matrix by using the class() function. Type class(my_vector) to see what I mean.


```r
class(my_vector)
```

```
## [1] "matrix"
```

Sure enough, my_vector is now a matrix. We should store it in a new variable that helps us remember what it is. Store the value of my_vector in a new variable called my_matrix.


```r
my_matrix <- my_vector
```

The example that we've used so far was meant to illustrate the point that a matrix is simply an atomic vector with a dimension attribute. A more direct method of creating the same matrix uses the matrix() function.

Bring up the help file for the matrix() function now using the `?` function.


```r
?matrix
```

Now, look at the documentation for the matrix function and see if you can figure out how to create a matrix containing the same numbers (1-20) and dimensions (4 rows, 5 columns) by calling the matrix() function. Store the result in a variable called my_matrix2.


```r
my_matrix2 <- matrix(data=1:20, nrow=4, ncol=5)
```

Finally, let's confirm that my_matrix and my_matrix2 are actually identical. The identical() function will tell us if its first two arguments are the same. Try it out.


```r
identical(my_matrix, my_matrix2)
```

```
## [1] TRUE
```

Now, imagine that the numbers in our table represent some measurements from a clinical experiment, where each row represents one patient and each column represents one variable for which measurements were taken.

We may want to label the rows, so that we know which numbers belong to each patient in the experiment. One way to do this is to add a column to the matrix, which contains the names of all four people.

Let's start by creating a character vector containing the names of our patients -- Bill, Gina, Kelly, and Sean. Remember that double quotes tell R that something is a character string. Store the result in a variable called patients.


```r
patients <- c("Bill", "Gina", "Kelly", "Sean")
```

Now we'll use the cbind() function to 'combine columns'. Don't worry about storing the result in a new variable. Just call cbind() with two arguments -- the patients vector and my_matrix.


```r
cbind(patients, my_matrix)
```

```
##      patients                       
## [1,] "Bill"   "1" "5" "9"  "13" "17"
## [2,] "Gina"   "2" "6" "10" "14" "18"
## [3,] "Kelly"  "3" "7" "11" "15" "19"
## [4,] "Sean"   "4" "8" "12" "16" "20"
```

Something is fishy about our result! It appears that combining the character vector with our matrix of numbers caused everything to be enclosed in double quotes. This means we're left with a matrix of character strings, which is no good.

If you remember back to the beginning of this lesson, I told you that matrices can only contain ONE class of data. Therefore, when we tried to combine a character vector with a numeric matrix, R was forced to 'coerce' the numbers to characters, hence the double quotes.

This is called 'implicit coercion', because we didn't ask for it. It just happened. But why didn't R just convert the names of our patients to numbers? I'll let you ponder that question on your own.

So, we're still left with the question of how to include the names of our patients in the table without destroying the integrity of our numeric data. Try the following -- my_data <- data.frame(patients, my_matrix)


```r
my_data <- data.frame(patients, my_matrix)
```

Now view the contents of my_data to see what we've come up with.


```r
my_data
```

```
##   patients X1 X2 X3 X4 X5
## 1     Bill  1  5  9 13 17
## 2     Gina  2  6 10 14 18
## 3    Kelly  3  7 11 15 19
## 4     Sean  4  8 12 16 20
```

It looks like the data.frame() function allowed us to store our character vector of names right alongside our matrix of numbers. That's exactly what we were hoping for!

Behind the scenes, the data.frame() function takes any number of arguments and returns a single object of class `data.frame` that is composed of the original objects.

Let's confirm this by calling the class() function on our newly created data frame.


```r
class(my_data)
```

```
## [1] "data.frame"
```

It's also possible to assign names to the individual rows and columns of a data frame, which presents another possible way of determining which row of values in our table belongs to each patient.

However, since we've already solved that problem, let's solve a different problem by assigning names to the columns of our data frame so that we know what type of measurement each column represents.

Since we have six columns (including patient names), we'll need to first create a vector containing one element for each column. Create a character vector called cnames that contains the following values (in order) -- "patient", "age", "weight", "bp", "rating", "test".


```r
cnames <- c("patient", "age", "weight", "bp", "rating", "test")
```

Now, use the colnames() function to set the `colnames` attribute for our data frame. This is similar to the way we used the dim() function earlier in this lesson.


```r
colnames(my_data) <- cnames
```

Let's see if that got the job done. Print the contents of my_data.


```r
my_data
```

```
##   patient age weight bp rating test
## 1    Bill   1      5  9     13   17
## 2    Gina   2      6 10     14   18
## 3   Kelly   3      7 11     15   19
## 4    Sean   4      8 12     16   20
```

In this lesson, you learned the basics of working with two very important and common data structures -- matrices and data frames. There's much more to learn and we'll be covering more advanced topics, particularly with respect to data frames, in future lessons.

## Logic

This lesson is meant to be a short introduction to logical operations in R.

There are two logical values in R, also called boolean values. They are TRUE and FALSE. In R you can construct logical expressions which will evaluate to either TRUE or FALSE.

Many of the questions in this lesson will involve evaluating logical expressions. It may be useful to open up a second R terminal where you can experiment with some of these expressions.

Creating logical expressions requires logical operators. You're probably familiar with arithmetic operators like `+`, `-`, `*`, and `/`. The first logical operator we are going to discuss is the equality operator, represented by two equals signs `==`. Use the equality operator below to find out if TRUE is equal to TRUE.


```r
TRUE == TRUE
```

```
## [1] TRUE
```

Just like arithmetic, logical expressions can be grouped by parenthesis so that the entire expression (TRUE == TRUE) == TRUE evaluates to TRUE.

To test out this property, try evaluating (FALSE == TRUE) == FALSE .


```r
(FALSE == TRUE) == FALSE
```

```
## [1] TRUE
```

The equality operator can also be used to compare numbers. Use `==` to see if 6 is equal to 7.


```r
6==7
```

```
## [1] FALSE
```

The previous expression evaluates to FALSE because 6 is less than 7. Thankfully, there are inequality operators that allow us to test if a value is less than or greater than another value.

The less than operator `<` tests whether the number on the left side of the operator (called the left operand) is less than the number on the right side of the operator (called the right operand). Write an expression to test whether 6 is less than 7.


```r
6 < 7
```

```
## [1] TRUE
```

There is also a less-than-or-equal-to operator `<=` which tests whether the left operand is less than or equal to the right operand. Write an expression to test whether 10 is less than or equal to 10.


```r
10 <= 10
```

```
## [1] TRUE
```

Keep in mind that there are the corresponding greater than `>` and greater-than-or-equal-to `>=` operators.

Which of the following evaluates to FALSE?

1: 9 >= 10

2: 6 < 8

3: 7 == 7

4: 0 > -36

**Selection: 1**

Which of the following evaluates to TRUE?

1: 7 == 9

2: -6 > -7

3: 9 >= 10

4: 57 < 8

**Selection: 2**

The next operator we will discuss is the 'not equals' operator represented by `!=`. Not equals tests whether two values are unequal, so TRUE != FALSE evaluates to TRUE. Like the equality operator, `!=` can also be used with numbers. Try writing an expression to see if 5 is not equal to 7.


```r
5 != 7
```

```
## [1] TRUE
```

In order to negate boolean expressions you can use the NOT operator. An exclamation point `!` will cause !TRUE (say: not true) to evaluate to FALSE and !FALSE (say: not false) to evaluate to TRUE. Try using the NOT operator and the equals operator to find the opposite of whether 5 is equal to 7.


```r
!5 == 7
```

```
## [1] TRUE
```

Let's take a moment to review. The equals operator `==` tests whether two boolean values or numbers are equal, the not equals operator `!=` tests whether two boolean values or numbers are unequal, and the NOT operator `!` negates logical expressions so that TRUE expressions become FALSE and FALSE expressions become TRUE.

1: 7 != 8

2: !FALSE

3: 9 < 10

4: !(0 >= -1)

**Selection: 4**

What do you think the following expression will evaluate to?: (TRUE != FALSE) == !(6 == 7)

1: %>%

2: TRUE

3: FALSE

4: Can there be objective truth when programming?

**Selection: 2**

At some point you may need to examine relationships between multiple logical expressions. This is where the AND operator and the OR operator come in.

Let's look at how the AND operator works. There are two AND operators in R, `&` and `&&`. Both operators work similarly, if the right and left operands of AND are both TRUE the entire expression is TRUE, otherwise it is FALSE. For example, TRUE & TRUE evaluates to TRUE. Try typing FALSE & FALSE to how it is evaluated.


```r
FALSE & FALSE
```

```
## [1] FALSE
```

You can use the `&` operator to evaluate AND across a vector. The `&&` version of AND only evaluates the first member of a vector. Let's test both for practice. Type the expression TRUE & c(TRUE, FALSE, FALSE).


```r
TRUE & c(TRUE, FALSE, FALSE)
```

```
## [1]  TRUE FALSE FALSE
```

What happens in this case is that the left operand `TRUE` is recycled across every element in the vector of the right operand. This is the equivalent statement as c(TRUE, TRUE, TRUE) & c(TRUE, FALSE, FALSE).

Now we'll type the same expression except we'll use the `&&` operator. Type the expression TRUE && c(TRUE, FALSE, FALSE).


```r
TRUE && c(TRUE, FALSE, FALSE)
```

```
## [1] TRUE
```

In this case, the left operand is only evaluated with the first member of the right operand (the vector). The rest of the elements in the vector aren't evaluated at all in this expression.

The OR operator follows a similar set of rules. The `|` version of OR evaluates OR across an entire vector, while the `||` version of OR only evaluates the first member of a vector.

An expression using the OR operator will evaluate to TRUE if the left operand or the right operand is TRUE. If both are TRUE, the expression will evaluate to TRUE, however if neither are TRUE, then the expression will be FALSE.

Let's test out the vectorized version of the OR operator. Type the expression TRUE | c(TRUE, FALSE, FALSE).


```r
TRUE | c(TRUE, FALSE, FALSE)
```

```
## [1] TRUE TRUE TRUE
```

Now let's try out the non-vectorized version of the OR operator. Type the expression TRUE || c(TRUE, FALSE, FALSE).


```r
TRUE || c(TRUE, FALSE, FALSE)
```

```
## [1] TRUE
```

Logical operators can be chained together just like arithmetic operators. The expressions: `6 != 10 && FALSE && 1 >= 2` or `TRUE || 5 < 9.3 || FALSE` are perfectly normal to see.

As you may recall, arithmetic has an order of operations and so do logical expressions. All AND operators are evaluated before OR operators. Let's look at an example of an ambiguous case. Type: 5 > 8 || 6 != 8 && 4 > 3.9


```r
5 > 8 || 6 != 8 && 4 > 3.9
```

```
## [1] TRUE
```

Let's walk through the order of operations in the above case. First the left and right operands of the AND operator are evaluated. 6 is not equal 8, 4 is greater than 3.9, therefore both operands are TRUE so the resulting expression `TRUE && TRUE` evaluates to TRUE. Then the left operand of the OR operator is evaluated: 5 is not greater than 8 so the entire expression is reduced to FALSE || TRUE. Since the
right operand of this expression is TRUE the entire expression evaluates to TRUE.

Which one of the following expressions evaluates to TRUE?

1: TRUE && 62 < 62 && 44 >= 44

2: 99.99 > 100 || 45 < 7.3 || 4 != 4.0

3: TRUE && FALSE || 9 >= 4 && 3 < 6

4: FALSE || TRUE && FALSE

**Selection: 3**

Which one of the following expressions evaluates to FALSE?

1: FALSE || TRUE && 6 != 4 || 9 > 4

2: !(8 > 4) ||  5 == 5.0 && 7.8 >= 7.79

3: 6 >= -9 && !(6 > 7) && !(!TRUE)

4: FALSE && 6 >= 6 || 7 >= 8 || 50 <= 49.5

**Selection: 4**

Now that you're familiar with R's logical operators you can take advantage of a few functions that R provides for dealing with logical expressions.

The function isTRUE() takes one argument. If that argument evaluates to TRUE, the function will return TRUE. Otherwise, the function will return FALSE. Try using this function by typing: isTRUE(6 > 4)


```r
isTRUE(6 > 4)
```

```
## [1] TRUE
```

Which of the following evaluates to TRUE?

1: isTRUE(3)

2: isTRUE(!TRUE)

3: isTRUE(NA)

4: !isTRUE(4 < 3)

5: !isTRUE(8 != 5)

**Selection: 4**

The function identical() will return TRUE if the two R objects passed to it as arguments are identical. Try out the identical() function by typing: identical('twins', 'twins')


```r
identical('twins', 'twins')
```

```
## [1] TRUE
```

Which of the following evaluates to TRUE?

1: identical(4, 3.1)

2: identical(5 > 4, 3 < 3.1)

3: !identical(7, 7)

4: identical('hello', 'Hello')

**Selection: 2**

You should also be aware of the xor() function, which takes two arguments. The xor() function stands for exclusive OR. If one argument evaluates to TRUE and one argument evaluates to FALSE, then this function will return TRUE, otherwise it will return FALSE. Try out the xor() function by typing: xor(5 == 6, !FALSE)


```r
xor(5 == 6, !FALSE)
```

```
## [1] TRUE
```

5 == 6 evaluates to FALSE, !FALSE evaluates to TRUE, so xor(FALSE, TRUE) evaluates to TRUE. On the other hand if the first argument was changed to 5 == 5 and the second argument was unchanged then both arguments would have been TRUE, so xor(TRUE, TRUE) would have evaluated to FALSE.

Which of the following evaluates to FALSE?

1: xor(!isTRUE(TRUE), 6 > -1)

2: xor(!!TRUE, !!FALSE)

3: xor(4 >= 9, 8 != 8.0)

4: xor(identical(xor, 'xor'), 7 == 7.0)

**Selection: 3**

For the next few questions, we're going to need to create a vector of integers called ints. Create this vector by typing: ints <- sample(10)


```r
ints <- sample(10)
```

Now simply display the contents of ints.


```r
ints
```

```
##  [1]  9  4  3  6  8  2  7  5  1 10
```

The vector `ints` is a random sampling of integers from 1 to 10 without replacement. Let's say we wanted to ask some logical questions about contents of ints. If we type ints > 5, we will get a logical vector corresponding to whether each element of ints is greater than 5. Try typing: ints > 5


```r
ints > 5
```

```
##  [1]  TRUE FALSE FALSE  TRUE  TRUE FALSE  TRUE FALSE FALSE  TRUE
```

We can use the resulting logical vector to ask other questions about ints. The which() function takes a logical vector as an argument and returns the indices of the vector that are TRUE. For example which(c(TRUE, FALSE, TRUE)) would return the vector c(1, 3).

Use the which() function to find the indices of ints that are greater than 7.


```r
which(ints > 7)
```

```
## [1]  1  5 10
```

Which of the following commands would produce the indices of the elements in ints that are less than or equal to 2?

1: which(ints <= 2)

2: ints <= 2

3: which(ints < 2)

4: ints < 2

**Selection: 1**

Like the which() function, the functions any() and all() take logical vectors as their argument. The any() function will return TRUE if one or more of the elements in the logical vector is TRUE. The all() function will return TRUE if every element in the logical vector is TRUE.

Use the any() function to see if any of the elements of ints are less than zero.


```r
any(ints <0 )
```

```
## [1] FALSE
```

Use the all() function to see if all of the elements of ints are greater than zero.


```r
all(ints > 0 )
```

```
## [1] TRUE
```

Which of the following evaluates to TRUE?

1: all(c(TRUE, FALSE, TRUE))

2: any(ints == 2.5)

3: all(ints == 10)

4: any(ints == 10)

**Selection: 4**

That's all for this introduction to logic in R. If you really want to see what you can do with logic, check out the control flow lesson!

## Functions

Functions are one of the fundamental building blocks of the R language. They are small pieces of reusable code that can be treated like any other R object.

If you've worked through any other part of this course, you've probably used some functions already. Functions are usually characterized by the name of the function followed by parentheses.

Let's try using a few basic functions just for fun. The Sys.Date() function returns a string representing today's date. Type Sys.Date() below and see what happens.


```r
Sys.Date()
```

```
## [1] "2020-04-18"
```

Most functions in R return a value. Functions like Sys.Date() return a value based on your computer's environment, while other functions manipulate input data in order to compute a return value.

The mean() function takes a vector of numbers as input, and returns the average of all of the numbers in the input vector. Inputs to functions are often called arguments. Providing arguments to a function is also sometimes called passing arguments to that function. Arguments you want to pass to a function go inside the function's parentheses. Try passing the argument c(2, 4, 5) to the mean() function.

The mean() function takes a vector of numbers as input, and returns the average of all of the numbers in the input vector. Inputs to functions are often called arguments. Providing arguments to a function is also sometimes called passing arguments to that function. Arguments you want to pass to a function go inside the function's parentheses. Try passing the argument c(2, 4, 5) to the mean() function.


```r
mean(c(2, 4, 5))
```

```
## [1] 3.666667
```

Functions usually take arguments which are variables that the function operates on. For example, the mean() function takes a vector as an argument, like in the case of mean(c(2,6,8)). The mean() function then adds up all of the numbers in the vector and divides that sum by the length of the vector.

In the following question you will be asked to modify a script that will appear as soon as you move on from this question. When you have finished modifying the script, save your changes to the script and type submit() and the script will be evaluated. There will be some comments in the script that opens up, so be sure to read them!

The last R expression to be evaluated in a function will become the return value of that function. We want this function to take one argument, x, and return x without modifying it. Delete the pound sign so that x is returned without any modification. Make sure to save your script before you type submit().


```r
boring_function <- function(x) {
  x
}
```

Now that you've created your first function let's test it! Type: boring_function('My first function!'). If your function works, it should
| just return the string: 'My first function!'


```r
boring_function('My first function!')
```

```
## [1] "My first function!"
```

To understand computations in R, two slogans are helpful: 1. Everything that exists is an object. 2. Everything that happens is a function call.

If you want to see the source code for any function, just type the function name without any arguments or parentheses. Let's try this out with the function you just created. Type: boring_function to view its source code.


```r
boring_function
```

```
## function(x) {
##   x
## }
```

Time to make a more useful function! We're going to replicate the functionality of the mean() function by creating a function called: my_mean(). Remember that to calculate the average of all of the numbers in a vector you find the sum of all the numbers in the vector, and then divide that sum by the number of numbers in the vector.


```r
my_mean <- function(my_vector) {
    #v <- sum(c(1, 2, 3))
	#x <- length(v)  
	#v / 3
	sum(my_vector)/length(my_vector)
}
```

Now test out your my_mean() function by finding the mean of the vector c(4, 5, 10).


```r
my_mean(c(4, 5, 10))
```

```
## [1] 6.333333
```

Next, let's try writing a function with default arguments. You can set default values for a function's arguments, and this can be useful if you think someone who uses your function will set a certain argument to the same value most of the time.


```r
remainder <- function(num, divisor=2) {
  num %% divisor
}
```

Let's do some testing of the remainder function. Run remainder(5) and see what happens.


```r
remainder(5)
```

```
## [1] 1
```

Let's take a moment to examine what just happened. You provided one argument to the function, and R matched that argument to 'num' since 'num' is the first argument. The default value for 'divisor' is 2, so the function used the default value you provided.

Now let's test the remainder function by providing two arguments. Type: remainder(11, 5) and let's see what happens.


```r
remainder(11, 5)
```

```
## [1] 1
```

Once again, the arguments have been matched appropriately.

You can also explicitly specify arguments in a function. When you explicitly designate argument values by name, the ordering of the arguments becomes unimportant. You can try this out by typing: remainder(divisor = 11, num = 5).


```r
remainder(divisor = 11, num = 5)
```

```
## [1] 5
```

As you can see, there is a significant difference between remainder(11, 5) and remainder(divisor = 11, num = 5)!

R can also partially match arguments. Try typing remainder(4, div = 2) to see this feature in action.


```r
remainder(4, div = 2)
```

```
## [1] 0
```

A word of warning: in general you want to make your code as easy to understand as possible. Switching around the orders of arguments by specifying their names or only using partial argument names can be confusing, so use these features with caution!

With all of this talk about arguments, you may be wondering if there is a way you can see a function's arguments (besides looking at the documentation). Thankfully, you can use the args() function! Type: args(remainder) to examine the arguments for the remainder function.


```r
args(remainder)
```

```
## function (num, divisor = 2) 
## NULL
```

You may not realize it but I just tricked you into doing something pretty interesting! args() is a function, remainder() is a function, yet remainder was an argument for args(). Yes it's true: you can pass functions as arguments! This is a very powerful concept. Let's write a script to see how it works.


```r
evaluate <- function(func, dat){
        func(dat)
}
```

Let's take your new evaluate() function for a spin! Use evaluate to find the standard deviation of the vector c(1.4, 3.6, 7.9, 8.8).


```r
evaluate(sd, c(1.4, 3.6, 7.9, 8.8))
```

```
## [1] 3.514138
```

The idea of passing functions as arguments to other functions is an important and fundamental concept in programming.

You may be surprised to learn that you can pass a function as an argument without first defining the passed function. Functions that are not named are appropriately known as anonymous functions.

Let's use the evaluate function to explore how anonymous functions work. For the first argument of the evaluate function we're going to write a tiny function that fits on one line. In the second argument we'll pass some data to the tiny anonymous function in the first argument.

Type the following command and then we'll discuss how it works: evaluate(function(x){x+1}, 6)


```r
evaluate(function(x){x+1}, 6)
```

```
## [1] 7
```

The first argument is a tiny anonymous function that takes one argument `x` and returns `x+1`. We passed the number 6 into this function so the entire expression evaluates to 7.

Try using evaluate() along with an anonymous function to return the first element of the vector c(8, 4, 0). Your anonymous function should only take one argument which should be a variable `x`.


```r
evaluate(function(x){x[1]}, c(8, 4, 0))
```

```
## [1] 8
```

Now try using evaluate() along with an anonymous function to return the last element of the vector c(8, 4, 0). Your anonymous function should only take one argument which should be a variable `x`.


```r
evaluate(function(x){x[length(x)]}, c(8, 4, 0))
```

```
## [1] 0
```

For the rest of the course we're going to use the paste() function frequently. Type ?paste so we can take a look at the documentation for
| the paste function.


```r
?paste
```

As you can see the first argument of paste() is `...` which is referred to as an ellipsis or simply dot-dot-dot. The ellipsis allows an indefinite number of arguments to be passed into a function. In the case of paste() any number of strings can be passed as arguments and paste() will return all of the strings combined into one string.

Just to see how paste() works, type paste("Programming", "is", "fun!")


```r
paste("Programming", "is", "fun!")
```

```
## [1] "Programming is fun!"
```

Time to write our own modified version of paste().


```r
# For example the expression `telegram("Good", "morning")` should evaluate to:
# "START Good morning STOP"

telegram <- function(...){
        paste("START", ..., "STOP")
}
```

Now let's test out your telegram function. Use your new telegram function passing in whatever arguments you wish!


```r
telegram("I'm sorry Ms Jackson")
```

```
## [1] "START I'm sorry Ms Jackson STOP"
```

Time to use your mad_libs function. Make sure to name the place, adjective, and noun arguments in order for your function to work.


```r
mad_libs <- function(...){
        # Do your argument unpacking here!
        args <- list(...)
        place <- args[["place"]]
        adjective  <- args[["adjective"]]
        noun <- args[["noun"]]
        
        # Notice the variables you'll need to create in order for the code below to
        # be functional!
        paste("News from", place, "today where", adjective, "students took to the streets in protest of the new", noun, "being installed on campus.")
}
```


```r
mad_libs("Baltimore", "bitch-ass", "Giant Penis")
```

```
## [1] "News from  today where  students took to the streets in protest of the new  being installed on campus."
```

We're coming to the end of this lesson, but there's still one more idea you should be made aware of.

You're familiar with adding, subtracting, multiplying, and dividing numbers in R. To do this you use the +, -, *, and / symbols. These symbols are called binary operators because they take two inputs, an input from the left and an input from the right.

In R you can define your own binary operators. In the next script I'll show you how.


```r
"%p%" <- function(left, right){ # Remember to add arguments!
        paste(left, right, sep = " ")
}
```

You made your own binary operator! Let's test it out. Paste together the strings: 'I', 'love', 'R!' using your new binary operator.


```r
"I" %p% "love" %p% "R!"
```

```
## [1] "I love R!"
```
