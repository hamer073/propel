# M-LRMUS - The Problem
The problem comes from the Fall 2018 Algorithms Sorting Competition. The goal was to generate the Maximimal Left to Right Minimal Unique Substring or the left most longest repeated substring plus one character. For more information you can read more about the details of the problem [here](https://github.com/pluck011/SortingCompetitionF18).

# The Set Up
We first used the data generator provided in the linked repo to generate test input data. We also hand wrote some smaller test cases as the data generator gives very long examples. From there we ran the provided M-LRMUS generator to get the expected outputs for our runs to compare against. Once we had that we began deciding on the instruction set.
We took all of the provided string functions since this problem is almost entirely a string problem. We also provided a limited set of integer functions for index manipulation, exec_dup and exec_if, and boolean functions. For constants we gave it integers 0-9, true, and false.  
We added string iterator and substring function, both were adapted from the version in Clojush. From this we had to add a character stack as well, this was only used by the string iterator function. We also had to implement a string_index function which was used to find the position of substrings. With all these functions, we had all that was required to create a function which could find M-LRMUS.
We later on added a longest repeated substring function, which was implmented in Java and then called in Clojure, though there were issues with this function which ended up causing us to not find the correct solution.

# The Results
We had multiple different types of runs which we ran.   
The initial runs did not include a string iterator or string index. We used lexicase for our selection algorithm and used levenshtein distance for the fitness function. These tests were by far the worst, even after 2000 generations we saw higher error than other runs at around 400 generations. We determined it was not possible to find M-LRMUS without string iterator at this point.  
The next set of runs included string iterator using the same selection and fitness functions. These runs faired far better, this is where we hit the same error as the last set of runs in about half the generations. We were missing string index though so it got stuck at 247 error.  
The next set of runs used our own fitness function. This ended up being far worse. It ended up only printing off the original string and never evolved the function even with lexicase selection. Needless to say we went back to levenshtein after around 2 runs.
The next set of runs included string index. This didn't change much from the string iterator generation. The lowest error value we got was 245 and never got lower. We ran this for quite a few runs but we could never get it lower than 245.
In a last ditch effort, our final two set of runs used string LRS. The first of these runs did not include string index. The ones without string index ended up getting stuck at around 90 after a few hundred generations. String index got the error down to 64 after around 400 generations. Now we think that the real error with index was actually 0, we realized part way through that LRS was returning the right most longest repeated substring, so words with multiple unique substrings would end up getting the wrong LRS for M-LRMUS. So if the LRS function returned the left most rather than the right most longest repeated substring, this would have most likely succeeded.

# Tweaks and Changes
These are recorded for the most part in [The Results](#the-results) though this will give a more indepth step by step for what we did (from what we remember):
1. Run with only propel functions. Disabled most integer functions.
2. Add the entire alphabet.
3. Add string_substring.
4. Fix string_substring
5. Add string_iterator.
6. Remove integers 0-9, only keep 1.
7. Add back integers 0-9.
8. Removes integers 0-9, only keep 0 and 1.
9. Add back integers 0-9.
10. Change to custom fitness functions.
11. Remove custom fitness function (it was bad).
12. Remove the entire alphabet.
13. Try Tournament Selection.
14. Switch back to Lexicase.
15. Add string_index
16. Remove string_index
17. Add string_index (fixed this time)
18. Add string_lrs (without string_index)
19. Add string_index to string_lrs branch  

There were more steps than this that we most likely missed, there were a lot of really tiny changes we made only to end up removing. You can also assume we ran ~1000 generations for each of these sets with ~1000 for the population.  
You can see the actual work in the fizz-buzz and longest-repeated-substring branches.