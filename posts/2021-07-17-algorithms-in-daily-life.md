---
layout: post
title: "Algorithms in Daily Life"
tags: programming algorithms
medium_link: algorithms-in-daily-life-5bfbf04ee368
---

*This post was originally published as an article for the [fourth issue](https://issuu.com/harrowhongkong/docs/scientific_harrovian_2019_single_page) of Harrow International School Hong Kong's Scientific Harrovian, in 2019. The article was intended to be a relatively non-technical introduction to basic algorithms and algorithm analysis for Sixth Form students. I've since made a few tiny edits and tidied it up somewhat.*

---

What comes to mind when you hear the word ‘algorithm’? Some sort of perplexing concept that has something to do with technology or coding? In a sense, you'd be correct. Algorithms are what make computers work; without them, computers would be useless, unable to complete any tasks. They are the instructions that allow computer systems to solve a variety of complicated problems, faster than any human could. And they are everywhere in modern life, from Netflix suggestions to financial markets.

However, in its simplest terms, an algorithm is just a collection of simple instructions for carrying out some task in finite time. An algorithm takes an input, does something to it, and provides an output. We take advantage of the computer’s ability to perform billions of operations per second to run algorithms at lightning speed, but algorithms don’t need to have anything to do with computer code. In fact, the concept of algorithms has existed for millennia; some of the first algorithms were used by the Ancient Babylonians around 1600 BCE, for purposes such as [finding square roots](https://doi.org/10.1006/hmat.1998.2209) and [calculating compound interest](https://doi.org/10.1145/361454.361514).

In fact, you use algorithms yourself every day, whether you know it or not. If you think about our earlier definition of an algorithm, you could also call it a ‘procedure’ or even a ‘recipe’. Even the completely ordinary task of cooking something from a recipe is you following an algorithm. You take your ingredients (input), follow the instructions, and produce a delicious meal (output)! A cooking recipe is nowhere near as complicated as the complex algorithms that power Google or Facebook, but it is an algorithm nonetheless.

This article aims to acquaint you with algorithmic thinking by introducing a range of simple algorithms in the context of everyday problems. The algorithms that will be introduced are simple – some may even be so intuitive that you already use them without thinking about it – but they still hold great importance in the computer systems that allow us to do things you’d never think twice about, such as accessing the Internet. Furthermore, different algorithms used to approach the same task will be compared, to see how efficient they are relative to each other. After all, speed is a very important factor to consider when implementing an algorithm – you wouldn’t want to use an algorithm that takes an hour to complete if there exists an alternative that only takes a minute!

<!-- excerpt-end -->

## Time complexity
Time complexity describes the amount of time it takes to run an algorithm. However, we don’t measure this in seconds or minutes, because that’s not what we're interested in. An algorithm’s running time will be different depending on the size of its input. For example, it would obviously take less time to find an item out of 100 other items than, say, a million other items! What we are interested in is how different algorithm running times grow at different rates. Instead of using seconds, time complexity is measured using Big O notation, which shows how quickly the runtime of an algorithm increases as the size of the input increases.

![Common Big O runtimes](/images/big-o-runtimes.png)

Big O notation is written with a ‘big O’ and some function of $$ n $$ within a pair of brackets, for example $$ O(n^2) $$.

## Searching
Imagine that you are playing a simple game. An integer from 1 to 16 is randomly chosen and you need to guess which number it is. After every guess, you are told if your guess was too high or too low. How would you approach this?

A very simple approach would be to guess 1, then 2, then 3, and so on. This method, known as *linear search*, is very easy to use, as you simply guess all the numbers in sequence. However, it should be obvious that it is not the best approach, since the worst-case scenario would be that you take 16 guesses, if the number was 16.

![Worst-case scenario of using linear search in the guessing game](/images/linear-search.png)

An alternative approach that you most likely would’ve taken is to start off by guessing at the midpoint, 8. If 8 is too high, you can eliminate all numbers from 8 to 16; if 8 is too low, you can eliminate all numbers from 1 to 8. With one guess, you will be able to eliminate half of the numbers. Each guess further eliminates half of the remaining numbers, until you achieve the correct number. Because of this, to find the worst-case scenario, we just need to find how many times you have to divide 16 by 2 in order to get to 1 – if there’s one number left it has to be the correct one. Therefore, the worst-case scenario would only require $$ \log_{2⁡} 16 = 4 $$ guesses.  This method, called *binary search*, is a much more efficient method to solve this problem as every guess halves the range of further guesses.

![Worst-case scenario of using binary search in the guessing game](/images/linear-search.png)

Now imagine applying binary search to a much larger list of items. Imagine trying to find one user out of the 2.3 billion users on Facebook (as of December 2018). Using linear search, your worst-case scenario would be having to check every single 2.3 billion users! However, if you used binary search, the first user you check would already eliminate the need to check over 1 billion other users. Therefore, you’d only need a maximum of $$ \lceil \log_{2} (2.3 \times 10^9) \rceil = 32 $$ guesses. This shows how binary search becomes much more efficient as the number of items in the list increases.

When expressed in Big O notation, the time complexity of linear search is $$ O(n) $$, also known as *linear time*. This means that the running time increases linearly with the size of the input, i.e. the runtime is directly proportional to the input size. You could be lucky and find the correct item in your first try, however statistically this is not likely to happen and this is why we usually use Big O to either express the worst-case or average-case runtime.

We have already established that at most, binary search requires searching through $$ \log_{2} ⁡n $$  items for a list of $$ n $$ items. Therefore binary search has a time complexity of $$ O(\log n) $$, known as *logarithmic time*. An $$ O(\log n) $$ algorithm is very efficient, since the properties of logarithms means that as the number of inputs increases, the rate at which the running time increases slows down very quickly.

![Big O runtimes of linear and binary search](/images/big-o-linear-vs-binary.png)

## Sorting
You may have noticed that there is one requirement of binary search – the list must be sorted! You cannot run binary search on an unsorted list, as it relies on knowing if your value is higher or lower than the target value. You would have to use linear search if you had an unsorted list – this means that at you are limited to $$ O(n) $$ time. Therefore, if you often need to search through a list, it may be useful to use a sorting algorithm to sort it, so that you can use the much more efficient binary search on it.

Imagine having to sort a hand of playing cards. Most people would probably use the following approach:
1. Take the second card
2. If it is smaller than the first card, move it to the beginning
3. Take the third card and compare it with the first and second card, moving it to the correct location if needed
4. Repeat for the rest of the cards

This method is called *insertion sort*. The following figure shows a visualisation of insertion sort being applied on an unsorted list[^1]:

![Example execution of insertion sort](/images/insertion-sort.png)

An alternative sorting algorithm is called *merge sort*. It is what is called a *divide and conquer algorithm*, as it breaks down the problem into multiple subproblems to solve. The idea of merge sort is as follows:
1. Split the list in half, resulting in 2 sublists
2. Repeat halving the sublists until you have n sublists, each containing only one item (after all, a list with one item is considered sorted!)
3. For each pair of sublists (which at this point are only size 1), put the smaller item to the left and larger item to the right, producing sublists of size 2
4. Repeat this with each pair of sublists (which are now size 2)
5. Continue ‘merging’ the sublists to produce new sorted sublists until you are left with only one list, which will be sorted

The following figure shows a visualisation of merge sort being applied on an unsorted list[^2]:

![Example execution of merge sort](/images/merge-sort.svg)

Merge sort may seem unnecessarily complicated, and it is true that it would probably be impractical for humans to use. It would be much more intuitive for us to use insertion sort instead. However, this is different for a computer. Insertion sort has a time complexity of $$ O(n^2) $$, also known as *quadratic time*, while merge sort is $$ O(n \log n) $$. This means that merge sort is more efficient than insertion sort, especially in large lists. While easy to program, insertion sort is usually only used in real-life applications when the expected size of lists is small. Merge sort handles larger lists much more easily but it is much more advanced and complex to implement.

![Big O runtimes of insertion and merge sort](/images/big-o-insertion-vs-merge.png)

## Maze solving
Imagine that you are trapped inside a maze. How would you find your way out? The most trivial (and inefficient) method would be to simply follow along the passages, making random turns until you find your way out. Although you would eventually escape, you might imagine that this approach can be extremely slow. Surprisingly, this unintelligent algorithm does actually have a name – the *random mouse algorithm*.

However, surely there is a better method. One that is commonly known is the *wall follower algorithm*, also known as the left-hand or right-hand rule. You simply follow the wall with one hand, and you would eventually find your way out of the maze. This approach works because if you rearrange a maze’s walls, you usually can end up with a straight line. And so, if you think of the maze as a piece of string, it is obvious that walking from one end will eventually get you to the other.

![A simple maze rearranged into a straight line](/images/maze-rearrangement.png)

However, a flaw of the wall follower algorithm is that it doesn’t work on all mazes. If the maze has ‘loops’ inside of it – walls that are not connected to the outer wall, this algorithm fails. The following figure[^3] shows how you would be stuck running around in circles if you started following a wall in a simple loop:

![Examples of the wall follower algorithm](/images/wall-follower.png)

If a loop was much larger and more complicated, it would be hard to realise that you are following one and you would never leave the maze. Luckily, there is another algorithm that doesn’t have this weakness – *Trémaux’s algorithm*. A simple version of the algorithm was actually described in the Greek myth of Theseus and the Minotaur. In the tale, the architect Daedalus built a labyrinth to contain the Minotaur, a ferocious monster. Theseus, who was to be fed to the Minotaur, was given a plan by Ariadne, the daughter of the King, to escape the labyrinth.

> “She sent for Daedalus and told him he must show her a way to get out of the Labyrinth, and she sent for Theseus and told him she would bring about his escape if he would promise to take her back to Athens and marry her. She gave him the clue she had got from Daedalus, a ball of thread which he was to fasten at one end to the inside of the door and unwind as he went on. This he did and, certain that he could retrace his steps whenever he chose, he walked boldly into the maze looking for the Minotaur.”

In more algorithmic terms, Trémaux’s algorithm can be described as follows:
1. Draw a line to record the path you take (Theseus used the thread to mark this)
2. At a junction, if an unmarked path exists, take it
3. Or else, take the path marked once
4. Never take a path marked twice
5. At a dead end, turn around

This method makes use of *backtracking*, so whenever you hit a dead end you can try a different path. The ability to go back to intersections and try better routes guarantees that you will find your way out, no matter if the maze has loops or not. In the following figure, the green arrows show a decision to take a new path when you meet a marked path (step 2 of our algorithm):

![Example of a maze solved using Trémaux’s algorithm](/images/tremaux.png)

These three maze-solving algorithms covered are useful for a person inside of a maze. There are other methods that may be faster and guarantee a shortest path out, however they require us to have a full bird’s eye view of the maze. These include dead-end filling and shortest path algorithms such as breadth-first search, Dijkstra’s algorithm, and the A* search algorithm[^4]. Those who have done the D1 module in A Level Further Mathematics may intuitively recognise that the [paths in any maze (with no loops) can be pulled and stretched out to resemble a tree (from graph theory)](https://youtu.be/k1tSK5V1pds).

Maze-solving seems to have little application to daily life, however it turns out that the idea of getting from one point to another in a constrained environment (like a maze) is very important in many applications that we use every day. In graph theory, an alternative way of describing a maze would be to have the passages as edges and intersections as nodes. A website that knows how roads are connected – such as Google Maps – can tell you the best way to get from your home to school, and an app that knows your friends – Facebook or Snapchat, for example – can better guess who else you may know, based on the connections between your mutual friends. Even robot vacuums are a good example, as different brands and models use different algorithms. Cheaper robots may use something similar to the random mouse algorithm, simply roaming around, while more expensive models may first map out your room to determine where obstacles are and then move back and forth in a grid-like pattern. The maze-solving algorithms that are used in everyday life may not be as simple as the ones described here, but it is important to understand how something so seemingly trivial can be so important in real-world applications.

## Conclusion

Algorithms are important in the computer applications that are everywhere in our lives; however, the concept has existed for millennia before computers were even invented. I hope that after reading this article, you have gained a deeper knowledge of how algorithms work. It is important to understand that each algorithm has its advantages and disadvantages, depending on the situation it is used in. Although it may seem obvious that you should use merge sort over insertion sort due to its higher efficiency, that is not always the case. Sometimes, it is better to use less efficient algorithms since more efficient ones are usually more complex and prone to bugs and errors. It is worth it to sacrifice some speed over an algorithm that may run faster but may also go wrong. In some cases, linear search is used instead of binary search because the lists or databases that require to be searched are not sorted; if items are constantly added or removed from the database, the time required to sort the items again and again for the sake of binary search may not be worth it.

Algorithms are beautiful because of how they allow you to solve generalised problems. All you have to do is understand the instructions to tackle a type of problem, and you can solve any that comes your way. No matter what kind of list you are given, you can always sort it with merge sort. No matter what kind of maze you are given, you can always find a way out with Trémaux’s algorithm. These mathematical recipes allow computer systems to do amazing things, enabling Google to find relevant search results, Amazon to speedily deliver packages, and YouTube to recommend videos. As technology continues to dominate our daily lives, algorithms will only become increasingly important in the everyday functioning of society.


[^1]: Image by [GeeksforGeeks](https://www.geeksforgeeks.org/insertion-sort/).
[^2]: Image via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Merge_sort_algorithm_diagram.svg).
[^3]: These illustrations are from the book 'Bad Choices: How Algorithms Can Help You Think Smarter and Live Happier', by Ali Almossawi. An online adaptation of the first seven chapters can be found [here](https://bookofbadchoices.com/) – it's an excellent short read for a further introduction to computational thinking.
[^4]: [Wikipedia](https://en.wikipedia.org/wiki/Maze-solving_algorithm) has a relatively useful page about these algorithms.