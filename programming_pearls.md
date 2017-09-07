# Programming Pearls

## Preface
The Mythical Man Month, Fred Books, crucial role of management in large software projects.
Code Complete, Steve MCConnell, good programming style.
> Just as natural pearls grow from grains of sand tha thav irritated oysters, these programming perals have grown  from real problems that have irritated real programmers.

ACM: communications of the association for computing machinery.


## PartI: preliminaries

### Column1: Cracking the Oyster

####Understand your context

A real database of toll-free telephone numbers:
- toll-free telephone number(800 followed by 7 digits) with no duplication
- real number towhich calls are routed
- the name and address of the subscriber

Output and preformance requirements.

####Precise problem statement

- input:
- output:
- constraints:
    
Quicksort for in-memory sorting in each pass. Read integers between Na and Na+offset, and sort them and write them to the output file. Then move Na to next set.

####Program Design

Input Once and output once. 
The problem boils down to whether we can represent at most ten million distinct integers in about eight million available bits.

####Implementation sketch

Bitmap. Three attributes of this problem not usually found in sorting problems: the input is from a relatively small range, it contains on duplicates, and no data is associated with each record beyong the single integer.
        
    /* phase 1: initialize set to empty */
    for i = [0, n)
        bit[i] = 0
    /* phase 2: intsert present elments into the set */
    for each in in the input file
        bit[i] = 1
    for i = [0, n)
        if bit[i] == 1
            write i on the output file

####Principles
Careful analysis of a small problem can sometime yield tremendous practical benefits.
> Simple, few parts, easy to maintain, very strong.

- The right problem
- The Bitmap data structure. This data structure represents a dense set over a finite domain whe each element occurs at msot once and no other data is associated with the element. Even when there are multiple elements or extra data, a key from a finite domain can be used as an index into a table with more ocmplicated entries.
- Multiple-pass algorithms. Achieve a little more each pass.
- A Time-Space tradeoff and one that isn't. The space-efficient structure of bitmaps dramatically reduced it srun time of sorting. Two reasons: less data to process means less time to process it, and keeping data in main memory rather than on disk avoids the overhaed of disk accesses.
- Tradeoffs are common to all engineering disciplines.
- A simple Design.
- Stages of Program Design.

####Problems
1. If memory were not scarce, how would you implement a sort in a language with libraries for representing and sorting sets?
Qsort.
2. How would you implement bit vectors using bitwise logical operations?
    void set(int i) { a[i>>5] |= (1<<(i&Ox1F))}; // a[i/32] 1= i%32; each array element is an integer, and i%32 == i&0x1F;
    void clr(int i) { a[i>>5]&=~(1<<i&0x1F)};
    void test(int i) {a[i>>5]&(1<<(i&0x1F))};
3. run-time efficiency was an important part of the design goal, and the resulting program was efficient enough. Implement the bitmap sort on your system and measure its run time; how does it compare to the system sort and to the sorts in Problme 1?
C/bitmaps is faster than C/qsort, and is 50 times faster or use less memory than C++, and even faster than system sort.
4. Generate a random K integer without duplicates
    for i = [0,n)
        x[i] = i
    for i = [0, k)
        swap(i, randint(i, n-1))
        yield x[i]
5. A k-pass algorithm sorts at most n nonrepeated positive integers less than n in time kn and space n/k.
6. What would you recommend to the programmer if, instead of each integer could appear at most once, he told you that each integer could appear at most ten times? How would your solution change as a function of the amount available storage?
Then use 4 bits or half byte(2^4=16) to represent each integer that could repeat 10 times. Memory usage is 10,000,000/2 bytes for 1 time pass algorithms. As for k time pass, time complexity is kn, and space complexity is n/2k.
7. Key indexing
10. Open hashing with collision resolution by sequential search. The last two digits of the phone number are quite close to trandome and therefore an excellent hash function.


#### Future reading
Conceptual block: mental walls that block the problem-solver from correctly preceiving a problem or conceiving its solution. More creative thinking.

A data index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure. An index is a copy of selected columns of data from a table that can be searched very efficiently that also includes a low-level disk block address or direct link to the complete row of data it was copied from.


## Aha! Algorithms
> A problem that seems difficult may have a simple, unexpected solution.

### Three problems
A. Given a sequential file that contains at most four billion 32-bit integers in random order, find a 32-bit integer that isn't in the file( and there must be at least one missing --Why?). How would you solve this problem with ample quantities of main memory? How would you solve it if you could use several external "scratch" files but only a few hundred bytes of main memory?

B. Rotate a one-dimensional vector of n elements left by i positions. For instance, with n=8 and i=3, the vector abcdefgh is rotated to defghabc. Simple code uses an n-element intermediate vector to do the job in n steps. Can you rotate the vector in time proprotional to n using only a few dozen extra bytes of storage?

C. Given a dictionary of English words, find all sets of anagrams. For instance, "pots", "stop" and "tops" are all anagrams of one another because each can be formed by permuting the letters of the others.

### Ubiquitious binary search
Find a missing number in 4 billion integers:
If plenty of memory, bitmap
If not, binary search, define a range of integer, count the number upper and lower than the integer, and search further in the smaller scope.
bisection to solve a single variable equation.
randomized binary search.
Tree data strucutres and program debugging.

### The powers of primitives
Rotation correspond to swapping adjacent blocks of memory of **unequal** size.
1. Space expensive: copy the first i elements of x to a temporary array, moving the remainning n-i elements left i places, and then copying the first i from the temporary array back to the last positions in x.
2. Time expensive: define a function to rotate x left one position and call it i times.
3. a delicate juggle act: move x[0] to the temporary t, then move x[i] x[0], x[2i] to x[i], and so on, until we come back to taking an element from x[0], at which point we instead take the element from t and stop the process. If taht process didn't move all the elements, then we start over at x[1], and continue until we move all the elements.
4. swapping: suppose a is shorter than b. Assuming len(br)==len(a), ab-->ablbr-->brbla-->swap brbl recursively
5. reverse: ab--> reverse(a)reverse(b)-->reverse(reverse(a)reverse(b))-->ba. A ha!!! Time- and Space- efficient.
    reverse(0,i-1); reverse(i,n-1); reverse(0,n-1)
Show it in a hand-waving example

### Getting it together: Sorting
Because of n!, any method that consider all permutations of letters for a word is doomed to failure.
Any method that compares all pairs of words is doomed to failure.
Aha: sign each each word in the dictionary so that words in the same anagram class ahve the same signature, and then bring together words with equal signatures. This reduces the problem to two subproblems: selecting a signature and collecting words with the same signature. Use a signature based on sorting: order the letters within the word alphabetically to solve the first. Sort the words in the order of their signatures to solve the second. SSort this way(with a horizontal wave of the hand), then that way(a vertical way).

### Principles
Sorting: tor produce sorted output, either as part of the system specifications or as preparation for another program.
Binary search: looing up an element in a sorted table is remarkably efficient. Only drawback is that the entire table must be known and sorted in advance.
Signatures: each item in a class has the same signature and on other has.
Problem definition. Determing what the user realy wants to do is an essential part of programming.
A Problem solver's perspective. Sit back and wait for an insight rather than rushing forward with their first idea. Know the proper time.

### Problems
1. Consider the problem of find all th aanagrams of a given input word. How would you solve this problem given only the world and the dictionary? What if you could spend some time and space to process the dictionary before answering any queries?
First compute its signature. If no preprocessingis allowed then we have to read the entire dictionary sequentially, compute the signature of each word, and compare the two signatures. With processing, we could perform a binary search or hash.
2. Given a sequential file containing 4,300,000,000 32-bit integers, how can you find one that appears at least twice?
Binary search finds an element that occurs at least twice by recursively searching the subinterval that contains more than half of the integers.
gcd(a,b)=gcd(b, a mod b) // the gcd of two numbers alos divides their difference.
if a < b, return gcd(b,a);
t = a mod b;
if t == 0, return b;
else return gcd(b, t); // a,b cannot be zero. Or use substraction. Or use recursion.

"juggling"  // error prone
for i = [0, gcd(rotlist, n))
t=x[i]
j=i
loop
    k=j+rotlist
    if k>=n
        k-=n
    if k == i
        break
    x[j]=x[k]
    j=k
x[j]=t
4. Several readers pointed out that while all the three rotation algorithms require time proportional to n, the juggling algorithm is apparently twice as fast as the reversal alogrithm: it stores and retrieves eac element of the array just once, while the reversal algorithm does twice. Experiment with the functions to compare their speeds on real machines; be especically sensitive to issues **surrounding the locality of memory references**.
The reversal code has a consistenct run time, while the block swap algorithms starts off as the most expensive one, but has a good caching behavior. The juggling algorithm starts as the cheapest, but has a poor caching behavior when rotating at 8.
5. transform the vector abc to cba?
reverse((reverse(a)reverse(b)reverse(c)))
6. a used-operated directory assistance
Use a signautre of a name, sort by signature. Finding a name: use binary serach in a sorted array, or use hash or database.
7. Transpose a 4000-by-4000 matrix
