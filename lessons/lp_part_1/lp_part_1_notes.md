# Data Structures and Data Frames

## Preflight

1. Ensure that `RStudio`/`R` is open with the appropriate packages installed (`tidyverse`)

----------

## Learning Questions

- This lesson might sound like it's going to be a bit dry: "Why should I care about data types and structures?"

- **IF YOU UNDERSTAND YOUR DATA, AND YOU UNDERSTAND HOW `R` SEES YOUR DATA, YOUR ANALYSIS WILL BE MUCH EASIER AND MORE EFFECTIVE**

- Some questions you might have had before this session may include those on the slide:
  - How can I read data in `R`?
  - What are the basic data types in `R`?
  - How do I represent categorical information in `R`?

- This session aims to help you answer those

----------

## Learning Objectives

- In this section, you'll be learning about the data types in `R`:
  - **WHAT DATA IS**
- You'll be learning about data *structures*:
  - **WHAT DATA IS BUILT INTO - HOW IT IS ARRANGED**
  
- You'll also learn how to find out what type/structure a particular piece of data has
  - Putting it together, you'll see how `R`'s data types and structures relate to the types of data that you work with, yourself.
  - This will help you work much more fluently in `R`

- In particular, we'll be working with **DATA FRAMES** - this is the workhorse data structure in `R` and, when you use `R`, you will see a _lot_ of them.

----------

## Data Types and Structures in `R`

- `R` is **MOSTLY USED FOR DATA ANALYSIS**
- `R` is set up with key, core data types designed to help you work with your own data
- A lot of the time, `R` focuses on tabular data.
- **`R` is very powerful when dealing with tabular data**
  
- **INTERACTIVE DEMO**
- **SWITCH TO THE CONSOLE** 

- Let's start by making a toy dataset.
- We'll eventually save this in your `data/` directory, with the name `feline-data.csv`
- Use the console in `RStudio`

- We'll create a new variable called `cats`
  - This holds a `data.frame`

```R
cats <- data.frame(coat = c("calico", "black", "tabby"),
                    weight = c(2.1, 5.0, 3.2),
                    likes_catnip = c(1, 0, 1))
```

- We can now save the content of the variable `cats` as a CSV (comma-separated variable) file.
  - We use the `write.csv()` function 
  - It is useful to call argument names explicitly so that the code is more readable

```R
write.csv(cats, file = "data/feline-data.csv", row.names = FALSE)
```

- We're writing the `cats` data out to file
  - `file` specifies the filename we're writing to
  - `row.names` tells `R` not to give the rows in the table a name

- **USE THE FILES TAB/VIEW FILES TO VIEW THE CONTENTS OF THE NEW FILE**

- This is a plain text file, and you could have created it using a text editor like Nano or Notepad.
  - The first row is the header row
  - Each individual cat gets its own row
  - Each column is a different kind of data
  - Each column is separated by a comma

- We can load the data from a CSV file in a similar way to how we write it
  - We can use the `read.csv()` function
  - Inspect the dataset by using the variable name

```R
cats <- read.csv(file = "data/feline-data.csv")
cats
    coat weight likes_catnip
1 calico    2.1            1
2  black    5.0            0
3  tabby    3.2            1
```

- **THINK ABOUT THE DATA TYPES** Are they all the same?
    - **NO** `coat` is text; `weight` is some real value (in kg or pounds, maybe), and `likes_string` looks like it should be `TRUE`/`FALSE` but is represented as 1s and 0s
    - **DOES IT MAKE SENSE TO WORK WITH EACH OF THESE ELEMENTS OF DATA AS IF THEY'RE THE SAME THING?** (No)

- Let's explore our dataset
- **EXTRACT A COLUMN FROM A TABLE**
    - Use `$` notation in the console
    - **NOTE THE AUTOCOMPLETION**

```R
> cats$weight
[1] 2.1 5.0 3.2
> cats$coat
[1] "calico" "black"  "tabby" 
```

- **WHAT DID `R` RETURN?**
 - A *vector* (1D ordered collection) of numbers or character strings
- **WE CAN OPERATE ON THESE *VECTORS***
  - *Vectors* are an important concept, and `R` is largely built so that operations on vectors are central to data analysis.

- Suppose that we discover the scales used to weigh the cats is miscalibrated
  - Every cat weight is underestimated by 2kg
  - We can correct all the weights at once

```R
> cats$weight + 2
[1] 4.1 7.0 5.2
```

- Suppose we want to make a string out of each of the coat types:

```R
> paste("My cat is", cats$coat)
[1] "My cat is calico" "My cat is black"  "My cat is tabby" 
```

- **What if we try to combine these columns?**

```R
> cats$weight + cats$coat
Error in cats$weight + cats$coat : 
  non-numeric argument to binary operator
```

- **WE HIT AN ERROR**
  - You probably already realised that wasn't going to work, because adding "calico" to "2.1" is nonsense.
    - **You already have intuition about this**
  - **THESE DATA TYPES ARE NOT COMPATIBLE** for addition
  - `R`'s data types reflect the ways in which data is expected to interact

- **UNDERSTANDING HOW YOUR OWN DATA MAP TO `R`'s DATA TYPES IS KEY**
  - It's very important to understand how `R` sees your data (**you want `R` to see your data the same way you do**)
  - Many problems in `R` come down to incompatibilities between data and data types.

----------

## What Data Types Do You Expect?

- **ASK THE STUDENTS**
    - What data types would you expect to see?
    - What data types do you think you would **WANT OR NEED**, from your own experience?
- **SPEND A COUPLE OF MINUTES ON THIS**
    - The difference between a *data type* and a *data structure*

----------

## Data Types in `R`

- `R`'s data *types* are *atomic*: they are **FUNDAMENTAL AND EVERYTHING ELSE IS BUILT UP FROM THEM**, the same way matter is built up from atoms
  - In particular, all the data *structures* are built up from data *types*
- There are only **FIVE DATA TYPES** in `R` (though one is split into two…) - three that matter here and two we won't deal with
  - **logical**: Boolean, True/False (also `1`/`0`)
  - **numeric**: anything that's a number on the number line; two types of number are supported: `integer` and `double` (real)
  - **character**: text data - readable symbols
- The other two are less relevant to you and we'll not deal with them much today
  - **complex**: complex numbers, defined on the 2D plane
  - **raw**: binary data

- **LET'S LEARN A BIT MORE ABOUT THEM IN THE DEMO**

- We can ask what type of data something is with the `typeof()` function

```R
> typeof(cats$weight)
[1] "double"
> typeof(TRUE)
[1] "logical"
> typeof(3.14)
[1] "double"
> typeof(1)
[1] "double"
```

- This might seem unintuitive: `1` is an integer
  - But `R` treats all numbers as `double`s (i.e. "Real" numbers) by default
  - To force `R` to see a value as an integer, we need to add an `L` suffix

```R
> typeof(1L)
[1] "integer"
> typeof(cats$coat)
[1] "character"
> typeof("Irn Bru")
[1] "character"
```

- No matter how complicated your analysis gets, _all_ data in `R` is interpreted as one of these basic data types.
  - `R` - like most computer languages - is strict about data types, and this has important consequences.

- **SUPPOSE OUR FRIEND WANTS TO ADD DETAILS OF THEIR OWN CAT TO OUR DATASET**
  - We can create a new dataframe to hold this data

```R
> additional_cat <- data.frame(coat = "tabby", weight = "2.3 or 2.4", likes_catnip = 1)
> additional_cat
   coat     weight likes_catnip
1 tabby 2.3 or 2.4   
```

- **We can "stack" dataframes using the function `rbind`**

```R
> cats2 <- rbind(cats, additional_cat)
> cats2
    coat     weight likes_catnip
1 calico        2.1            1
2  black          5            0
3  tabby        3.2            1
4  tabby 2.3 or 2.4            1
```

- Initially, everything looks OK - but we've silently caused some problems.
  - Let's look at the data type of `cats$weight` in the two dataframes

```R
> typeof(cats$weight)
[1] "double"
> typeof(cats2$weight)
[1] "character"
```

- Our friend gave us a different data type in the `weight` column, and this means we can no longer do the same weight adjustment (adding on 2kg) that we did before:

```R
> cats2$weight + 2
Error in cats2$weight + 2 : non-numeric argument to binary operator
```

----------

## A Quick Note About Dataframes

- Any column in a dataframe can contain only one datatype.
- Initially, the datatype of `cats$weight` was `double`
- When we added the new cat, the data in that column was `character` - i.e. a string
- Because we can always represent a number as a string, but we can't always represent a string as a number, `R` chose to _coerce_ the datatype from `double` to `character` for that column.

- **INTERACTIVE DEMO**
- We can look at the structure of a dataframe using the `str()` function

```R
> str(cats)
'data.frame':	3 obs. of  3 variables:
 $ coat        : chr  "calico" "black" "tabby"
 $ weight      : num  2.1 5 3.2
 $ likes_catnip: int  1 0 1
> str(cats2)
'data.frame':	4 obs. of  3 variables:
 $ coat        : chr  "calico" "black" "tabby" "tabby"
 $ weight      : chr  "2.1" "5" "3.2" "2.3 or 2.4"
 $ likes_catnip: num  1 0 1 1
```

- Here the `str()` function tells us that `cats` and `cats2` are both `data.frame`s - a very common kind of data structure in `R`
  - All data frames are composed of rows and columns.
  - Every column has the same number of rows
  - Every column has one and only one data type
  - Each column can be a different data type

- To understand `data.frame`s, a bit better, let's meet a different data structure called a _vector_

----------

## Vectors

- These are the **MOST COMMON DATA STRUCTURE**
  - Vectors are an ordered collection of data values
  - Vectors can contain **ONLY A SINGLE DATA TYPE** (*atomic vectors*)

- **INTERACTIVE DEMO**

- Let's define an **ATOMIC VECTOR OF NUMBERS**
  - To create a vector **USE THE `c()` FUNCTION** (`c()` is `combine`; use `?c`)

```R
> x <- c(10, 12, 45, 33)
> x
[1] 10 12 45 33
```

- Let's check the data type, and what kind of structure we have

```R
> typeof(x)
[1] "double"
> length(x)
[1] 4
> str(x)
 num [1:4] 10 12 45 33
```

- This is a little cryptic as output
  - We can see from `typeof()` that the vector contains the `double` type
  - The output of `str()` tells us:
    - it's a `numeric` (`num`) vector - which includes `double` and `integer` datatypes
    - there are four elements `[1:4]` in the vector
    - the first few datapoint in the vector

- `str()` tells us that the `cats$coat` column is a vector, too:

```R
> str(cats$coat)
 chr [1:3] "calico" "black" "tabby"
```

----------

## Coercion

- **INTERACTIVE DEMO**

- Given what we've done so far, what do you think the following will produce when we use `typeof()`?
  - **PAUSE FOR STUDENT SUGGESTIONS**

```R
> quiz_vector <- c(2,6,'3')
> typeof(quiz_vector)
[1] "character"
```

- Here, `R` has enforced that the type of the vector is `character` (string), because we can always represent numbers as strings, but we can't always represent strings as numbers

- **NEXT CALLOUTS**
- This is called _type coercion_ and can cause surprises in your code
  - It is a key reason why you need to be aware of the basic data types and how `R` interprets them.

- **INTERACTIVE DEMO**

- Consider these two vectors

```R
> coercion_vector <- c('a', TRUE)
> coercion_vector
[1] "a"    "TRUE"
> another_coercion_vector <- c(0, TRUE)
> another_coercion_vector
[1] 0 1
```

- What do you expect their datatypes to be?

```R
> typeof(coercion_vector)
[1] "character"
> typeof(another_coercion_vector)
[1] "double"
```

----------

## Coercion

- *Coercion* is what happens when you **CONVERT ONE DATA TYPE INTO ANOTHER**
- If `R` thinks it needs to, it will **COERCE DATA IMPLICITLY** without telling you
- There is a set order for coercion
  - `logical` can be coerced to `integer`, but `integer` cannot be coerced to `logical`
  - That's because `integer` can describe all `logical` values, but not *vice versa*
  - Everything can be represented as a `character`, so that's the fallback position for `R`
- **IF THERE'S A FORMATTING PROBLEM IN YOUR DATA, `R` MIGHT CONVERT THE TYPE TO COPE**
  - `R` will choose the simplest data type that can represent all items in the vector

- **INTERACTIVE DEMO IN CONSOLE** More useful things to do with vectors
- You can (usually) **COERCE VECTORS MANUALLY** with `as.<type>()`

```R
> x
[1] 10 12 45 33
> as.character(x)
[1] "10" "12" "45" "33"
> as.complex(x)
[1] 10+0i 12+0i 45+0i 33+0i
> as.logical(x)
[1] TRUE TRUE TRUE TRUE
```

- Surprising things can happen when `R` forces one data type into another.
- **If your data doesn't look how you expect it to, _type coercion_ may be to blame**
  - Check your data formatting (is there an accidental string/character?)

- Let's look at our `cats` dataframe again
  
```R
> cats
    coat weight likes_catnip
1 calico    2.1            1
2  black    5.0            0
3  tabby    3.2            1
> typeof(cats$likes_catnip)
[1] "integer"
```

- The type of the `likes_catnip` column is recorded as an integer, but we want `logical` `TRUE`/`FALSE` values.
  - We can use the `as.logical()` function to change this

```R
> cats$likes_catnip <- as.logical(cats$likes_catnip)
> cats
    coat weight likes_catnip
1 calico    2.1         TRUE
2  black    5.0        FALSE
3  tabby    3.2         TRUE
> str(cats)
'data.frame':	3 obs. of  3 variables:
 $ coat        : chr  "calico" "black" "tabby"
 $ weight      : num  2.1 5 3.2
 $ likes_catnip: logi  TRUE FALSE TRUE
```

----------

## Lists

- `list`s are data structures like *vectors*, **EXCEPT THEY CAN HOLD ANY DATA TYPE**
  - They are not constrained to atomic types
  - They do not coerce their contents' datatypes

- Let's create a new list using the `list()` function

```R
> l <- list(1, 'a', TRUE, seq(2, 5))
> length(l)
[1] 4
> l
[[1]]
[1] 1

[[2]]
[1] "a"

[[3]]
[1] TRUE

[[4]]
[1] 2 3 4 5
```

- Individual elements in the list are identified with **DOUBLE SQUARE BRACKETS**
  - There are four elements
  - `[[1]]` is the number `1`
  - `[[2]]` is the character `"a"`
  - `[[3]]` is the logical value `TRUE`
  - `[[4]]` is the vector of values from 2 to 5
- We can extract a single element from the list with the double square bracket notation

```R
> l[[1]]
[1] 1
> l[[4]]
[1] 2 3 4 5
```

- Using the `str()` function we can see the datatypes of all the elements in the list

```R
> str(l)
List of 4
 $ : num 1
 $ : chr "a"
 $ : logi TRUE
 $ : int [1:4] 2 3 4 5
```

- **The elements of a list can also have names**
  - We specify the name when we create the list

```R
> l_named <- list(a = "SWC", b = 1:4)
> l_named
$a
[1] "SWC"

$b
[1] 1 2 3 4
```

- We can use the name of each element to retrieve it, with the `$` notation:

```R
> l_named$a
[1] "SWC"
> l_named$b
[1] 1 2 3 4
```

- But it's still a list like any other, and we can use the double square bracket notation.

```R
> l_named[[1]]
[1] "SWC"
> l_named[[2]]
[1] 1 2 3 4
> str(l_named)
List of 2
 $ a: chr "SWC"
 $ b: int [1:4] 1 2 3 4
```

----------

## Let's Look At A Data Frame

- We didn't go into detail about the `cats` dataframe we created at the start of this episode.
  - But let's look at it more closely

```R
> cats
    coat weight likes_catnip
1 calico    2.1         TRUE
2  black    5.0        FALSE
3  tabby    3.2         TRUE
> typeof(cats)
[1] "list"
> cats[[2]]
[1] 2.1 5.0 3.2
> typeof(cats$weight)
[1] "double"
```

- So `cats` is a `list`, and each element in the list is a vector
- But `cats` is a **special kind of list** - a `data.frame` - where all the vectors have the same length.
  - We can see that this is a special kind of list by using the `class()` function

```R
> class(cats)
[1] "data.frame"
> class(l)
[1] "list"
```

- The class `data.frame` represents a standard way of organising data:
  - Each _row_ is an observation
  - Each _column_ is a variable
  - The data frame represents a series of observations
