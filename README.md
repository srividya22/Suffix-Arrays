# Suffix Arrays
I am sharing my experince with various Suffix Array Construction Algorithms(SACA).

# Introduction
Few years back I stumbled upon this nice data structure Suffix Tree. I must admit its construction is a bit complex.
While studying more about it, I read about Suffix Array.
There are so many algorithm being developed considering vast application of Suffix Array in patter matching, compression, Genome Sequencing.These Algorithm can be categorised into broadly three category.

* Prefix Doubling  - Manaber -meyers/Larsson-Sadakane

Key idea is if you take cyclic rotation at each position of a given string and sort it and remove everything after $ from each string you will have a sorted suffix.
Lets see this with an example 
String = ababaa
Append $ at the end this will help in indetifying each uique suffix, for example try to make suffix tree for papa
it will be 
a
pa
apa
papa

a & apa share common prefix a , so suffix 'a' is invisible, to fix this we add $
now suffixs are
$
a$
pa$
apa$
papa$

So our input string is ababaa$
String starting at index.


index | string|index | string|index | string
------|-------|------|-------|------|-------
0| ababaa$       |6| $ababaa          |6| $
1| babaa$a       |5| a$ababa          |5| a$
2| abaa$ab       |4| aa$abab          |4| aa$
3| baa$aba       |2| abaa$ab          |2| abaa$ 
4| aa$abab       |0| ababaa$          |0| ababaa$
5| a$ababa       |3| baa$aba          |3| baa$
6| $ababaa       |1| babaa$a          |1| babaa$

So final suffix array would be [6, 5, 4, 2, 0, 3, 1]
Now how to convert this into an efficient algorithm.
We will first sort 1 characters and then at each iterator double up, so total iterations would be log(n) , in each iterator we will do 'n' cycle to calculate 'order' and 'rank'. SO overall complexity would be O(n log(n))

order: index of suffix in lexigoraphic order in this scan cycle. 
rank:  different type of string encoruted in this scan cycle.

Lets start with 1
ababaa$
Counting sort       Partial Sum 
$ -> 1               $ -> 1
a -> 4               a -> 5    
b -> 2               b -> 7
Scan from backward 
Each count[literal]-- 
order [count[literal]-- ] = pos

ababaa$

i  |str |Count|Order
---|----|-----|-----
6  |  $ |  0  |6, , , , , ,
5  |  a |  4  |6, , , ,5, ,
4  |  a |  3  |6, , ,4,5, , ,
3  |  b |  6  |6, , ,4,5, ,3
2  |  a |  2  |6, ,2,4,5, ,3
1  |  b |  5  |6, ,2,4,5,1,3
0  |  a |  1  |6,0,2,4,5,1,3 

We scan the inpur string from backward and we know based on couting sort that their is 1 $, 4 a and 2 b , so a is going to end  (count of $) + count of a i.e. at 5 , similarly b will end at 7
One more thing is stable sort i.e. if we encounter 2 a's order will be decided based on order of occurence in string.
So we have 4 a's at index 0,2,4,5 and in suffix array they come in same order.
So this is how we have calculated order when taken 1 at a time.

rank array : each distinguised elemnt occur in order 
rank = [0,1,1,1,1,2,2] because there are only 3 different elements i.e. $, a, b



* Recursive Algorithm - DC3, KA Algroithm, NZC Algorithm
* Induced Sorting - S Algorith/MF Algorithm

Complete taxonomy of these algorithm can be found [here](http://www.cas.mcmaster.ca/~bill/best/algorithms/07Taxonomy.pdf)

# Prefix Doubling Algorithm
# DC3 Algorithm
# S[Seward] Algorithm
# KA Algorithm / NZC Algorithm
# Burrows Wheelers Transform
   * BWT
      
      BWT are helpful in converting and inpur string containing **repeated subsequence into runs** and then run length encoding can be         used to allow compression.
      For a given input string, sort all cyclic rotation in lexigoraphical order and the last column give you BWT.
      For example
      ababaa$
      Refer the table above , BWT(ababaa$)   = aabb$aa
      Actually they are one previous character in suffix array.
      Trick is in English language any common word like **and** when suffix sorted its previous characters(in last column) would be           **a** and that would contribute to run.
   * Inverting
       Lets see if we have BWT can we recover original string.
       
       ##### Naive Way.
       
       As we know BWT is last column in sorted cyclic rotation.We also know that first column is sorted.
       
       $-----a1
       a1-----a2
       a2-----b1      
       a3-----b2
       a4-----$
       b1-----a3
       b2-----a4 
       combine them because we know what preceeds each sorted suffix and then sort
       
       a$       $a---a         a$a        $ab--a       a$ab       $aba--a       a$aba       $abab-a
       aa       a$---a         aa$        a$a--a       aa$a       a$ab--a       aa$ab       a$aba-a
       ba       aa---b         baa        aa$--b       baa$       aa$a--b       baa$a       aa$ab-b
       ba Sort->ab---b Join -> bab Sort-->aba--b Join->baba Sort->abaa--b Join->babaa Sort->abaa$-b  
       $a       ab---$         $ab        aba--$       $aba       abab--$       $abab       ababa-$ 
       ab       ba---a         aba        baa--a       abaa       baa$--a       abaa$       baa$a-a
       ab       ba---a         aba        bab--a       abab       baba--a       ababa       babaa-a
       
       Keep doing this and eventually we get the input string.
       But the problem is it take lot more memory.
       ##### First last property.
       Occurence of any symbol in first and last column matches.
       Trick is we know that BWT contains symbol one previous that of suffix array (which are in lexigraphical order).
       So start with $ (because we know for sure that this is last symbol in original string).
       Now BWT corresponding to this $ tells what lies before $ so that symbol preceed $ in original string, take that lookup i
       first column and find again the correspoding BWT symbol.
       Repeating this process we spell out original string.
       Take example above
       $ ->a1 -> a2-> b1->a3->b2-> a4->$
       So original string is ababaa$
       
   * Pattern Matching using LF mapping
   * FM-Index
   * Wavelet Tree
   * RRR Data structure.

# Practical Application
# Competitive Programming Problems
# Reference
# Suffix Tree using Suffix Array
# Suffix Tree using Ukkonen Algorithm
