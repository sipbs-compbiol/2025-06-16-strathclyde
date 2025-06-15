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

- To understand `data.frame`s, we first need to know about a different data structure called a _vector_