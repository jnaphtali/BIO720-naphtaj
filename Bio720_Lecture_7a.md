---
title: "In class assignment week 2, part 2. A worked example using control flow (for loops, if statements, etc)"
author: "James Naphtali"
output: 
  html_document:
    keep_md: yes
    number_sections: no
---

# Introduction
Let's do a little exercise integrating some of the things we have learned. Here are some Illumina HiSeq reads for one of our recent projects:


```r
read_1 <- "CGCGCAGTAGGGCACATGCCAGGTGTCCGCCACTTGGTGGGCACACAGCCGATGACGAACGGGCTCCTTGACTATAATCTGACCCGTTTGCGTTTGGGTGACCAGGGAGAACTGGTGCTCCTGC"

read_2 <- "AAAAAGCCAACCGAGAAATCCGCCAAGCCTGGCGACAAGAAGCCAGAGCAGAAGAAGACTGCTGCGGCTCCCGCTGCCGGCAAGAAGGAGGCTGCTCCCTCGGCTGCCAAGCCAGCTGCCGCTG"

read_3  <- "CAGCACGGACTGGGGCTTCTTGCCGGCGAGGACCTTCTTCTTGGCATCCTTGCTCTTGGCCTTGGCGGCCGCGGTCGTCTTTACGGCCGCGGGCTTCTTGGCAGCAGCACCGGCGGTCGCTGGC"
```

####Question 1. what species are these sequences from?
  #Drosphila melanogaster.

####Question 2. Put all three of these reads into a single object (a vector).  What class will the vector `reads` be? Check to make sure! How many characters are in each read (and why does `length()` not give you what you want.. try...)

```r
reads <- c(read_1, read_2, read_3)
class(reads)
```

```
## [1] "character"
```

```r
length(reads)
```

```
## [1] 3
```
    #Length does not provide the correct read length as converting the reads into a single vector results in R treating each read as a single element of a vector. 

#####Question 3. Say we wanted to print each character (not the full string) from read_1, how do we do this using a for loop? You may wish to look at a function like `strsplit()` to accomplish this (there are other ways.)


```r
read_1_split <- strsplit(read_1, split = character(0), fixed = T) #split combined vector of reads into individual base reads as elements of new list read_1_split. Use parameter character(0) for the split argument to use null characters as the element identifier (credit to Jerry for this idea!)

for (base in read_1){
  base <- strsplit(base, split = character(0), fixed = T)
  print(base)
}
```

```
## [[1]]
##   [1] "C" "G" "C" "G" "C" "A" "G" "T" "A" "G" "G" "G" "C" "A" "C" "A" "T"
##  [18] "G" "C" "C" "A" "G" "G" "T" "G" "T" "C" "C" "G" "C" "C" "A" "C" "T"
##  [35] "T" "G" "G" "T" "G" "G" "G" "C" "A" "C" "A" "C" "A" "G" "C" "C" "G"
##  [52] "A" "T" "G" "A" "C" "G" "A" "A" "C" "G" "G" "G" "C" "T" "C" "C" "T"
##  [69] "T" "G" "A" "C" "T" "A" "T" "A" "A" "T" "C" "T" "G" "A" "C" "C" "C"
##  [86] "G" "T" "T" "T" "G" "C" "G" "T" "T" "T" "G" "G" "G" "T" "G" "A" "C"
## [103] "C" "A" "G" "G" "G" "A" "G" "A" "A" "C" "T" "G" "G" "T" "G" "C" "T"
## [120] "C" "C" "T" "G" "C"
```
    #Interestingly, using character(0) generates the same list as using "" as the string identifier.
####Question 4. What kind of object does this return? How might we make it a character vector again?


```r
read_1_split <- as.character(unlist(read_1_split)) #unlist list to a vector and convert vectorized list to a character vector
```
    #Creates a list object. We can use the function unlist() to convert each base into vector elements and then as.character to convert it into a vector of characters

####Question 5. How about if we wanted the number of occurrences of each base? Or better yet, their frequencies? You could write a loop, but I suggest looking at the help for the `table()` function... Also keep in mind that for for most objects `length()` tells you how many elements there are in a vector. For lists use `lengths()` (so you can either do this on a character vector or a list, your choice)

```r
table(read_1_split) #count base occurences in vector of combined reads.
```

```
## read_1_split
##  A  C  G  T 
## 23 35 40 26
```

```r
length(read_1_split) #length of character vector
```

```
## [1] 124
```

```r
read_1_freq <- table(read_1_split)/length(read_1_split) #Calculate base frequencies
print(read_1_freq)
```

```
## read_1_split
##         A         C         G         T 
## 0.1854839 0.2822581 0.3225806 0.2096774
```

####Question 6. How would you make this into a nice looking function that can work on either a list or vectors of characters? (Still just for a single read)

```r
BaseFrequencies <- function(x) {
    if (mode(x) == "list") {
    	tab <- table(x)/lengths(x)}
    
    else {
    	tab <- table(x)/length(x)
    }	
   return(tab)
}
```


####Question 7. Now how can you modify your approach to do it for an arbitrary numbers of reads? You could use a loop or use one of the apply like functions (which one)?

```r
basefreq <- lapply(read_1_split, BaseFrequencies)
print(basefreq, digits = 2)
```

```
## [[1]]
## x
## C 
## 1 
## 
## [[2]]
## x
## G 
## 1 
## 
## [[3]]
## x
## C 
## 1 
## 
## [[4]]
## x
## G 
## 1 
## 
## [[5]]
## x
## C 
## 1 
## 
## [[6]]
## x
## A 
## 1 
## 
## [[7]]
## x
## G 
## 1 
## 
## [[8]]
## x
## T 
## 1 
## 
## [[9]]
## x
## A 
## 1 
## 
## [[10]]
## x
## G 
## 1 
## 
## [[11]]
## x
## G 
## 1 
## 
## [[12]]
## x
## G 
## 1 
## 
## [[13]]
## x
## C 
## 1 
## 
## [[14]]
## x
## A 
## 1 
## 
## [[15]]
## x
## C 
## 1 
## 
## [[16]]
## x
## A 
## 1 
## 
## [[17]]
## x
## T 
## 1 
## 
## [[18]]
## x
## G 
## 1 
## 
## [[19]]
## x
## C 
## 1 
## 
## [[20]]
## x
## C 
## 1 
## 
## [[21]]
## x
## A 
## 1 
## 
## [[22]]
## x
## G 
## 1 
## 
## [[23]]
## x
## G 
## 1 
## 
## [[24]]
## x
## T 
## 1 
## 
## [[25]]
## x
## G 
## 1 
## 
## [[26]]
## x
## T 
## 1 
## 
## [[27]]
## x
## C 
## 1 
## 
## [[28]]
## x
## C 
## 1 
## 
## [[29]]
## x
## G 
## 1 
## 
## [[30]]
## x
## C 
## 1 
## 
## [[31]]
## x
## C 
## 1 
## 
## [[32]]
## x
## A 
## 1 
## 
## [[33]]
## x
## C 
## 1 
## 
## [[34]]
## x
## T 
## 1 
## 
## [[35]]
## x
## T 
## 1 
## 
## [[36]]
## x
## G 
## 1 
## 
## [[37]]
## x
## G 
## 1 
## 
## [[38]]
## x
## T 
## 1 
## 
## [[39]]
## x
## G 
## 1 
## 
## [[40]]
## x
## G 
## 1 
## 
## [[41]]
## x
## G 
## 1 
## 
## [[42]]
## x
## C 
## 1 
## 
## [[43]]
## x
## A 
## 1 
## 
## [[44]]
## x
## C 
## 1 
## 
## [[45]]
## x
## A 
## 1 
## 
## [[46]]
## x
## C 
## 1 
## 
## [[47]]
## x
## A 
## 1 
## 
## [[48]]
## x
## G 
## 1 
## 
## [[49]]
## x
## C 
## 1 
## 
## [[50]]
## x
## C 
## 1 
## 
## [[51]]
## x
## G 
## 1 
## 
## [[52]]
## x
## A 
## 1 
## 
## [[53]]
## x
## T 
## 1 
## 
## [[54]]
## x
## G 
## 1 
## 
## [[55]]
## x
## A 
## 1 
## 
## [[56]]
## x
## C 
## 1 
## 
## [[57]]
## x
## G 
## 1 
## 
## [[58]]
## x
## A 
## 1 
## 
## [[59]]
## x
## A 
## 1 
## 
## [[60]]
## x
## C 
## 1 
## 
## [[61]]
## x
## G 
## 1 
## 
## [[62]]
## x
## G 
## 1 
## 
## [[63]]
## x
## G 
## 1 
## 
## [[64]]
## x
## C 
## 1 
## 
## [[65]]
## x
## T 
## 1 
## 
## [[66]]
## x
## C 
## 1 
## 
## [[67]]
## x
## C 
## 1 
## 
## [[68]]
## x
## T 
## 1 
## 
## [[69]]
## x
## T 
## 1 
## 
## [[70]]
## x
## G 
## 1 
## 
## [[71]]
## x
## A 
## 1 
## 
## [[72]]
## x
## C 
## 1 
## 
## [[73]]
## x
## T 
## 1 
## 
## [[74]]
## x
## A 
## 1 
## 
## [[75]]
## x
## T 
## 1 
## 
## [[76]]
## x
## A 
## 1 
## 
## [[77]]
## x
## A 
## 1 
## 
## [[78]]
## x
## T 
## 1 
## 
## [[79]]
## x
## C 
## 1 
## 
## [[80]]
## x
## T 
## 1 
## 
## [[81]]
## x
## G 
## 1 
## 
## [[82]]
## x
## A 
## 1 
## 
## [[83]]
## x
## C 
## 1 
## 
## [[84]]
## x
## C 
## 1 
## 
## [[85]]
## x
## C 
## 1 
## 
## [[86]]
## x
## G 
## 1 
## 
## [[87]]
## x
## T 
## 1 
## 
## [[88]]
## x
## T 
## 1 
## 
## [[89]]
## x
## T 
## 1 
## 
## [[90]]
## x
## G 
## 1 
## 
## [[91]]
## x
## C 
## 1 
## 
## [[92]]
## x
## G 
## 1 
## 
## [[93]]
## x
## T 
## 1 
## 
## [[94]]
## x
## T 
## 1 
## 
## [[95]]
## x
## T 
## 1 
## 
## [[96]]
## x
## G 
## 1 
## 
## [[97]]
## x
## G 
## 1 
## 
## [[98]]
## x
## G 
## 1 
## 
## [[99]]
## x
## T 
## 1 
## 
## [[100]]
## x
## G 
## 1 
## 
## [[101]]
## x
## A 
## 1 
## 
## [[102]]
## x
## C 
## 1 
## 
## [[103]]
## x
## C 
## 1 
## 
## [[104]]
## x
## A 
## 1 
## 
## [[105]]
## x
## G 
## 1 
## 
## [[106]]
## x
## G 
## 1 
## 
## [[107]]
## x
## G 
## 1 
## 
## [[108]]
## x
## A 
## 1 
## 
## [[109]]
## x
## G 
## 1 
## 
## [[110]]
## x
## A 
## 1 
## 
## [[111]]
## x
## A 
## 1 
## 
## [[112]]
## x
## C 
## 1 
## 
## [[113]]
## x
## T 
## 1 
## 
## [[114]]
## x
## G 
## 1 
## 
## [[115]]
## x
## G 
## 1 
## 
## [[116]]
## x
## T 
## 1 
## 
## [[117]]
## x
## G 
## 1 
## 
## [[118]]
## x
## C 
## 1 
## 
## [[119]]
## x
## T 
## 1 
## 
## [[120]]
## x
## C 
## 1 
## 
## [[121]]
## x
## C 
## 1 
## 
## [[122]]
## x
## T 
## 1 
## 
## [[123]]
## x
## G 
## 1 
## 
## [[124]]
## x
## C 
## 1
```
####Question 8. Can you revise your function so that it can handle the input of either a string as a single multicharacter vector, **or** a vector of individual characters **or** a list? Try it out with the vector of three sequence reads (`reads`).  

```r
BaseFrequencies <- function(x) {
    
    # if it is a single string still
    if (length(x) == 1 & mode(x) == "character") {
    	x <- strsplit(x, split = "", fixed = T) 
        x <- as.character(unlist(x))
    }     
    
    if (mode(x) == "list") {
    	tab <- table(x)/lengths(x)}
    
    else {
    	tab <- table(x)/length(x)
    }	
   return(tab)
}
basefreq <- sapply(reads, BaseFrequencies, USE.NAMES = F)
print(basefreq, digits = 2)
```

```
##   [,1]  [,2]  [,3]
## A 0.19 0.274 0.081
## C 0.28 0.331 0.331
## G 0.32 0.298 0.347
## T 0.21 0.097 0.242
```


