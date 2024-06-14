# clean
About this session
Sequel to our earlier discussions on Tidy Data, we will learn about data cleaning, covering handling of strings and dates.

Changing character case

Splitting characters

vectorisation with vector

Necessary to have understood

Gathering Data

R Programming

Session will be useful

Data Exploration

Reporting/Reproducing Data Analysis

Character vectors - tolower(), toupper()
Recalling our learning from the last session, we should be able to acquire data from some sources (including) from the web. We can attempt

# if(!file.exists("./data")){dir.create("./data")}
# fileUrl <- "FILE_LINK"
# download.file(fileUrl,destfile="./data/FILENAME",method="curl")

ambulancesData <- read.csv("./data/nigeria_ambulances.csv")
names(ambulancesData)
tolower(names(ambulancesData))
Splitting character vectors
Good for automatically splitting variable names
Important parameters: x, split
ambulancesData <- read.csv("./data/nigeria_ambulances.csv")

splitNames = strsplit(names(ambulancesData), "\\_")

splitNames[[5]]

splitNames[[6]]
Subsetting - lists
eloho <- list(letters = c("Serious", "Studious", "Stable", "Others"), numbers = 1:6, matrix(1:12, ncol = 3))

head(eloho)

eloho[1]

eloho$letters

eloho[[1]]
Vectorisation with characters
Applies a function to each element in a vector or list
Important parameters: X, FUN
ambulancesData <- read.csv("./data/nigeria_ambulances.csv")

splitNames = strsplit(names(ambulancesData), "\\_")

splitNames[[6]][1]

firstElement <- function(x){x[1]}

sapply(splitNames,firstElement)
Substituting character vectors
Important parameters: pattern, replacement, x
ambulances <- read.csv("./data/nigeria_ambulances.csv")

head(ambulances,4)

names(ambulances)

sub("_", "", names(ambulances),)
String substitution
maria <- "so_long_a_letter"

sub("_", "", maria)

gsub("_", "", maria)
The sub() function applies for the first match. The gsub() function applies for all matches

Finding values
ambulances <- read.csv("./data/nigeria_ambulances.csv")

grep("Lagos", ambulances$state_name)

table(grepl("Lagos", ambulances$state_name))

ambulances2 <- ambulances[!grepl("Lagos",ambulances$state_name),]

ambulances2
The grep returns matched items themselves while grepl returns a logical vector with TRUE to represent a match and FALSE otherwise

More on grep()
ambulances <- read.csv("./data/nigeria_ambulances.csv")

grep("Lagos", ambulances$state_name, value=TRUE)

grep("Abia", ambulances$state_name)

length(grep("Abia", ambulances$state_name))
More useful string functions
library(stringr)

sa <- "Senator Adetokunbo"

nchar(sa)

sa_b4 <- substr(sa, 8,18)

sa_b4 

sa_ft <- paste("HRM", sa_b4)

sa_ft
More useful string functions
paste0("Senator","Adetokunbo")

str_trim("Senator      ")
String replace
ambulances <- read.csv("./data/nigeria_ambulances.csv")

ambulances2 <- ambulances %>% 
  mutate(state_name = str_replace(state_name, "Lagos", "Oduduwa"))

ambulances2
Replace multiple strings at a time
ambulances <- read.csv("./data/nigeria_ambulances.csv")

rep_str = c('Abia'='South East','Anambra'='South East','Taraba'='North East')

ambulances$state_name <- str_replace_all(ambulances$state_name, rep_str)

ambulances
Practice:
*** Select relevant rows & columns and practice what you have learnt***

Focus on the abstract
head(ai_papers_sample)
Further Practice
Focus on the abstract

#ai_papers_sample

table(grepl("AI", ai_papers_sample$abstract))

ai2 <- ai_papers_sample[!grepl("AI", ai_papers_sample$abstract),]

ai2
Practice with iris data
Focus on variable names

head(iris)
Notes on text in data sets
Names of variables:

Use lower case when possible

Avoid duplicateion

Avoid dots, white spaces and underscore

Variables with character values

Treat variables with character as factors (depending on application)

Make use of descriptive (TRUE/FALSE instead of 0/1, Male/Female instead of 0/1 or M/F)

Handling Dates in R
"Date-time data can be frustrating to work with in R."

"R commands for date-times are generally unintuitive and change depending on the type of date-time object being used."

"Methods we use with date-times must be robust to time zones, leap days, daylight savings times, and other time related quirks."

Source: Lubridate Team (Github/tidyverse/lubridate)

Date class
Let’s recall our earlier discussions on dates (Data Structure)

yomi = Sys.Date()

yomi

class(yomi)
Understanding dates formats
%d = day as number (0-31)
%a = weekday abbreviated
%A = weekday unabbreviated
%m = month (00-12)
%b = month abbreviated
%B = month unabbrevidated
%y = 2 digit year
%Y = four digit year
yomi = Sys.Date()

format(yomi,"%a %b %d")
Creating dates
aba = c("14feb2023", "22feb2023", "18jun2023", "28jun2023", "14jul2023")

layi = as.Date(aba, "%d%b%Y")

layi

layi[2] - layi[1]

as.numeric(layi[2]-layi[1])
Converting to Julian
yomi = Sys.Date()

weekdays(yomi)

months(yomi)

julian(yomi)
Lubridate
library(lubridate)

ymd("230628")

mdy("06/28/2023")

dmy("28-06-2023")
Handling times in dates
ymd_hms("2011-08-03 10:15:03")

ymd_hms("2011-08-03 10:15:03",tz="GMT")

?Sys.timezone
Syntax differences in dates
aba = dmy(c("14feb2023", "22feb2023", "18jun2023", "28jun2023", "14jul2023"))











Functions in R Programming
In this sub-section, we will learn:

What a function in R is?

Purpose of functions

Writing functions

Calling functions

Built-in functions

Creating user-defined functions

Arguements

Vectorisation

Function in R?
A function is a block of code that performs a specific task. R treats functions as objects. The interpreter can pass control to them along with the arguments required by the function. Once the function has achieved its objective(s), it passes control back to the interpreter.

Syntax of function
The following is the syntax for a user-defined function in R:

Function_name <- function(arguments){

  function_body

  return (return)
}
Where:

function_name = name of the function,

arguments = input arguments needed by the function,

function_body = body of the function,

return = value the function should return.

Let’s briefly look at each component of the syntax.

Components of R function
The following are the components of a function in R. A function may not necessarily have all or some of them.

Function name:
Every function needs a name. The name is used to call the function from other parts of the program. This is because R stores a function as an object with the name assigned to it.

Arguments:
Arguments are placeholders for the inputs a function may require. When we call a function, we need to provide the proper values for all the arguments the function needs. A function may or may not necessarily have arguments.

Function body:
A function’s body contains a set of instructions/statements. A function completes its task by running the code inside its body.

Return value:
After the function has executed its task, it may or may not return a value. The returned value can be the result of the task done by the function or a confirmation about the completion/failure of the task.

The purpose of a function
The purpose of a function is to simplify the process of performing a specific task. - Instead of writing several lines of code to carryout a specific task, we can use a function to perform the same task saving time. It saves us time and saves us from repeating the same task multiple times (take a case of a number multiplying itself 4 times).

Writing functions
Recalling is intentional, to underscore multiple arguements

Recall the components of writting a function in R:

    function_name <- function(parameter1, parameter2, ...)
      {
  
      # code goes here
  
    }
Example.
Suppose we want to create a function named helloSAIL that prints a welcome message for participant in this fellowship.

*** Function with no arguement***

helloSAIL <- function()
  {
 
  # Print message
  print(paste("You are welcome to this cohort of SAIL, Data Science for society")
  
  }
Once created, the function can be called with the syntax:

helloSAIL()
Calling Functions
To call a function, we use the function’s name along with parentheses. For our example above:

helloSAIL <- function()
  {
 
  # Print message
  
  print("You are welcome to this cohort of SAIL, Data Science for society")
  
  }

helloSAIL()
*** Function with arguement***

leke <- function(niyi)
  {
  
  return (niyi ^ 4)
  
}

# We can call the `leke()` function by passing any value as an argument:

leke(10)
Built-in functions
In addition to us being able to create custom functions, R also provides many built-in functions (recall mean(), median()). These functions are already defined and can be used to simplify tasks - (lm()).

Exercise
Create a numeric vector, give it a name you want. Use some in-built functions on the vector.

Calling R function?
When a function has been created, we can call it from anywhere in the environment in which the function is declared.

*** Please, read more on global and local environments.***

To call a function, we simply have to use the function’s name and provide appropriate arguments. For example:

function_name(arguments)

Where function_name is the name of the function you want to call,

and arguments are the arguments needed by the function.

If the function doesn’t need any arguments, the parenthesis can be left empty.

When do you need the R function? We need a function if we need to run the same instruction more than once.

Refresh
Quiz - Control Structure Type
Refreshing materials on Functions
Functions are created using the function() directive and are stored as R objects just like anything else. In particular, they are R objects of class “function”.

f <- function(<arguments>) {
        ## Do something interesting
}
Functions in R are what we can call “special/first class objects”, They can be treated much like any other R object. To be noted:

They can be passed as arguments to other functions

Functions can be nested i.e., we can have a function inside another function

Their return value is the last expression in the function body to be evaluated.

Function Arguments
Functions have named arguments which potentially have default values.

The formal arguments are the arguments included in the function definition

The formals function returns a list of all the formal arguments of a function

Not every function call in R makes use of all the formal arguments

Function arguments can be missing or might have default values

Argument Matching
This is serious as we will begin to use various in-built function.

R functions arguments can be matched by position or by name. The following calls to to use standard deviation (), sd are all equivalent


set.seed(234)

sample_data <- rnorm(100)

sd(sample_data)

sd(x = sample_data)

sd(x = sample_data, na.rm = FALSE)

sd(na.rm = FALSE, x = sample_data)

sd(na.rm = FALSE, sample_data)
We should be careful with messing around with the order of arguments in functions (as it may lead to some confusions).

Argument Matching 1
You can mix positional matching with matching by name. When an argument is matched by name, it is “taken out” of the argument list and the remaining unnamed arguments are matched in the order that they are listed in the function definition.


args(lm)
function (formula, data, subset, weights, na.action,
          method = "qr", model = TRUE, x = FALSE,
          y = FALSE, qr = TRUE, singular.ok = TRUE,
          contrasts = NULL, offset, ...)
          
The following two calls are equivalent.


lm(data = yourdata, y ~ x, model = FALSE, 1:100)
lm(y ~ x, yourdata, 1:100, model = FALSE)
Argument Matching 2
Most of the time, named arguments are useful on the command line when you have a long argument list and you want to use the defaults for everything except for an argument near the end of the list

Named arguments also help if you can remember the name of the argument and not its position on the argument list (plotting is a good example).

Argument Matching 3
Function arguments can also be partially matched, which is useful for interactive work. The order of operations when given an argument is

Check for exact match for a named argument
Check for a partial match
Check for a positional match
Defining a Function
your_function_name <- function(abuja, b = 1, c = 2, d = NULL) {

}
In addition to not specifying a default value, you can also set an argument value to NULL.

Lazy Evaluation
Arguments to functions are evaluated lazily, so they are evaluated only as needed.

f <- function(a, b) {
        a^2
} 
f(2)
This function never actually uses the argument b, so calling f(2) will not produce an error because the 2 gets positionally matched to a.

Lazy Evaluation
f <- function(a, b) {
        print(a)
        print(b)
}
f(234)
Running the code will throw an error. However, we should observe that “234” or whatever value/arguement provided, will be printed before the error is triggered. This is because b is evaluated after print(a) instruction. The moment, the function tries to evaluate print(b) it will throw an error.

Vectorisation in R
This segment is to introduce the concept of vectorisation and some of the related functions in R. Vectorisation is a process of creating a single vector (a list of elements) from a set of smaller vectors (elements) using “apply (and related) functions”.

The tutorial covers the use of the following functions:

lapply
mapply
sapply
tapply
lapply
The lapply() function allows you to perform an operation on a list of elements. It returns a list of results, each of which is the result of the operation performed on each element in the list.

For example, if we have a list x:

x <- list(1,2,3,4,5,6,7,8,9,10)

We can perform an operation on each element x using lapply:

y <- lapply(x, function(n) n^2)

This returns a list of the result of the operation multipling each element by itself:

Verify lapply
x <- list(c(1:10))

y <- lapply(x, function(n) n^2)

y
[[1]]  [1]   1   4   9  16  25  36  49  64  81 100

mapply
The mapply() function is similar to lapply(), but it allows us to pass multiple arguments to our function(s).

For instance, if we have two lists x and y:

`x<-list(1,2,3,4)

y<-list(10,20,30,40)`

We can perform an operation on each element of the two lists using mapply:

Verify mapply
x <- list(c(1:10))

y <- lapply(x, function(n) n^2)

z <- mapply(function(a, b) a*b, x, y)

z
This returns a list of the result of the operation multipling each corresponding element from the two lists:

> z       [,1]  [1,]    1  [2,]    8  [3,]   27  [4,]   64  [5,]  125  [6,]  216  [7,]  343  [8,]  512  [9,]  729 [10,] 1000

sapply
The sapply() is similar to lapply(), but it is used when the function being applied returns a vector or list.

For example, if we recall our list x:

x <- list(c(1:10))

We can perform an operation on each element of the list using sapply:

y <- sapply(x, function(n) n*2)

This returns a vector of the result of the operation multipling each element by itself:

Verify sapply
x <- list(c(1:10))

y <- sapply(x, function(n) n^2)

y
> y       [,1]  [1,]    1  [2,]    4  [3,]    9  [4,]   16  [5,]   25  [6,]   36  [7,]   49  [8,]   64  [9,]   81 [10,]  100

tapply
The tapply() function is used to apply a function to each sub-group of a data set.

For example, if you have the vector x:

x<-c(1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4, 4)

You can apply a function to each group of this vector using the tapply function:

y <- tapply(x, x, function(n) n*2)

Verify tapply
x<-c(1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4, 4)

y1 <- tapply(x, x,  function(n) n*2)

y1

y2 <- tapply(x, x, function(n) n*n)

y2
This returns a vector of the result of the operation (y1) multipling each group element by two and (y2) squaring each group element.

> y1 $1` [1] 2 2 2

$2 [1] 4 4 4

$3 [1] 6 6 6

$4 [1] 8 8 8 8 `

> y2 $1` [1] 1 1 1

$2 [1] 4 4 4

$3 [1] 9 9 9

$4 [1] 16 16 16 16`
wday(aba[1])

wday(aba[1], label=TRUE)
*** Practice more with lubridate***
