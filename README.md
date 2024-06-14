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

wday(aba[1])

wday(aba[1], label=TRUE)
*** Practice more with lubridate***
