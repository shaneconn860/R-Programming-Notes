---
title: "RProgramming Swirl Lesson 10-12"
output: 
  html_document:
    keep_md: true
---

## lapply and sapply

In this lesson, you'll learn how to use lapply() and sapply(), the two most important members of R's *apply family of functions, also known as loop functions.

These powerful functions, along with their close relatives (vapply() and tapply(), among others) offer a concise and convenient means of implementing the Split-Apply-Combine strategy for data analysis.

Each of the *apply functions will SPLIT up some data into smaller pieces, APPLY a function to each piece, then COMBINE the results. A more detailed discussion of this strategy is found in Hadley Wickham's Journal of Statistical Software paper titled 'The Split-Apply-Combine Strategy for Data Analysis'.

Throughout this lesson, we'll use the Flags dataset from the UCI Machine Learning Repository. This dataset contains details of various nations and their flags. More information may be found here: http://archive.ics.uci.edu/ml/datasets/Flags

Download the file from the above site and read in to R. Column names must be added using the code below:


```r
flags <- read.csv("./flag_data.csv", header=FALSE, sep=",")

colnames(flags) <- c("name", "landmass", "zone", "area", "population", "language", "religion", "bars", "stripes", 
                             "colours", "red", "green", "blue", "gold", "white", "black", "orange", "mainhue", "circles", 
                             "crosses", "saltires", "quarters", "sunstars", "crescent", "triangle", "icon", "animate", "text", 
                             "topleft", "botright")
```

I've stored the dataset in a variable called flags. Type head(flags) to preview the first six lines (i.e. the 'head') of the dataset.


```r
head(flags)
```

```
##             name landmass zone area population language religion bars stripes
## 1    Afghanistan        5    1  648         16       10        2    0       3
## 2        Albania        3    1   29          3        6        6    0       0
## 3        Algeria        4    1 2388         20        8        2    2       0
## 4 American-Samoa        6    3    0          0        1        1    0       0
## 5        Andorra        3    1    0          0        6        0    3       0
## 6         Angola        4    2 1247          7       10        5    0       2
##   colours red green blue gold white black orange mainhue circles crosses
## 1       5   1     1    0    1     1     1      0   green       0       0
## 2       3   1     0    0    1     0     1      0     red       0       0
## 3       3   1     1    0    0     1     0      0   green       0       0
## 4       5   1     0    1    1     1     0      1    blue       0       0
## 5       3   1     0    1    1     0     0      0    gold       0       0
## 6       3   1     0    0    1     0     1      0     red       0       0
##   saltires quarters sunstars crescent triangle icon animate text topleft
## 1        0        0        1        0        0    1       0    0   black
## 2        0        0        1        0        0    0       1    0     red
## 3        0        0        1        1        0    0       0    0   green
## 4        0        0        0        0        1    1       1    0    blue
## 5        0        0        0        0        0    0       0    0    blue
## 6        0        0        1        0        0    1       0    0     red
##   botright
## 1    green
## 2      red
## 3    white
## 4      red
## 5      red
## 6    black
```

You may need to scroll up to see all of the output. Now, let's check out the dimensions of the dataset using dim(flags).


```r
dim(flags)
```

```
## [1] 194  30
```

This tells us that there are 194 rows, or observations, and 30 columns, or variables. Each observation is a country and each variable describes some characteristic of that country or its flag. To open a more complete description of the dataset in a separate text file, type viewinfo() when you are back at the prompt (>).

As with any dataset, we'd like to know in what format the variables have been stored. In other words, what is the 'class' of each variable? What happens if we do class(flags)? Try it out.

As with any dataset, we'd like to know in what format the variables have been stored. In other words, what is the 'class' of each variable? What happens if we do class(flags)? Try it out.


```r
class(flags)
```

```
## [1] "data.frame"
```

That just tells us that the entire dataset is stored as a 'data.frame', which doesn't answer our question. What we really need is to call the class() function on each individual column. While we could do this manually (i.e. one column at a time) it's much faster if we can automate the process. Sounds like a loop!

The lapply() function takes a list as input, applies a function to each element of the list, then returns a list of the same length as the original one. Since a data frame is really just a list of vectors (you can see this with as.list(flags)), we can use lapply() to apply the class() function to each column of the flags dataset. Let's see it in action!

Type cls_list <- lapply(flags, class) to apply the class() function to each column of the flags dataset and store the result in a variable called cls_list. Note that you just supply the name of the function you want to apply (i.e. class), without the usual parentheses after it.


```r
cls_list <- lapply(flags, class)
```

Type cls_list to view the result.


```r
cls_list
```

```
## $name
## [1] "factor"
## 
## $landmass
## [1] "integer"
## 
## $zone
## [1] "integer"
## 
## $area
## [1] "integer"
## 
## $population
## [1] "integer"
## 
## $language
## [1] "integer"
## 
## $religion
## [1] "integer"
## 
## $bars
## [1] "integer"
## 
## $stripes
## [1] "integer"
## 
## $colours
## [1] "integer"
## 
## $red
## [1] "integer"
## 
## $green
## [1] "integer"
## 
## $blue
## [1] "integer"
## 
## $gold
## [1] "integer"
## 
## $white
## [1] "integer"
## 
## $black
## [1] "integer"
## 
## $orange
## [1] "integer"
## 
## $mainhue
## [1] "factor"
## 
## $circles
## [1] "integer"
## 
## $crosses
## [1] "integer"
## 
## $saltires
## [1] "integer"
## 
## $quarters
## [1] "integer"
## 
## $sunstars
## [1] "integer"
## 
## $crescent
## [1] "integer"
## 
## $triangle
## [1] "integer"
## 
## $icon
## [1] "integer"
## 
## $animate
## [1] "integer"
## 
## $text
## [1] "integer"
## 
## $topleft
## [1] "factor"
## 
## $botright
## [1] "factor"
```

The 'l' in 'lapply' stands for 'list'. Type class(cls_list) to confirm that lapply() returned a list.


```r
class(cls_list)
```

```
## [1] "list"
```

As expected, we got a list of length 30 -- one element for each variable/column. The output would be considerably more compact if we could represent it as a vector instead of a list.

You may remember from a previous lesson that lists are most helpful for storing multiple classes of data. In this case, since every element of the list returned by lapply() is a character vector of length one (i.e. "integer" and "vector"), cls_list can be simplified to a character vector. To do this manually, type as.character(cls_list).


```r
as.character(cls_list)
```

```
##  [1] "factor"  "integer" "integer" "integer" "integer" "integer" "integer"
##  [8] "integer" "integer" "integer" "integer" "integer" "integer" "integer"
## [15] "integer" "integer" "integer" "factor"  "integer" "integer" "integer"
## [22] "integer" "integer" "integer" "integer" "integer" "integer" "integer"
## [29] "factor"  "factor"
```

sapply() allows you to automate this process by calling lapply() behind the scenes, but then attempting to simplify (hence the 's' in 'sapply') the result for you. Use sapply() the same way you used lapply() to get the class of each column of the flags dataset and store the result in cls_vect. If you need help, type ?sapply to bring up the documentation.

sapply() allows you to automate this process by calling lapply() behind the scenes, but then attempting to simplify (hence the 's' in 'sapply') the result for you. Use sapply() the same way you used lapply() to get the class of each column of the flags dataset and store the result in cls_vect. If you need help, type ?sapply to bring up the documentation.


```r
cls_vect <- sapply(flags, class)
```

Use class(cls_vect) to confirm that sapply() simplified the result to a character vector.


```r
class(cls_vect)
```

```
## [1] "character"
```

In general, if the result is a list where every element is of length one, then sapply() returns a vector. If the result is a list where every element is a vector of the same length (> 1), sapply() returns a matrix. If sapply() can't figure things out, then it just returns a list, no different from what lapply() would give you.

Let's practice using lapply() and sapply() some more!

Columns 11 through 17 of our dataset are indicator variables, each representing a different color. The value of the indicator variable is 1 if the color is present in a country's flag and 0 otherwise.

Therefore, if we want to know the total number of countries (in our dataset) with, for example, the color orange on their flag, we can just add up all of the 1s and 0s in the 'orange' column. Try sum(flags$orange) to see this.


```r
sum(flags$orange)
```

```
## [1] 26
```

Now we want to repeat this operation for each of the colors recorded in the dataset.

First, use flag_colors <- flags[, 11:17] to extract the columns containing the color data and store them in a new data frame called flag_colors. (Note the comma before 11:17. This subsetting command tells R that we want all rows, but only columns 11 through 17.)


```r
flag_colors <- flags[, 11:17]
```

Use the head() function to look at the first 6 lines of flag_colors.


```r
head(flag_colors)
```

```
##   red green blue gold white black orange
## 1   1     1    0    1     1     1      0
## 2   1     0    0    1     0     1      0
## 3   1     1    0    0     1     0      0
## 4   1     0    1    1     1     0      1
## 5   1     0    1    1     0     0      0
## 6   1     0    0    1     0     1      0
```

To get a list containing the sum of each column of flag_colors, call the lapply() function with two arguments. The first argument is the object over which we are looping (i.e. flag_colors) and the second argument is the name of the function we wish to apply to each column (i.e. sum). Remember that the second argument is just the name of the function with no parentheses, etc.


```r
lapply(flag_colors, sum)
```

```
## $red
## [1] 153
## 
## $green
## [1] 91
## 
## $blue
## [1] 99
## 
## $gold
## [1] 91
## 
## $white
## [1] 146
## 
## $black
## [1] 52
## 
## $orange
## [1] 26
```

This tells us that of the 194 flags in our dataset, 153 contain the color red, 91 contain green, 99 contain blue, and so on.

The result is a list, since lapply() always returns a list. Each element of this list is of length one, so the result can be simplified to a vector by calling sapply() instead of lapply(). Try it now.


```r
sapply(flag_colors, sum)
```

```
##    red  green   blue   gold  white  black orange 
##    153     91     99     91    146     52     26
```

Perhaps it's more informative to find the proportion of flags (out of 194) containing each color. Since each column is just a bunch of 1s and 0s, the arithmetic mean of each column will give us the proportion of 1s. (If it's not clear why, think of a simpler situation where you have three 1s and two 0s -- (1 + 1 + 1 + 0 + 0)/5 = 3/5 = 0.6).

Use sapply() to apply the mean() function to each column of flag_colors. Remember that the second argument to sapply() should just specify the name of the function (i.e. mean) that you want to apply.


```r
sapply(flag_colors, mean)
```

```
##       red     green      blue      gold     white     black    orange 
## 0.7886598 0.4690722 0.5103093 0.4690722 0.7525773 0.2680412 0.1340206
```

In the examples we've looked at so far, sapply() has been able to simplify the result to vector. That's because each element of the list returned by lapply() was a vector of length one. Recall that sapply() instead returns a matrix when each element of the list returned by lapply() is a vector of the same length (> 1).

To illustrate this, let's extract columns 19 through 23 from the flags dataset and store the result in a new data frame called flag_shapes. flag_shapes <- flags[, 19:23] will do it.


```r
flag_shapes <- flags[, 19:23]
```

Each of these columns (i.e. variables) represents the number of times a particular shape or design appears on a country's flag. We are interested in the minimum and maximum number of times each shape or design appears.

The range() function returns the minimum and maximum of its first argument, which should be a numeric vector. Use lapply() to apply the range function to each column of flag_shapes. Don't worry about storing the result in a new variable. By now, we know that lapply() always returns a list.


```r
lapply(flag_shapes, range)
```

```
## $circles
## [1] 0 4
## 
## $crosses
## [1] 0 2
## 
## $saltires
## [1] 0 1
## 
## $quarters
## [1] 0 4
## 
## $sunstars
## [1]  0 50
```

Do the same operation, but using sapply() and store the result in a variable called shape_mat.


```r
shape_mat <- sapply(flag_shapes, range)

shape_mat
```

```
##      circles crosses saltires quarters sunstars
## [1,]       0       0        0        0        0
## [2,]       4       2        1        4       50
```

Each column of shape_mat gives the minimum (row 1) and maximum (row 2) number of times its respective shape appears in different flags.

Use the class() function to confirm that shape_mat is a matrix.


```r
class(shape_mat)
```

```
## [1] "matrix"
```

As we've seen, sapply() always attempts to simplify the result given by lapply(). It has been successful in doing so for each of the examples we've looked at so far. Let's look at an example where sapply() can't figure out how to simplify the result and thus returns a list, no different from lapply().

When given a vector, the unique() function returns a vector with all duplicate elements removed. In other words, unique() returns a vector of only the 'unique' elements. To see how it works, try unique(c(3, 4, 5, 5, 5, 6, 6)).


```r
unique(c(3, 4, 5, 5, 5, 6, 6))
```

```
## [1] 3 4 5 6
```

We want to know the unique values for each variable in the flags dataset. To accomplish this, use lapply() to apply the unique() function to each column in the flags dataset, storing the result in a variable called unique_vals.


```r
unique_vals <- lapply(flags,unique)
```

Print the value of unique_vals to the console.

Since unique_vals is a list, you can use what you've learned to determine the length of each element of unique_vals (i.e. the number of unique values for each variable). Simplify the result, if possible. Hint: Apply the length() function to each element of unique_vals.


```r
sapply(unique_vals, length)
```

```
##       name   landmass       zone       area population   language   religion 
##        194          6          4        136         48         10          8 
##       bars    stripes    colours        red      green       blue       gold 
##          5         12          8          2          2          2          2 
##      white      black     orange    mainhue    circles    crosses   saltires 
##          2          2          2          8          4          3          2 
##   quarters   sunstars   crescent   triangle       icon    animate       text 
##          3         14          2          2          2          2          2 
##    topleft   botright 
##          7          8
```

The fact that the elements of the unique_vals list are all vectors of *different* length poses a problem for sapply(), since there's no obvious way of simplifying the result.

Use sapply() to apply the unique() function to each column of the flags dataset to see that you get the same unsimplified list that you got from lapply().


```r
sapply(flags, unique)
```

```
## $name
##   [1] Afghanistan              Albania                  Algeria                 
##   [4] American-Samoa           Andorra                  Angola                  
##   [7] Anguilla                 Antigua-Barbuda          Argentina               
##  [10] Argentine                Australia                Austria                 
##  [13] Bahamas                  Bahrain                  Bangladesh              
##  [16] Barbados                 Belgium                  Belize                  
##  [19] Benin                    Bermuda                  Bhutan                  
##  [22] Bolivia                  Botswana                 Brazil                  
##  [25] British-Virgin-Isles     Brunei                   Bulgaria                
##  [28] Burkina                  Burma                    Burundi                 
##  [31] Cameroon                 Canada                   Cape-Verde-Islands      
##  [34] Cayman-Islands           Central-African-Republic Chad                    
##  [37] Chile                    China                    Colombia                
##  [40] Comorro-Islands          Congo                    Cook-Islands            
##  [43] Costa-Rica               Cuba                     Cyprus                  
##  [46] Czechoslovakia           Denmark                  Djibouti                
##  [49] Dominica                 Dominican-Republic       Ecuador                 
##  [52] Egypt                    El-Salvador              Equatorial-Guinea       
##  [55] Ethiopia                 Faeroes                  Falklands-Malvinas      
##  [58] Fiji                     Finland                  France                  
##  [61] French-Guiana            French-Polynesia         Gabon                   
##  [64] Gambia                   Germany-DDR              Germany-FRG             
##  [67] Ghana                    Gibraltar                Greece                  
##  [70] Greenland                Grenada                  Guam                    
##  [73] Guatemala                Guinea                   Guinea-Bissau           
##  [76] Guyana                   Haiti                    Honduras                
##  [79] Hong-Kong                Hungary                  Iceland                 
##  [82] India                    Indonesia                Iran                    
##  [85] Iraq                     Ireland                  Israel                  
##  [88] Italy                    Ivory-Coast              Jamaica                 
##  [91] Japan                    Jordan                   Kampuchea               
##  [94] Kenya                    Kiribati                 Kuwait                  
##  [97] Laos                     Lebanon                  Lesotho                 
## [100] Liberia                  Libya                    Liechtenstein           
## [103] Luxembourg               Malagasy                 Malawi                  
## [106] Malaysia                 Maldive-Islands          Mali                    
## [109] Malta                    Marianas                 Mauritania              
## [112] Mauritius                Mexico                   Micronesia              
## [115] Monaco                   Mongolia                 Montserrat              
## [118] Morocco                  Mozambique               Nauru                   
## [121] Nepal                    Netherlands              Netherlands-Antilles    
## [124] New-Zealand              Nicaragua                Niger                   
## [127] Nigeria                  Niue                     North-Korea             
## [130] North-Yemen              Norway                   Oman                    
## [133] Pakistan                 Panama                   Papua-New-Guinea        
## [136] Parguay                  Peru                     Philippines             
## [139] Poland                   Portugal                 Puerto-Rico             
## [142] Qatar                    Romania                  Rwanda                  
## [145] San-Marino               Sao-Tome                 Saudi-Arabia            
## [148] Senegal                  Seychelles               Sierra-Leone            
## [151] Singapore                Soloman-Islands          Somalia                 
## [154] South-Africa             South-Korea              South-Yemen             
## [157] Spain                    Sri-Lanka                St-Helena               
## [160] St-Kitts-Nevis           St-Lucia                 St-Vincent              
## [163] Sudan                    Surinam                  Swaziland               
## [166] Sweden                   Switzerland              Syria                   
## [169] Taiwan                   Tanzania                 Thailand                
## [172] Togo                     Tonga                    Trinidad-Tobago         
## [175] Tunisia                  Turkey                   Turks-Cocos-Islands     
## [178] Tuvalu                   UAE                      Uganda                  
## [181] UK                       Uruguay                  US-Virgin-Isles         
## [184] USA                      USSR                     Vanuatu                 
## [187] Vatican-City             Venezuela                Vietnam                 
## [190] Western-Samoa            Yugoslavia               Zaire                   
## [193] Zambia                   Zimbabwe                
## 194 Levels: Afghanistan Albania Algeria American-Samoa Andorra ... Zimbabwe
## 
## $landmass
## [1] 5 3 4 6 1 2
## 
## $zone
## [1] 1 3 2 4
## 
## $area
##   [1]   648    29  2388     0  1247  2777  7690    84    19     1   143    31
##  [13]    23   113    47  1099   600  8512     6   111   274   678    28   474
##  [25]  9976     4   623  1284   757  9561  1139     2   342    51   115     9
##  [37]   128    43    22    49   284  1001    21  1222    12    18   337   547
##  [49]    91   268    10   108   249   239   132  2176   109   246    36   215
##  [61]   112    93   103  3268  1904  1648   435    70   301   323    11   372
##  [73]    98   181   583   236    30  1760     3   587   118   333  1240  1031
##  [85]  1973  1566   447   783   140    41  1267   925   121   195   324   212
##  [97]   804    76   463   407  1285   300   313    92   237    26  2150   196
## [109]    72   637  1221    99   288   505    66  2506    63    17   450   185
## [121]   945   514    57     5   164   781   245   178  9363 22402    15   912
## [133]   256   905   753   391
## 
## $population
##  [1]   16    3   20    0    7   28   15    8   90   10    1    6  119    9   35
## [16]    4   24    2   11 1008    5   47   31   54   17   61   14  684  157   39
## [31]   57  118   13   77   12   56   18   84   48   36   22   29   38   49   45
## [46]  231  274   60
## 
## $language
##  [1] 10  6  8  1  2  4  3  5  7  9
## 
## $religion
## [1] 2 6 1 0 5 3 4 7
## 
## $bars
## [1] 0 2 3 1 5
## 
## $stripes
##  [1]  3  0  2  1  5  9 11 14  4  6 13  7
## 
## $colours
## [1] 5 3 2 8 6 4 7 1
## 
## $red
## [1] 1 0
## 
## $green
## [1] 1 0
## 
## $blue
## [1] 0 1
## 
## $gold
## [1] 1 0
## 
## $white
## [1] 1 0
## 
## $black
## [1] 1 0
## 
## $orange
## [1] 0 1
## 
## $mainhue
## [1] green  red    blue   gold   white  orange black  brown 
## Levels: black blue brown gold green orange red white
## 
## $circles
## [1] 0 1 4 2
## 
## $crosses
## [1] 0 1 2
## 
## $saltires
## [1] 0 1
## 
## $quarters
## [1] 0 1 4
## 
## $sunstars
##  [1]  1  0  6 22 14  3  4  5 15 10  7  2  9 50
## 
## $crescent
## [1] 0 1
## 
## $triangle
## [1] 0 1
## 
## $icon
## [1] 1 0
## 
## $animate
## [1] 0 1
## 
## $text
## [1] 0 1
## 
## $topleft
## [1] black  red    green  blue   white  orange gold  
## Levels: black blue gold green orange red white
## 
## $botright
## [1] green  red    white  black  blue   gold   orange brown 
## Levels: black blue brown gold green orange red white
```

Occasionally, you may need to apply a function that is not yet defined, thus requiring you to write your own. Writing functions in R is beyond the scope of this lesson, but let's look at a quick example of how you might do so in the context of loop functions.

Pretend you are interested in only the second item from each element of the unique_vals list that you just created. Since each element of the unique_vals list is a vector and we're not aware of any built-in function in R that returns the second element of a vector, we will construct our own function.

lapply(unique_vals, function(elem) elem[2]) will return a list containing the second item from each element of the unique_vals list. Note that our function takes one argument, elem, which is just a 'dummy variable' that takes on the value of each element of unique_vals, in turn.


```r
lapply(unique_vals, function(elem) elem[2])
```

```
## $name
## [1] Albania
## 194 Levels: Afghanistan Albania Algeria American-Samoa Andorra ... Zimbabwe
## 
## $landmass
## [1] 3
## 
## $zone
## [1] 3
## 
## $area
## [1] 29
## 
## $population
## [1] 3
## 
## $language
## [1] 6
## 
## $religion
## [1] 6
## 
## $bars
## [1] 2
## 
## $stripes
## [1] 0
## 
## $colours
## [1] 3
## 
## $red
## [1] 0
## 
## $green
## [1] 0
## 
## $blue
## [1] 1
## 
## $gold
## [1] 0
## 
## $white
## [1] 0
## 
## $black
## [1] 0
## 
## $orange
## [1] 1
## 
## $mainhue
## [1] red
## Levels: black blue brown gold green orange red white
## 
## $circles
## [1] 1
## 
## $crosses
## [1] 1
## 
## $saltires
## [1] 1
## 
## $quarters
## [1] 1
## 
## $sunstars
## [1] 0
## 
## $crescent
## [1] 1
## 
## $triangle
## [1] 1
## 
## $icon
## [1] 0
## 
## $animate
## [1] 1
## 
## $text
## [1] 1
## 
## $topleft
## [1] red
## Levels: black blue gold green orange red white
## 
## $botright
## [1] red
## Levels: black blue brown gold green orange red white
```

The only difference between previous examples and this one is that we are defining and using our own function right in the call to lapply(). Our function has no name and disappears as soon as lapply() is done using it. So-called 'anonymous functions' can be very useful when one of R's built-in functions isn't an option.

In this lesson, you learned how to use the powerful lapply() and sapply() functions to apply an operation over the elements of a list. In the next lesson, we'll take a look at some close relatives of lapply() and sapply().

## vapply() and tapply()

In the last lesson, you learned about the two most fundamental members of R's *apply family of functions: lapply() and sapply(). Both take a list as input, apply a function to each element of the list, then combine and return the result. lapply() always returns a list, whereas sapply() attempts to simplify the result.

In this lesson, you'll learn how to use vapply() and tapply(), each of which serves a very specific purpose within the Split-Apply-Combine methodology. For consistency, we'll use the same dataset we used in the 'lapply and sapply' lesson.

As you saw in the last lesson, the unique() function returns a vector of the unique values contained in the object passed to it. Therefore, sapply(flags, unique) returns a list containing one vector of unique values for each column of the flags dataset. Try it again now.


```r
sapply(flags, unique)
```

```
## $name
##   [1] Afghanistan              Albania                  Algeria                 
##   [4] American-Samoa           Andorra                  Angola                  
##   [7] Anguilla                 Antigua-Barbuda          Argentina               
##  [10] Argentine                Australia                Austria                 
##  [13] Bahamas                  Bahrain                  Bangladesh              
##  [16] Barbados                 Belgium                  Belize                  
##  [19] Benin                    Bermuda                  Bhutan                  
##  [22] Bolivia                  Botswana                 Brazil                  
##  [25] British-Virgin-Isles     Brunei                   Bulgaria                
##  [28] Burkina                  Burma                    Burundi                 
##  [31] Cameroon                 Canada                   Cape-Verde-Islands      
##  [34] Cayman-Islands           Central-African-Republic Chad                    
##  [37] Chile                    China                    Colombia                
##  [40] Comorro-Islands          Congo                    Cook-Islands            
##  [43] Costa-Rica               Cuba                     Cyprus                  
##  [46] Czechoslovakia           Denmark                  Djibouti                
##  [49] Dominica                 Dominican-Republic       Ecuador                 
##  [52] Egypt                    El-Salvador              Equatorial-Guinea       
##  [55] Ethiopia                 Faeroes                  Falklands-Malvinas      
##  [58] Fiji                     Finland                  France                  
##  [61] French-Guiana            French-Polynesia         Gabon                   
##  [64] Gambia                   Germany-DDR              Germany-FRG             
##  [67] Ghana                    Gibraltar                Greece                  
##  [70] Greenland                Grenada                  Guam                    
##  [73] Guatemala                Guinea                   Guinea-Bissau           
##  [76] Guyana                   Haiti                    Honduras                
##  [79] Hong-Kong                Hungary                  Iceland                 
##  [82] India                    Indonesia                Iran                    
##  [85] Iraq                     Ireland                  Israel                  
##  [88] Italy                    Ivory-Coast              Jamaica                 
##  [91] Japan                    Jordan                   Kampuchea               
##  [94] Kenya                    Kiribati                 Kuwait                  
##  [97] Laos                     Lebanon                  Lesotho                 
## [100] Liberia                  Libya                    Liechtenstein           
## [103] Luxembourg               Malagasy                 Malawi                  
## [106] Malaysia                 Maldive-Islands          Mali                    
## [109] Malta                    Marianas                 Mauritania              
## [112] Mauritius                Mexico                   Micronesia              
## [115] Monaco                   Mongolia                 Montserrat              
## [118] Morocco                  Mozambique               Nauru                   
## [121] Nepal                    Netherlands              Netherlands-Antilles    
## [124] New-Zealand              Nicaragua                Niger                   
## [127] Nigeria                  Niue                     North-Korea             
## [130] North-Yemen              Norway                   Oman                    
## [133] Pakistan                 Panama                   Papua-New-Guinea        
## [136] Parguay                  Peru                     Philippines             
## [139] Poland                   Portugal                 Puerto-Rico             
## [142] Qatar                    Romania                  Rwanda                  
## [145] San-Marino               Sao-Tome                 Saudi-Arabia            
## [148] Senegal                  Seychelles               Sierra-Leone            
## [151] Singapore                Soloman-Islands          Somalia                 
## [154] South-Africa             South-Korea              South-Yemen             
## [157] Spain                    Sri-Lanka                St-Helena               
## [160] St-Kitts-Nevis           St-Lucia                 St-Vincent              
## [163] Sudan                    Surinam                  Swaziland               
## [166] Sweden                   Switzerland              Syria                   
## [169] Taiwan                   Tanzania                 Thailand                
## [172] Togo                     Tonga                    Trinidad-Tobago         
## [175] Tunisia                  Turkey                   Turks-Cocos-Islands     
## [178] Tuvalu                   UAE                      Uganda                  
## [181] UK                       Uruguay                  US-Virgin-Isles         
## [184] USA                      USSR                     Vanuatu                 
## [187] Vatican-City             Venezuela                Vietnam                 
## [190] Western-Samoa            Yugoslavia               Zaire                   
## [193] Zambia                   Zimbabwe                
## 194 Levels: Afghanistan Albania Algeria American-Samoa Andorra ... Zimbabwe
## 
## $landmass
## [1] 5 3 4 6 1 2
## 
## $zone
## [1] 1 3 2 4
## 
## $area
##   [1]   648    29  2388     0  1247  2777  7690    84    19     1   143    31
##  [13]    23   113    47  1099   600  8512     6   111   274   678    28   474
##  [25]  9976     4   623  1284   757  9561  1139     2   342    51   115     9
##  [37]   128    43    22    49   284  1001    21  1222    12    18   337   547
##  [49]    91   268    10   108   249   239   132  2176   109   246    36   215
##  [61]   112    93   103  3268  1904  1648   435    70   301   323    11   372
##  [73]    98   181   583   236    30  1760     3   587   118   333  1240  1031
##  [85]  1973  1566   447   783   140    41  1267   925   121   195   324   212
##  [97]   804    76   463   407  1285   300   313    92   237    26  2150   196
## [109]    72   637  1221    99   288   505    66  2506    63    17   450   185
## [121]   945   514    57     5   164   781   245   178  9363 22402    15   912
## [133]   256   905   753   391
## 
## $population
##  [1]   16    3   20    0    7   28   15    8   90   10    1    6  119    9   35
## [16]    4   24    2   11 1008    5   47   31   54   17   61   14  684  157   39
## [31]   57  118   13   77   12   56   18   84   48   36   22   29   38   49   45
## [46]  231  274   60
## 
## $language
##  [1] 10  6  8  1  2  4  3  5  7  9
## 
## $religion
## [1] 2 6 1 0 5 3 4 7
## 
## $bars
## [1] 0 2 3 1 5
## 
## $stripes
##  [1]  3  0  2  1  5  9 11 14  4  6 13  7
## 
## $colours
## [1] 5 3 2 8 6 4 7 1
## 
## $red
## [1] 1 0
## 
## $green
## [1] 1 0
## 
## $blue
## [1] 0 1
## 
## $gold
## [1] 1 0
## 
## $white
## [1] 1 0
## 
## $black
## [1] 1 0
## 
## $orange
## [1] 0 1
## 
## $mainhue
## [1] green  red    blue   gold   white  orange black  brown 
## Levels: black blue brown gold green orange red white
## 
## $circles
## [1] 0 1 4 2
## 
## $crosses
## [1] 0 1 2
## 
## $saltires
## [1] 0 1
## 
## $quarters
## [1] 0 1 4
## 
## $sunstars
##  [1]  1  0  6 22 14  3  4  5 15 10  7  2  9 50
## 
## $crescent
## [1] 0 1
## 
## $triangle
## [1] 0 1
## 
## $icon
## [1] 1 0
## 
## $animate
## [1] 0 1
## 
## $text
## [1] 0 1
## 
## $topleft
## [1] black  red    green  blue   white  orange gold  
## Levels: black blue gold green orange red white
## 
## $botright
## [1] green  red    white  black  blue   gold   orange brown 
## Levels: black blue brown gold green orange red white
```

What if you had forgotten how unique() works and mistakenly thought it returns the *number* of unique values contained in the object passed to it? Then you might have incorrectly expected sapply(flags, unique) to return a numeric vector, since each element of the list returned would contain a single number and sapply() could then simplify the result to a vector.

When working interactively (at the prompt), this is not much of a problem, since you see the result immediately and will quickly recognize your mistake. However, when working non-interactively (e.g. writing your own functions), a misunderstanding may go undetected and cause incorrect results later on. Therefore, you may wish to be more careful and that's where vapply() is useful.

Whereas sapply() tries to 'guess' the correct format of the result, vapply() allows you to specify it explicitly. If the result doesn't match the format you specify, vapply() will throw an error, causing the operation to stop. This can prevent significant problems in your code that might be caused by getting unexpected return values from sapply().

Try vapply(flags, unique, numeric(1)), which says that you expect each element of the result to be a numeric vector of length 1. Since this is NOT actually the case, YOU WILL GET AN ERROR. Once you get the error, type ok() to continue to the next question.


```r
#vapply(flags, unique, numeric(1))
```

Recall from the previous lesson that sapply(flags, class) will return a character vector containing the class of each column in the dataset. Try that again now to see the result.


```r
sapply(flags, class)
```

```
##       name   landmass       zone       area population   language   religion 
##   "factor"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
##       bars    stripes    colours        red      green       blue       gold 
##  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
##      white      black     orange    mainhue    circles    crosses   saltires 
##  "integer"  "integer"  "integer"   "factor"  "integer"  "integer"  "integer" 
##   quarters   sunstars   crescent   triangle       icon    animate       text 
##  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
##    topleft   botright 
##   "factor"   "factor"
```

If we wish to be explicit about the format of the result we expect, we can use vapply(flags, class, character(1)). The 'character(1)' argument tells R that we expect the class function to return a character vector of length 1 when applied to EACH column of the flags dataset. Try it now.


```r
vapply(flags, class, character(1))
```

```
##       name   landmass       zone       area population   language   religion 
##   "factor"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
##       bars    stripes    colours        red      green       blue       gold 
##  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
##      white      black     orange    mainhue    circles    crosses   saltires 
##  "integer"  "integer"  "integer"   "factor"  "integer"  "integer"  "integer" 
##   quarters   sunstars   crescent   triangle       icon    animate       text 
##  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
##    topleft   botright 
##   "factor"   "factor"
```

Note that since our expectation was correct (i.e. character(1)), the vapply() result is identical to the sapply() result -- a character vector of column classes.

You might think of vapply() as being 'safer' than sapply(), since it requires you to specify the format of the output in advance, instead of just allowing R to 'guess' what you wanted. In addition, vapply() may perform faster than sapply() for large datasets. However, when doing data analysis interactively (at the prompt), sapply() saves you some typing and will often be good enough.

As a data analyst, you'll often wish to split your data up into groups based on the value of some variable, then apply a function to the
| members of each group. The next function we'll look at, tapply(), does exactly that.

Use ?tapply to pull up the documentation.


```r
?tapply
```

The 'landmass' variable in our dataset takes on integer values between 1 and 6, each of which represents a different part of the world. Use table(flags$landmass) to see how many flags/countries fall into each group.


```r
table(flags$landmass)
```

```
## 
##  1  2  3  4  5  6 
## 31 17 35 52 39 20
```

The 'animate' variable in our dataset takes the value 1 if a country's flag contains an animate image (e.g. an eagle, a tree, a human hand) and 0 otherwise. Use table(flags$animate) to see how many flags contain an animate image.


```r
table(flags$animate)
```

```
## 
##   0   1 
## 155  39
```

This tells us that 39 flags contain an animate object (animate = 1) and 155 do not (animate = 0).

If you take the arithmetic mean of a bunch of 0s and 1s, you get the proportion of 1s. Use tapply(flags$animate, flags$landmass, mean) to apply the mean function to the 'animate' variable separately for each of the six landmass groups, thus giving us the proportion of flags containing an animate image WITHIN each landmass group.


```r
tapply(flags$animate, flags$landmass, mean)
```

```
##         1         2         3         4         5         6 
## 0.4193548 0.1764706 0.1142857 0.1346154 0.1538462 0.3000000
```

The first landmass group (landmass = 1) corresponds to North America and contains the highest proportion of flags with an animate image (0.4194).

Similarly, we can look at a summary of population values (in round millions) for countries with and without the color red on their flag with tapply(flags$population, flags$red, summary).


```r
tapply(flags$population, flags$red, summary)
```

```
## $`0`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    0.00    3.00   27.63    9.00  684.00 
## 
## $`1`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     0.0     0.0     4.0    22.1    15.0  1008.0
```

What is the median population (in millions) for countries *without* the color red on their flag?

1: 22.1

2: 9.0

3: 0.0

4: 27.6

5: 4.0

6: 3.0

**Selection: 6**

Lastly, use the same approach to look at a summary of population values for each of the six landmasses.


```r
tapply(flags$population, flags$landmass, summary)
```

```
## $`1`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    0.00    0.00   12.29    4.50  231.00 
## 
## $`2`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    1.00    6.00   15.71   15.00  119.00 
## 
## $`3`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    0.00    8.00   13.86   16.00   61.00 
## 
## $`4`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.000   1.000   5.000   8.788   9.750  56.000 
## 
## $`5`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    2.00   10.00   69.18   39.00 1008.00 
## 
## $`6`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    0.00    0.00   11.30    1.25  157.00
```

What is the maximum population (in millions) for the fourth landmass group (Africa)?

1: 119.0

2: 157.00

3: 56.00

4: 1010.0

5: 5.00

**Selection: 3**

In this lesson, you learned how to use vapply() as a safer alternative to sapply(), which is most helpful when writing your own functions. You also learned how to use tapply() to split your data into groups based on the value of some variable, then apply a function to each group. These functions will come in handy on your quest to become a better data analyst.

## Looking at Data

Whenever you're working with a new dataset, the first thing you should do is look at it! What is the format of the data? What are the dimensions? What are the variable names? How are the variables stored? Are there missing data? Are there any flaws in the data?

This lesson will teach you how to answer these questions and more using R's built-in functions. We'll be using a dataset constructed from the United States Department of Agriculture's PLANTS Database (http://plants.usda.gov/adv_search.html).

I've stored the data for you in a variable called plants. Type ls() to list the variables in your workspace, among which should be plants.

> ls()

Let's begin by checking the class of the plants variable with class(plants). This will give us a clue as to the overall structure of the
| data.

> class(plants)
[1] "data.frame"

It's very common for data to be stored in a data frame. It is the default class for data read into R using functions like read.csv() and read.table(), which you'll learn about in another lesson.

Since the dataset is stored in a data frame, we know it is rectangular. In other words, it has two dimensions (rows and columns) and fit neatly into a table or spreadsheet. Use dim(plants) to see exactly how many rows and columns we're dealing with.

> dim(plants)
[1] 5166   10

The first number you see (5166) is the number of rows (observations) and the second number (10) is the number of columns (variables).

You can also use nrow(plants) to see only the number of rows. Try it out.

> nrow(plants)
[1] 5166

And ncol(plants) to see only the number of columns.

> ncol(plants)
[1] 10

If you are curious as to how much space the dataset is occupying in memory, you can use object.size(plants).

> object.size(plants)
686080 bytes

Now that we have a sense of the shape and size of the dataset, let's get a feel for what's inside. names(plants) will return a character vector of column (i.e. variable) names. Give it a shot.

> names(plants)
 [1] "Scientific_Name"      "Duration"             "Active_Growth_Period" "Foliage_Color"        "pH_Min"              
 [6] "pH_Max"               "Precip_Min"           "Precip_Max"           "Shade_Tolerance"      "Temp_Min_F"          

We've applied fairly descriptive variable names to this dataset, but that won't always be the case. A logical next step is to peek at the actual data. However, our dataset contains over 5000 observations (rows), so it's impractical to view the whole thing all at once.

The head() function allows you to preview the top of the dataset. Give it a try with only one argument.

> head(plants)
               Scientific_Name          Duration Active_Growth_Period Foliage_Color pH_Min pH_Max Precip_Min Precip_Max Shade_Tolerance
1                  Abelmoschus              <NA>                 <NA>          <NA>     NA     NA         NA         NA            <NA>
2       Abelmoschus esculentus Annual, Perennial                 <NA>          <NA>     NA     NA         NA         NA            <NA>
3                        Abies              <NA>                 <NA>          <NA>     NA     NA         NA         NA            <NA>
4               Abies balsamea         Perennial    Spring and Summer         Green      4      6         13         60        Tolerant
5 Abies balsamea var. balsamea         Perennial                 <NA>          <NA>     NA     NA         NA         NA            <NA>
6                     Abutilon              <NA>                 <NA>          <NA>     NA     NA         NA         NA            <NA>
  Temp_Min_F
1         NA
2         NA
3         NA
4        -43
5         NA
6         NA

Take a minute to look through and understand the output above. Each row is labeled with the observation number and each column with the variable name. Your screen is probably not wide enough to view all 10 columns side-by-side, in which case R displays as many columns as it can on each line before continuing on the next.

By default, head() shows you the first six rows of the data. You can alter this behavior by passing as a second argument the number of rows you'd like to view. Use head() to preview the first 10 rows of plants.

> head(plants, 10)
                     Scientific_Name          Duration Active_Growth_Period Foliage_Color pH_Min pH_Max Precip_Min Precip_Max
1                        Abelmoschus              <NA>                 <NA>          <NA>     NA     NA         NA         NA
2             Abelmoschus esculentus Annual, Perennial                 <NA>          <NA>     NA     NA         NA         NA
3                              Abies              <NA>                 <NA>          <NA>     NA     NA         NA         NA
4                     Abies balsamea         Perennial    Spring and Summer         Green      4    6.0         13         60
5       Abies balsamea var. balsamea         Perennial                 <NA>          <NA>     NA     NA         NA         NA
6                           Abutilon              <NA>                 <NA>          <NA>     NA     NA         NA         NA
7               Abutilon theophrasti            Annual                 <NA>          <NA>     NA     NA         NA         NA
8                             Acacia              <NA>                 <NA>          <NA>     NA     NA         NA         NA
9                  Acacia constricta         Perennial    Spring and Summer         Green      7    8.5          4         20
10 Acacia constricta var. constricta         Perennial                 <NA>          <NA>     NA     NA         NA         NA
   Shade_Tolerance Temp_Min_F
1             <NA>         NA
2             <NA>         NA
3             <NA>         NA
4         Tolerant        -43
5             <NA>         NA
6             <NA>         NA
7             <NA>         NA
8             <NA>         NA
9       Intolerant        -13
10            <NA>         NA

The same applies for using tail() to preview the end of the dataset. Use tail() to view the last 15 rows.

> tail(plants, 15)
                      Scientific_Name  Duration Active_Growth_Period Foliage_Color pH_Min pH_Max Precip_Min Precip_Max Shade_Tolerance
5152                          Zizania      <NA>                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5153                 Zizania aquatica    Annual               Spring         Green    6.4    7.4         30         50      Intolerant
5154   Zizania aquatica var. aquatica    Annual                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5155                Zizania palustris    Annual                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5156 Zizania palustris var. palustris    Annual                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5157                      Zizaniopsis      <NA>                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5158             Zizaniopsis miliacea Perennial    Spring and Summer         Green    4.3    9.0         35         70      Intolerant
5159                            Zizia      <NA>                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5160                     Zizia aptera Perennial                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5161                      Zizia aurea Perennial                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5162                 Zizia trifoliata Perennial                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5163                          Zostera      <NA>                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5164                   Zostera marina Perennial                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5165                           Zoysia      <NA>                 <NA>          <NA>     NA     NA         NA         NA            <NA>
5166                  Zoysia japonica Perennial                 <NA>          <NA>     NA     NA         NA         NA            <NA>
     Temp_Min_F
5152         NA
5153         32
5154         NA
5155         NA
5156         NA
5157         NA
5158         12
5159         NA
5160         NA
5161         NA
5162         NA
5163         NA
5164         NA
5165         NA
5166         NA

After previewing the top and bottom of the data, you probably noticed lots of NAs, which are R's placeholders for missing values. Use summary(plants) to get a better feel for how each variable is distributed and how much of the dataset is missing.

> summary(plants)
                     Scientific_Name              Duration              Active_Growth_Period      Foliage_Color      pH_Min     
 Abelmoschus                 :   1   Perennial        :3031   Spring and Summer   : 447      Dark Green  :  82   Min.   :3.000  
 Abelmoschus esculentus      :   1   Annual           : 682   Spring              : 144      Gray-Green  :  25   1st Qu.:4.500  
 Abies                       :   1   Annual, Perennial: 179   Spring, Summer, Fall:  95      Green       : 692   Median :5.000  
 Abies balsamea              :   1   Annual, Biennial :  95   Summer              :  92      Red         :   4   Mean   :4.997  
 Abies balsamea var. balsamea:   1   Biennial         :  57   Summer and Fall     :  24      White-Gray  :   9   3rd Qu.:5.500  
 Abutilon                    :   1   (Other)          :  92   (Other)             :  30      Yellow-Green:  20   Max.   :7.000  
 (Other)                     :5160   NA's             :1030   NA's                :4334      NA's        :4334   NA's   :4327   
     pH_Max         Precip_Min      Precip_Max         Shade_Tolerance   Temp_Min_F    
 Min.   : 5.100   Min.   : 4.00   Min.   : 16.00   Intermediate: 242   Min.   :-79.00  
 1st Qu.: 7.000   1st Qu.:16.75   1st Qu.: 55.00   Intolerant  : 349   1st Qu.:-38.00  
 Median : 7.300   Median :28.00   Median : 60.00   Tolerant    : 246   Median :-33.00  
 Mean   : 7.344   Mean   :25.57   Mean   : 58.73   NA's        :4329   Mean   :-22.53  
 3rd Qu.: 7.800   3rd Qu.:32.00   3rd Qu.: 60.00                       3rd Qu.:-18.00  
 Max.   :10.000   Max.   :60.00   Max.   :200.00                       Max.   : 52.00  
 NA's   :4327     NA's   :4338    NA's   :4338                         NA's   :4328    

summary() provides different output for each variable, depending on its class. For numeric data such as Precip_Min, summary() displays the minimum, 1st quartile, median, mean, 3rd quartile, and maximum. These values help us understand how the data are distributed.

For categorical variables (called 'factor' variables in R), summary() displays the number of times each value (or 'level') occurs in the data. For example, each value of Scientific_Name only appears once, since it is unique to a specific plant. In contrast, the summary for Duration (also a factor variable) tells us that our dataset contains 3031 Perennial plants, 682 Annual plants, etc.

You can see that R truncated the summary for Active_Growth_Period by including a catch-all category called 'Other'. Since it is a categorical/factor variable, we can see how many times each value actually occurs in the data with table(plants$Active_Growth_Period).

> table(plants$Active_Growth_Period)

Fall, Winter and Spring                  Spring         Spring and Fall       Spring and Summer    Spring, Summer, Fall 
                     15                     144                      10                     447                      95 
                 Summer         Summer and Fall              Year Round 
                     92                      24                       5 

Each of the functions we've introduced so far has its place in helping you to better understand the structure of your data. However, we've left the best for last....

Perhaps the most useful and concise function for understanding the *str*ucture of your data is str(). Give it a try now.

> str(plants)
'data.frame':	5166 obs. of  10 variables:
 $ Scientific_Name     : Factor w/ 5166 levels "Abelmoschus",..: 1 2 3 4 5 6 7 8 9 10 ...
 $ Duration            : Factor w/ 8 levels "Annual","Annual, Biennial",..: NA 4 NA 7 7 NA 1 NA 7 7 ...
 $ Active_Growth_Period: Factor w/ 8 levels "Fall, Winter and Spring",..: NA NA NA 4 NA NA NA NA 4 NA ...
 $ Foliage_Color       : Factor w/ 6 levels "Dark Green","Gray-Green",..: NA NA NA 3 NA NA NA NA 3 NA ...
 $ pH_Min              : num  NA NA NA 4 NA NA NA NA 7 NA ...
 $ pH_Max              : num  NA NA NA 6 NA NA NA NA 8.5 NA ...
 $ Precip_Min          : int  NA NA NA 13 NA NA NA NA 4 NA ...
 $ Precip_Max          : int  NA NA NA 60 NA NA NA NA 20 NA ...
 $ Shade_Tolerance     : Factor w/ 3 levels "Intermediate",..: NA NA NA 3 NA NA NA NA 2 NA ...
 $ Temp_Min_F          : int  NA NA NA -43 NA NA NA NA -13 NA ...

The beauty of str() is that it combines many of the features of the other functions you've already seen, all in a concise and readable format. At the very top, it tells us that the class of plants is 'data.frame' and that it has 5166 observations and 10 variables. It then gives us the name and class of each variable, as well as a preview of its contents.

str() is actually a very general function that you can use on most objects in R. Any time you want to understand the structure of something (a dataset, function, etc.), str() is a good place to start.

In this lesson, you learned how to get a feel for the structure and contents of a new dataset using a collection of simple and useful functions. Taking the time to do this upfront can save you time and frustration later on in your analysis.
