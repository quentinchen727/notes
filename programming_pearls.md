# Programming Pearls

## Preface
- The Mythical Man Month, Fred Books, crucial role of management in large software projects.
- Code Complete, Steve MCConnell, good programming style.
- The Science of Programming, David Gries
- The Medical Detective, Berton Roueche
- Hints for Computer system design, Bulter lampson
- How to lie with statistics, Darrel Huff
- Introduction to Algorithms, Cormen, Leiserson and Rivest
- Dr. Dobb's Essential Books on Algorithms and Data structures.
- Art of Computer Programming, Don Knuth
- Seminumerical algorithms, volume 2 of Don Knuth's Art of Computer Programming.
- Algorithms, Bob Sedgewick
> Just as natural pearls grow from grains of sand that irritated oysters, these programming perals have grown  from real problems that have irritated real programmers.

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

Bitmap. Three attributes of this problem not usually found in sorting problems: the input is from a relatively small range, it contains no duplicates, and no data is associated with each record beyong the single integer.
        
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
Aha: sign each each word in the dictionary so that words in the same anagram class ahve the same signature, and then bring together words with equal signatures. This reduces the problem to two subproblems: selecting a signature and collecting words with the same signature. Use a signature based on sorting: order the letters within the word alphabetically to solve the first. Sort the words in the order of their signatures to solve the second. Sort this way(with a horizontal wave of the hand), then that way(a vertical way).

### Principles
Sorting: tor produce sorted output, either as part of the system specifications or as preparation for another program.
Binary search: looing up an element in a sorted table is remarkably efficient. Only drawback is that the entire table must be known and sorted in advance.
Signatures: each item in a class has the same signature and on other has.
Problem definition. Determing what the user really wants to do is an essential part of programming.
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
To tranpose the row-major matrix, prepended the column and row to each record, sort by column then row, and then remove the column and row number, kind of like radix sort.
8. Given a set of n real numbers, a real number t, and an integer k, how quickly can you dtermine whether there exists a k-element subset of the set that sums to at most t?
Select the smallest k elements in the set:
    - sort and select the first k elements: nlg(n)
    - quick select, divide the set into two subset and the pivot is the kth largest: n
9. Sequential search and binary search represent a tradeoff between search time and preprocessing time. How many binary search need be be preformed in an n-element table to buy back the preprocessing time required to sor the table.
nlg(n)/(n-lg(n)) times.

### Implementing an anagram program[sidebar]
sort each word to get its signature, then sort each word by its signature and then squash them.

## 3.Data Structure Programs
A proper view of data does indeed structure programs.

### 3.1 Survey Program
An array or multi-dimensional arrays are the way to reduce viriable declaration and improve program performance.
Conceptual blocks against using a dynamic array of counters.

### 3.2 Form-letter programming
Separating the view from the data.
Writing the generator and the schema will probably be easier than writing the obvious program. Separating the data from the control will pay off handsomely. The shema can be manipulated with a text editor.

### 3.3 An array of examples
DRY.
- Menu
- Error Message
- Data Functions
- Word Analysis

### 3.4 Structuring data
OOP

### 3.5 Powerful tools for specialized data
- Hypertext: a unique interface
- Name-value pairs: 
- Spreadsheets
- Databases
- Domain-specific language.

### 3.6 Principles
Don't write a big program when a little one will do.
Inventor's Paradox: the more general problem may be easier to solve.
Data structure desing can have postive impacts:
- recude big programs to small programs
- time and space reduction
- increased portability and maintainability

Principles:
- Rework repeated code into arrays
- Encapsulate complex structures. Define it in abstract terms, and express those operations as a class, e.g. iterator patterns.
- Use advanced tools when possible: hypertext, name-value pairs, spreadsheets. databases, domain-specific language.
- Let the data structure the program.

### 3.7 Problems
1. Tax rate problem
[base rate, lower bound, the rate over lower bound]. Used with binary search.
2. A Kth-order linear recurrence with constant coefficients defines a series:
Use two arrays or one array and one queue
3. Write a "banner" function that is given a capital letter as input and produces an array of characters that graphically depicts that letter.
An array of compound instructions to produce the required graphics.
4. Write functions for the following date problems: given two dates, compute the number of days between them; given a date, return its day of the week; given a month and year, produce a calendar for the month as an array of characters.
5. Suffix matching: reverse and match

## 4. Writing correct programs
Coding skill is just one small part of writing correct programs. The majority of the task is the subject of the three previous columsn: problem definition, alogrithm design, and data structure selection.

### 4.1  The challenge of binary search
Ninty percent of programmers failed to write a correct binary serach.
The first binary search was publish in 1946, but hte first published binary search without bugs did not appear until 1962.

### Writing the program
The key idea of binary search is that we always know that if t is anywhere in x[0...n-1], then it must be in a certain range of x.
Initalize range to 0..n-1
loop
    ***{invariant: mustbe(range)}***
    if range is empty
        break and report that t is not in the array
    compute m, the middle of the range
    use m as a probe to shrink the range
        if t is found during the shrinking process,
        break and reports its position

use two indices l and u to represent the range l.u.
Initialization consists of the assignments l = 0 and u = n - 1.
    if l > u
        p = -1; break
    m = (1 + u ) / 2
    use ma as a probe to shrink the range l..u
    if t is found during the shrinking process,
    break and note its position.
    case
        x[m]<t: action a; mustbe(m+1,u): l = m+1;
        x[m]=t: action b; p=m; break
        x[m]>t: action c; u = m - 1

The basic techniques of program verification: stating the invariant precisely and keep and eye towards maintaining the invariant as we wrote each line of code.

### 4.3 Understanding the program
- Initialization
- Preservation
- Termination
Put a lot of assertion to ensure the invariant.

### 4.4 Principles
- Asserstions: The relations among input, program variables, and output describe the "state" of a program; assertions allow a programmer to enunciate those relations precisely.
- Sequential Control Structures: Place assertions between them and analyze each step of the program's progess individually.
- Selection Control structures: 
- Iteration control: initialization, each loop, and termination
- Functions: a contract, if precondition is satisfied, postcondition is guaranteed.
    - precondition staet that must be true before it is called.
    - postcondition: what the function will guarantee on termination.

### 4.5 The roles of program verification
Deciding which assertions to include in real software is an art that comes only with practice.
Assertions are crucial during maintainance of a program.
Use the techniques frequently to stay away from bugs.

### 4.6 Problems
1. To avoid word overflow, division by zero, variables out of declared range, or array indices out of bounds.
    word overflow: m = (u-l)/2 + l;
    array indices out of bounds: add condition 0<=l<=n and -1 <=u < n to the invariant.
    Or define fictitous boundary elements.
2. Find the first element if there are duplicate elements in a sorted array.
    Add two dummy elements x[-1]=-OO and x[n] = +00
3. Find a point between two rungs
    Binary search
4. Make the binary serach faster?


## 5. A Small Matter of Programming
Wise programmers build *** scaffolding to give them easy access to the function.

### 5.1 From psudocode to C
    int binarySearch(DataType t) {
    int l, u, m;
    l = 0;
    u = n -1;
    while (l<=u) {
        m = l + (u-l)/2;
        if (x[m] < t) {
            l = m + 1;
        } else if (x[m]==t) {
            return m;
        } else {
            u = m - 1;
        }
    }
    return -1;
}

### 5.2 A test harness
Debugger and print statements

### 5.3 The art of assertion
The weak assertion merely repeats the condition in `if` statement.
assert((u<0 || x[u]<t)&&(u+1>=n||x[u+1]>t)) to make sure u is in array range
assert(size<oldsize) to make sure the range is shrinking.
assert(sorted(array)) to make sure the array is sorted.
Assertion are helpful as we test the function in its scaffolding, and as we move from component test through system test.
Some preprocessors complied away the assertionsincur no-turn-time overhead in production, while it is like a sailer who wears a life vest while drilling on the shore and takes it off at sea.
Use of assertions in industrial-stength software.

### 5.4 Automated testing
Test exaustively, covering the empty array, common size for bugs(zero, one and two), several pwoers of two,a dn many number one away from a power of tw.

### 5.5 Timing
Time and measure it in different scales.

### 5.6 The Complete Program

### 5.7 Principles
- Scaffolding
- Coding: psudocode -> implementation language
- Testing: One can test a component much more easily and thoroughly in scaffolding than in a large system.
- Debuging
- Timing. Conduct experiment ot ensure that its performance is what we expect.

### 5.8 Problems
1. When writing large programs, use long names for global variables. Use short name for building scaffolding and essential for mathematical proofs.
Clarity is often achieved through brevity.
2. Caching behavior in timing scaffolding.
3. The tab-separated output format of the scaffolding is designed to be compatible with most spreadsheets.

### 5.9 Debugging
Debugging is hard. Great debuggers, though, can make the job look simple.

# Part II: Peformance
A simple, powerful program that delights its users and does not vex its builders.
One specific aspect of delightful programs: efficiency.
Efficiency has the advantage that it can be measured, while discusstion on userinterfaces often get bogged down in personal tastes.
- intrisic importance
- educational
- Need for speed.

## 6. perspective on performance
### 6.1 A case study
n-body simulation problem in physics:
- Algorithms and data structure: By representing the physical objectss as leaves ina binary tree; higer nodes represent clusters of objects. The tree has roughly log(n) level, and the resuling O(nlgn).
- Algorithm tuning
- Data structure reorganization.
- Code Tuning: 64-bit double-precision floating point numbers could be replaced by 32-bit single-precision numbers; that change halved the run time. Rewriting one high-weight code in assembly language increased its speed by a factor of 25.
- Hardware. A floating point accelerator.

### 6.2 Design levels
many levels, ranging from its high-level software structure down to the transistors in its hardware.
- Problem Definition.
- System Structure. The decomposition of a large system into modules is probably the single most important factor in determing its performance.
- Algorithms and Data structures. The keys to a fast module are usually the structures that represent its data and the algorithms that operate on the data.
- Code tuning.
- System software. New database system? Operating system? Compiler optimization?
- Hardware.

### 6.3 Principles
- The cheapest, fastest and most reliable components of a computer system are thos that aren't there.
- If you need a little speedup, work at the best level.
- If you need a big speedup, work at many levels.


## 7 The back of envelope
### 7.1 Basic skills
- Two answers are better than one
- Quick checks.
- Rules of Thumb. rate of return per year X year == 72, then your money will double, which is the rule of 72. 2^10 == 1024. 
    pie second is a nanocentury. 3.155 X 10^7 seconds in a year.
    10^7 seconds is about 4 months.
Casting out nines: the sums of the digits are equal after "casting out" groups of digits that sum to nine.
- Practice. As with many activities, your estimation skills can only be improved with practice.

### 7.2 performance estimates
Variables also consume metadata, which is an overhead to memory consumption.
Caching also play an important role in speed.
Little experiments can put key parameters at your fingertips. The short time required to do such little experiments today will be more than paid in the time you save by making wise decision tomorrow.

### 7.3 Safety factors
The output of any calculation is only as good as its input.
If the input is unknown, take considerable safety factors into accounts.
In making reliability/availability commitments, we ought to stay back from the objectives we think we can meet by a facotr of ten, to compensate for our ignorance. In estimating size and cost and schedule, we should be conservative by a factor of two or four to compensate for our ignorance.

### 7.4 Little's Law
The average number of things in the system is the product of the average rae at which things elave the system and the average time each one `spends` in the system.
The average number of objects in a queue is the product of the entry rate and the average `holding` time.

### 7.5 Principles
Everything should be mad as simple as possible, but no simpler.

### 7.6 Problems
death rate ~= 1/life expectancy, according to little's law.

### 7.7 future reading
Fermi problems

## 8. Algorithm Design Techniques
### 8.1 the problem and a simple algorithm
Find the maximum sum in any contiguous subvector of the input.
Brute force: O(n^3)

### 8.2 Two quadratic algorithms
    maxsofar = 0
    for i = [0,n)
        sum = 0
        for j = [i,n)
            sum += x[j]
            maxsofar = max(maxsofar, sum)

    cumarr[-1] = 0
    for i = [0,n)
        cumarr[i] = cumarr[i-1] + x[i]
    maxsofar = 0
    for i = [0,n)
        for j = [i,n)
            sum = cumarr[j] = cumar[i-1]
            maxsofar = max(maxsofar, sum)

### 8.3 A divide-and conque algorithm
To solve a problem of size n, recursivley solve two subproblems of size approximately n/2, and combine their solutions to yield a solution to the complete problem.
Maximum subarray problem, or stock puchasing problem with just one opportunity
    float maxsum3(l,u)
        if (l>u)
            return 0
        if (l == u)
            return max(0, x[l])
        m = (l+u)/2
        lmax = sum = 0
        for (i=m; i>=l; i--)
            sum += x[i]
            lmax = max(lmax, sum)
        rmax = sum = 0
        for i = (m,u]
            sum += x[i]
            rmax = max(rmax, sum)
        return max(lmax+rmax, maxsum3(l,m), maxsum3(m,u))
T(n) = 2T(n/2) + O(n) => T(n) = O(nlgn)

### 8.4 Scanning algorithm
Kadane's algorithm
The maximum-sum subarray in the first i elements is either in the first i-1 elements(maxsofar), or it ends in position i(which we'll store in maxendinghere)
    maxsofar = 0
    maxendinghere = 0
    for i = [0,n)
    /* invariant: maxendinghere and maxsoar are accurate for x[0..i-1]*/
    maxendinghere = max(maxendinghere+x[i],0)
    maxsofar = max(maxsofar, maxendinghere)
It is a linear and online algorithm.

### 8.5 What does it matter?
When we are comparing cubic, quaratic and linear algorithms with one another, th econstan factors of the programs don't matter much.

### 8.6 Principles
- Save state to avoid recomputation.  The simple form of dynamic programming avoid use time to compute them by using space to store results.
- Preprocess information inta data strucutres.
- Divided-and-conquer algorithms. 
- Scanning algorithms. Problems on arrays can often be solved by asking "how can I extend a solution for x[0...i-1] to a solution for x[0...i]?"
- Cumulatives.
- Lower bounds. Algorithm designers sleep peacefully only when thye know their algorithms are the best possible.

### 8.7 Problems
9. We defined the maximum subvector of an array of negative numbers to be zero, the sum of the empty subvector. Suppose that we had instead defined the maximum subvector to be the value of the largest element; how would you change the various programs?
maxsofar=-00 or x[0]
10. Suppose that we wished to find the subvector with the sum closet to zero rather than that with maximum su. What is the most efficient algorithm you can design for this task? What algorithm design techniques are applicable? What if we wished to find the subvector with the sum closest to a given real number?
Using cumulative array, sort it and find two adjacent values  with the minimum gap.
11. A turnpike consists of n-1 stretches of road between n toll stations; each stretch has an associated cost of travel. It is trivial to tell the cost of going between any two stations in O(n) time using only an array of the costs or in contant time using a table with O(n^2) entries. Descrie a data structure that requires O(n) space but allows the cost of any route to be computed in constant time.
An accumulative array will do it.
13. In the maximum subarray problem we are given an nxn array of reals, and we must find the maximum sum contained in any rectangular subarray. What is the complexity of this problem?
Fix left and right column, and use Kadane's algorithm to calculate the maximum subarray of each sum of every row from left to right column. The run time is O(n^3).
14. Given integers m and n and the real vector x[n] , find the integer i (0<=i\<n-m) such that the sum x[i]+...+x[i+m] is nearest zero.
Use accumulative array.

### 8.8 Future Reading
Only extensive study and practice can put algorithm design techniques at your fingertips.


## 9. Code Tuning
Good programmers keep efficiency in context.

### 9.1 Typical story
Profile the program to find out the bottleneck.
The principle of caching: data that is accessed most often should be the cheapest to access.

### 9.2 A first aid sampler
1. Problem one - Integer Remainders
C remainder operator % can be very expensive, ten times slower than most arithmetic operations.
Fetch RAM into caches vs computation???
It was futile to try to speed up the computations in programs that spend their time doing input and output. On modern architecutres, it is equally futile to try to reduce computation time when so many of the cycles are spent accessing memory.
2. Problem two - functions, Macros, and Inline Code.
C++ allows one to request that a function be compiled inline, which combines the clean semantics of functions with the low overhead of macros.
3. Problem Three - Sequential search.
Reduce check in loop to reduce run time.
On modern machines, loop unrolling can help to avoid pipeline stalls, to reduce branches, and to increase instruction-level parallelism.
4. Problem Four - computing spherical distances.
Rather than using latitudes and longitudes, represent a point's location on the surfaceof the globe by its x, y and z coordinates. Thus the data strucutre is an array that holds each point's latitude and longitude as well as its three Cartesian coordiantes. Its distance to a point in S is computed as the sum of the squres fo the differences in the three dimensions, which is much cheaper than computing one trigonometric function.
Code turning solves the problem with a couple of dozen lines of code, while algorithmica and data strucutre changes would have required many hundreds of lines.

### 9.3 Major surgery - binary search
orginal binary search: (find any occurrence)
    l = 0; u = n - 1;
    loop
        /*invariant: if t is present, it is in x[l...u]*/
    if l > u
        p = -1; break;
    m = l + (u-l)/2;
    case
        x[m] < t: l = m+1;
        x[m] == t: p = m; break;
        x[m] >t: u = m - 1;

Improved binary search: (only find the first occurrence)
    l = -1; u = n;
    while l+1 != u
        /* invariant: x[l] < t && x[u] >= t && l < u */
        m = l + (u-l)/2;
        if x[m] < t
            l = m
        else 
            u = m
        /* assert l+1==u && x[l]<t && x[u]>=t */
        p = u
        if p>=n || x[p] != t
            p = -1
// It compares only once and have only two branches, which is more efficient.

Further improved algorithm removes the overhead of loop contorl and the division of i by tow by unrolling the entire loop. Must know the size of the array pre-computionally.


### 9.4 Principles
The most important principle about code tuning is that it should be done rarely.
- The role of efficiency. Many other properties of software are as important as efficiency, such as correctness, functionality, and maintainablity.
- Measurement Tools. Profiling points to the critical areas; for other parts we follow the wise maxim of, "If it ain't broke, don't fix it".
- Design Levels. Before tuning code, we should make sure that other approaches don't provide a more effective solution.
- When speedups are slowdowns. People who play  with bits should expect to get bitten.

- Exploit common cases: Caching a list of the most common kind of record.
- Exploit an Algebraic Identity: to replace an expensive remainder operation with a cheap comparison.
- Collapsing a procedure hiearchy: replacing a function with a macro, but writing the code inline made no further difference.
- Using a sentinel to combine Tests and loop unrolling.
- Data structure augmentation and Exploit an algebraic identity.
- Combining tests reduces the number of array comparisions.
We can turn code for other purposes, such as reducing paging or increasing a cache hit ratio.

### 9.5 Problems.
1. malloc optimzation
If a fixed length of memory is request repeatly, assign a group of fixed length of memory, and return them on demand.
4. # define max(a,b)((a)>(b)?(a):(b)) will call the function two times, which produces a run time of 2^n.
5. How do the various binary search algorithms behave if they are applied to unsorted arrays?
If found, the target is indeed in the array. Otherwise, it will find two adjacjent elements in the array and report that target is not in the array.
6. How would you implement functions such as isdigit, isupper and islower to determine the type of characters?
if c>='0' && c <='9'  // put the test most likely to succeed first.
Store several bits in each element of a table, then extract them with a logical :
#define isupper(c) (bigtable[c]&UPPER)
#define isalnum(c) (bigtable[c]&(DIGIT|LOWER|UPPER))
inspect ctype.h
7. given a very long sequence of byese, how would you efficiently count the total number of one bits?
Look at the bits in order, or perform a lookup in a table of 2^16 elements.
The second approach: to count the number of each input unit in the iput, and then at the end take the sum of that number mulitplied by the number of one bits in that unit.
8. How can sentinel be sued in a program to find the mximum element in an array?
Compute the maximum element of x[0...n-1], using x[n] as sentinel:
    i = 0;
    while i < n
        max = x[i]
        x[n] = max
        i ++
        while (x[i]<max)
            i++

10. Hashing is more time-efficient than binary search, but consuming more space.

11. Replacing the function evaluation by serval 72-element tables.

12. The following code use 2n multiplications
    y = a[0]
    xi = 1
    for i = [1,n]
        xi = x * xi
        y = y + a[i] * xi

    The following use only n multiplications:
    y = a[n]
    for i = [n-1,0]
        y = x * y + a[i]


### Appendix 3 Cost models for time and space
Overhead associated with new operator.
All of the arithmetic and logical(bitwise) operations have about the same cost, with the exception of the division and remainder operator, which are an order-of-magnitued omre expensive.
The function versions of comparing and swapping cost a little more than their inline couterparts.
The floating operations cos tabout as much as their integer couterparts, and the array operations are equally inexpensive.
The rand function is relatively inexpensive, swaure root is an order of magnitued greater than basic arithmetic operations(twice the costof a division), simple trigonometric operations cost twice that, and advanced trigonometric operations run into microseconds.
Memory allocation is more expensive yet.


#### Appendix 4: Rules for code tuning
- Space-for-time rules
    - Data structure augmentation: augment the strucutre with extra information or chaning the information withinin the structure.
    - Store precomputed results. The cost of recomputing an expensive function can be reduced by computing the function only once and storing the results. Subsequent request for the function are hten handled by table lookup rather then by computing the function. 
    - Caching.
    - Lazy evaluation. Never evaluate an item until it is needed.
- Time-for-space rules
    - Packing.  Although packing sometimes trades time for space, the smaller representations are often also faster to process.
    - Interpreters.
- Loop rules
    - Code Motion out of loops. Better to perform a certain computation only once, outside the loop.
    - Combing Tests. An efficient inner loop should contain as few tests as possible.
    - Sentinels. Place a sentinel at the boundary of a data structure to reduce the cost of testing whether our search has exhausted the structure or yield clean code for arrays, linked lists, bins and binary search trees.
    - Loop unrolling. It can remove the cost of modifying loop indices, and also help to avoid pipeline stalls, to reduce branches, and to increase instruction-level parallelism.
    - Transfer-Driven loop unrolling.
    - Unconditional branch removal.
    - Loop fusion.
- Logic rules
    - Exploit algebraic identities.
    - Short-circuiting monotone functions.
    - Reordering tests. Inexpensive and often successful tests precede expensive and rarely successful tests.
    - Precompute logical functions. A logical function over a small finite domain can be replaced by a lookup in a table that represent the domain.
    - boolean variable elimination. Remove boolean variables by replacing the assignment to boolean variables by a if-else statement.
- Procedure rules
    - Collapsing function hierachies.
    - Exploit common cases. Functions shold be organized to handle all cases correctly and comon case efficiently.
    - Coroutines. A multiple-pass algorithm can often be turned into a single-pass algorithm by use of coroutines.
    - Transformations on Recursive functions. Rewrite recursion to iteration. Convert the recursion to iteration by using an explicit program stack. Romove tail recursion. It is often more efficient to solve small subproblems by use of an auxiliary procedure, rather than recurring down to problems of size zero or one.
    - Parallelism. A program should be structured to exploit as much of the parallelism as possible in the underlying hardware.
- Expression rules
    - Compile-time initialzation. As many variables as possible should be initialized before program exectution.
    - Exploit algebraic identities.
    - Common subexpression elimination.
    - Pairing computation. If two similar expression are frequently evaluated together, then we should make a new prodecure that evaluates them as a pair.
    - Exploit world parallelism. Use the full datapath width of the underlying computer architecture to evaluate expensive expressions.


### 10. Squeezing Space
Reducing space often has desirable side-effects on run time.

### 10.1 The Key - Simplicity
Simplicity can reduce memory space usage and also code space, not only source code but object code.

### 10.2 An illustrative problem
Represent a 10,000 x 10,000 matrix with a million active entries on a hundred-megabyte computer.
Solution to sparse matrix: use an array to represent all columns, and linked lists to represent the active elements in a given column.
parallel array to represent the pointers.
Key indexing.
Classic problem: sparse array representation(a sparse aray is one in which most entires have the same value, usually zero).

### 10.3 Techniques for data space
- Don't store, recompute. E.g., matrix of points, generator programs for huge objects, installing software from a CD or DVD-ROM. "Store, don't retransmit"
- Sparse Data structures. 
- Key indexing. If we use a key to be stored as an index into a table, then we need not sotre the key itself; rather, we store only its relevant attributes.
- Storing pointers to shared large objects.
- Data compression.
- Allocation Policies. Dynamic allocations avoid waste by allocating records as needed.
- Garbage collection.
- Sharing storage: c[max(i,j), min(i,j)]
- On modern computing systems, it may be critical to use cache-sensitive memory layouts. Even though arrays touch more data than linked lists, they are faster because their sequential memory accesses interact efficiently with the system's cache.

### 10.4 Techniques for code space
- Function definition.
- Intepreters. Replace a long line of program text with a four-byte command to an interpreter.
- Translation to Machine language. As a last resort, a programmer might consider coding key parts of a large system into assembly language by hand.

### 10.5 Principles
- The cost of space. Know the cost of space before you set out to reduce that cost.
- The "Host Spots" of space.
- Measuing Space.
- Tradeoffs. Often reducing space may have a positive impact on the other dimensions.
- Work with the Environment. Compiler, run-time system, memory allocation policies, and pageing policies.
- Use the right tool for the job.

### 10.6 Problems
1. Every high-level language instruction that accessed one of the packed fields compiled into many machine instructions.
2. Write a program to build a sparse-matrix data structure?
((x,y), pointnum) build an array sorted by x then y.
5. a function with a difference table.
6. c = a*10 + b. Decode it with / and %. Discuss the time and space tradeoffs iinvolved in replacing those operatoins by logical operations or table lookups.
Replacing the expensive / and % operations with a table lookup cost 200 bytes of primary memory but reduced the read time almost to its original cost. Thus 200 bytes bought more disk space.
Or c= (a<<b) | b, and a = c>>4 and b = c&0xF. Not only shifting and maksing commonly faster than multiplying and dividing, but common utitlies like a hex dump could display the encoded data ina readable form.
10. raw sound files .wav->.mp3, raw image .bmp ->.gif or .jpg, raw motion picture files .avi -> mpg

### 10.7 Future reading
dynamic programming, retrograde analysis.

# Part III: The product

## Column 11: sorting
### 11.1 Insertion sort
Insertion sot is the method used to sort cards.
    for i = [1,n)
        /* invariant: x[0...i-1] is sorted */
        /* goal: sift x[i] down to its proper place in x[0...i]*/
version 1:
    for i = [1,n)
        for (j=i; j>0 && x[j-1]>x[j];j--)
            swap(j-1,j)
version 2 (inline function)
    for i = [1,n)
        for (j=i; j>0&& x[j-1]>x[j];j--)
            t = x[j]; x[j] = x[j-1]; x[j-1] =t;
version 3:
    for i = [1,n)
        t = x[i]
        for (j=i; j>0&&x[j-1]>t; j--)
            x[j]=x[j-1]
        x[j]=t 
Note: if this input array is already almost sorted, thought, Inserstion sort is much faster than quicksort because each element sifts down just a short distance.

### 11.2 A simple quicksort
"Quicksort" by C.A.R. Hoare
A partition operation goes a long way towards sorting the sequence, while the sift operation of Insertion sort mananges to get just one more element into the right place.
    void qsort(l,u)
        if l>=u then
            /* at most one element, do nothing */
            return
        /* goal: partition array around a particular value, which is eventually place in its correct position p. */
        qsort(l, p-1)
        qsort(p+1, u)

A simple scheme from Nico Lomuto:
    Given the value t, we are to rerange x[a..b] and ocmpute the index m(for the "middle") such that all elements less than t are to one side of m, while all othe relements are on the other side. Accomplish the job with a simple for loop that scans the array from left to right, using the variable i and m to maintain the following invariant in array x. Two-sided partitioning in most presentation is tricky, and easy to have bug in it.
    t|a   <t      m | >=t     |i   ?    b|
    m = a - 1
    for i = [a,b]
        if x[i] < t
            swap(++m, i)
In quickSort, we'll partition the array x[l..u] around the value t=x[l], a will therefore be l+1 and b will be u.
    (l)t| <t     m| >=t    | i ?   u|
    we then swap x[l] with x[m], otherwise x[l] will never be moved.
    Then recursive call qsort(l, m-1) and qsort(m+1, u).

    void qsort(l,u)
        if (l>=u)
            return
        m = l
        for i = [l+1, u]
            /* invariant: x[l+1..m] < x[l] && x[m+1...i-1] >= x[l]*/
            if (x[i] < x[l])
                swap(++m,i)
        swap(l,m)
        /* x[l..m-1]<x[m] <= x[m+1..u] */
        qsort(l, m-1)
        qsort(m+1, u)
This QuickSort runs in O(nlgn) time and O(lgn) stack space on the average. The lower bound that any comparison-based sort must use O(nlgn) comparisons; Quicksrot is there close to optimal.

### 11.3 Better quicksorts
If an array of n identical elements, the qsort1 function blows this case badly. Each of n-1 partitioning passese use O(n) to peel off a single element, so the total run time is O(n^2).
To solve that, use a careful two-sided partitioning code, using this loop invariant:
    l(t)|  <=t    i| ???    |j >=t    u|
Stop each scan on equal elements, and swap them.
    void qsort3(l, u)
        if l >= u
            return
        t = x[l]; i=l; j = u+1
        loop
            do i++ while i<=u && x[i]< t
            do j-- while x[j]>t
            if i > j
                break
            swap(i,j)
        swap(l, j)
        qsort3(l, j-1)
        qsort3(j+1, u)
This quicksort tames teh denegerate case of all equal elements and also perform less swaps on the average than qsort1.
But, if the array is already sorted in increasing order, for instance, it will partition around the smallest element, and then the second smalleset,, in O(n^2) time. We do far better to choose a partitioning element at random: swap(l, randint(l,u));

When Quicksort is called on a small subarray, we do nothing.Call Insertion Sort on the final almost sorted array.
    qsort4(0,n-1); isort3()
    void qsort4(l,u)
        if u - l < cutoff
            return
        swap(l, randint(l,u))
        t = x[l]; i = l; j = u + 1;
        loop
            do i++; while i <= u && x[i]<t
            do j--; while x[j]>t
            if i > j
                break
            tmp = x[i]; x[i]= x[j]; x[j]= tmp;
        swap(l,j)
        qsort4(l, j-1)
        qsort4(j+1, u)

### 11.4 Principles
The C library qsort is easy and relatively fast; it is slower than hand-made Quicksort only because its general and flexible interface uses a function call for each comparison. The C++ library sort has the simplest interface, and also has a particularly efficient implementation. If a system sort can meet your need, don't even consider writing your own code.

### 11.5 Problems
2. remove conditoinal check and final swap in qsort:
t(l) |  ???  i|  <t    | m   >=t    u|
    m = i = u + 1
    do
        while x[--i] <t
            ;
        swap(--m, i)
    while i != l
9. Find kth-smallest element in an aray in O(n).
    void select(l,u,k)
            pre l<=k<=u
            post x[l..k-1]<=x[k]<=x[k+1..u]
        if l >=u
            return
        swap(l, randint(l,u))
        t = x[l]; i = l; j = u+1
        loop
            do i++; while i <=u && x[i]<t
            do j--; while x[j]>t
            if i>j
                break
            tmp = x[i]; x[i]=x[j];x[j]=tmp;
        swap(l,j)
        if j < k
            select(j+1, u, k)
        else if j>k
            select(l,j-1,k)
Because the recursion is the last action of the function, it could be transformed into a while loop.
11. Dutch flag problem
 | < t   |  = t  | > t
Use three pointers?!

## Column 12: A Sample problem
### 12.1 The problem
Random sampling
### 12.2 One solution
Knuth's algorithm to pick m random number out of n numbers i order:
    select = m
    remaining = n
    for i = [0, n)
        if (bigrand()%remainging) < select
            print i
            select--
        remaining --

    void genknuth(int m, int n)
    { for int(i=0; i<n; i++)
        /* select m of remaining n-i */
        if((bigrand() % (n-i) < m) {
            cout <<i <<"\n";
            m--;
        }
    }

### 12.3 The design space
Solving current or future problem require preparing, either taking classes or studying books, or mental exercise of asking how we might have solved a problem differently.
- Use set:
    initialize set S to empty
    size = 0
    while size < m do
        t = bigrand()%n
        if t is not in S
            insert t into S
            size ++
    print the elements of S in sorted order
- Shuffle an n-element array that contains the number 0...n-1, and then sort the first m for the output.
    Knuth's algorithm
    for i = [0,n)
        swap(i, randint(i,n-1))

### 12.4 Principles
- Understanding the preceived problem.
- Specify an abstract problem.
- Explore the Design space. Don't jump into "The" solution.
- Implement One solution. At other times we have to prototype the top few to choose the bes. Weh should strive to implement the chosen design in straightforward code, using the most powerful operations avaiable.
> The purpose of software engineering is to control complexity, not to create it.
- Retrospect.
> There remains always something to do; with sufficient study and penetration, we can always improve any solution, and, in any case, we can always improve our understanding of the solution.

### 12.5 Problems
1. The C library rand() function typically returns about fiftenn random bits. use that function to implement a function bigrand() to return at least 30 random bits, and a function randint(l,u) to return a random integer in the range [l,u].
    int bigrand() {
        return RADN_MAX * rand() + rand();
    }
    int randint(int l,int u) { return l + bigrand()%(u-l+1);}
2. Describe an algorithm that choose each element equiprobably, but choose some subsets with greater probability than others.
Select m integers from the range 0...n-1,  choose the number i at random in the range, and then report the numbers i, i+1,..., i+m-1, possibly wrapping around to 0. Tis method chooses each integer with probability m/n, but is strongly biased twoards certains subsets.
3. *** Bernoulli trail ***: success, which occurs with probability p, and failure, which occurs with probablity q = 1-p.
E[X]= 1/p means it takes 1/p time to get a success.
Coupon Collecter's problem: how many basebasll cards must I collect to make sure I have all n? the answer is roughly nln(n).
Birthday Paradox: in any group of 23 or more people, two are likely(at least 50%) to shared a birthday.
In general, two balls are likely to share one of n urns if there are O(sqrt(n)) balls.
10. How would you select one of n objects at random, where you see the objects sequentitally but you don't know the value of n beofrehand?
    i = 0
    while more input lines
        with probability 1.0/++i
            choice = this iput line
    print choice
11. A promotional game consists of a card containing 16 spots, which hide a random permutation of the integers 1..16. The player rubs the dots off the card to expoese the hidden integers. If the integers 3 is ever expoese then the card loses; if 1 and 2 are both revealed then the card wins. Describe the probablity thatn randomly choosing a sequence of spots wins the game.
4..16 are irrelevant. We just need to consider 1..3, only if 3 is the last one of three, we will win, so the probability is 1/3. Don't be misled by the statement, Don't use computer just because it is available.


## Column 13: Searching
### 13.1 The interface
### 13.2 Linear structures
Arrays are excellent structure for sets when the size is known in advance. Because our arrays are sorted, we could use binary search to build a member function that runs in O(lgn) time.

*** Put a sentinel in the end of array or linked list to make code simpler ***

If the size of a set is not know in advance, linked lists are a prime candidate for set representation; list also remove the cost of sliding over elements in an insertion.
To insert an item into a sorted linked list, we walk along the list.
The simplest code is a recursive function:
    void insert(t)
        head = rinsert(head,t)
    node *rinsert(p,t)
        if p->val < t
            p->next = rinsert(p-next, t)
        else if p->val>t
            p = new node(t,p)
            n ++
        return p
When a programming problem is hidden under a pile of special cases, recursion often leads to code as straightforward as this.

However, the recursion causes lots of overhead.
Storage allocation is about two orders of magnitued more time-cosuming than most simple operations.
Allocating the nodes individually will consume more memory what overflows the Level-2 cache.
Like most code-tuning techniques, caching and recursion removal sometimes yield great benefit, and other times have no effect.
large list must read 8-bytes nodes into a cache to access the 4-byte integer. Array access data with perfect predictability, while access patterns for lists bounce all around memory.

### 13.3 Binary search trees
We initialize the tree by setting the root to be empty, and perform the other actions by calling recursive functions.
    void insert(int t) { root = rinsert(root, t);}
    node *rinsert(p, t)
        if p == 0
            p = new node(t)
            n ++
        else if t < p->val
            p->left = rinsert(p->left,t)
        else if t > p->val
            p-right = rinsert(p->right, t)
        // do nothing if p->val == t
        return p

    void inorderTraverse(p)
        if p == 0
            return
        traverse(p->left)
        v[vn++] = p->val
        traverse(p->right)
This simple BST avoids the complex balancing scheme used by the STL.
Convert recursion to iteration and use sentinel, and allocate all nodes all at once for speedup.

### 13.4 Strucutres for integers
#### BitVectors
enum { BITSPERWORD=32, SHIFT=5, MASK=0x1F};
int n, hi, *x;
void set(int i) { x[i>>SHIFT] |= (1<<(i&MASK));}
void clr(int i) { x[i>>SHIFT] &= ~(1<<i&MASK);)}
int test(int i) { return x[i>>SHIFT] & (1<<(i&MASK));}
InsetBitVec(maxelements, maxval) 
    hi = maxval
    x = new int[1+hi/BITSPERWORD]
    for i = [0,hi)
        clr(i)
    n = 0
if the maximum value n is small enough so that the bit vector fits in main memory, then the structure is remarkably efficient.
### Bins (a kind of hashing)
The m bins can be views as a kind of hashing. The integers in each bin are represented by a sorted linked list. Because the integer are uniformly distributed, each linked list has expected length one.
mapping: t*bins/maxval can lead to numberical overflow, instead we change it to t/(1+maxval/bins)
    void insert(t)
        i = t/(1+maxval/bins)
        bin[i] = rinsert[bin[i], t]
Bins are fast.

### 13.5 Principles
Set representation |  O( time per operatoin)   | Total time     Space in words
                      Init    insert  report
========               ===============            ==============
Sorted Array       |   1       m        m      |    O(m^2)           m
Sorted List        |   1       m       m       |    O(m^2)           2m
Binary Tree        |   1      lg m     m       |    O(mlgm)          3m
Bins               |   m       1       m       |    O(m)             3m
Bit vector         |   n       1       n       |    O(n)             n/b

- The role of libaries: when you face a problme with data structures, your first inclination should be to search for a general tool that solves the problem. In this case, though, special-purpose code could exploit properties of the particular problem to be much faster.
- The Importance of Space. Time increased substantially as we overflowed the memory at magic boundaries such as Level-2 cache size and free RAM.
- Code tuning techniques. Replace general-purpose memory allocation with a single allocation of a large block, rewrite a recursive function to be iterative, and use sentinels.

### 13.6 Problems
1. Bob Floyd's algorithm to generate a sorted set of random integers. When all numbers are sorted, it will trigger the worst case in binary search tree.
6. Inserting nodes in increasing order will provoke worst-case behaviro for bins and binary search tree.
9. replace division with shifting
    goal = n/m
    binshift = 1
    for (i=2; i<goal; i*=2) 
        binshift++
    nbins = 1 + (n>>binshift)
    p in bin[t>>binshift]
10. Ordered hash table: implemented by small array and linked lists.


## Column 14: heaps
- Sorting: heapsort never taks more than O(nlgn) time to srot an n-element array,and uses just a few words of extra space.
- Priority Queues. Insert new elements and extract the smallest element in the set requires O(lgn) time.

### 14.1 The Data Structure
Heap properties:
- Order: the value at any node is less than or equal to the values of the node's children This implies that the least element of the set is at the root of the tree, but it doesn't say anything about the relavtive order of left and right children.
- Shape: Has its terminal nodes on at most two levels, with those on the bottom level as far left as possible. There is no "holes" in the tree; if it contains n nodes, no nodeis at a distance more than lgn from the root.

Heaps use a one-based array; the easiest approach in C is to declare x[n+1] and waste element x[0]. The root is in x[1].
    root = 1; value(i) = x[i]; leftchild(i)=2*i; rightchild(i)=2*i+1; parent(i)=i/2;
In this representation, the `shape` property is guanranteed.
for 2<=i<=n, x[i/2]<=x[i]

### 14.2 Two critical functions
siftup:
    loop
        /* invariant: heap(1,n) except perhaps between i and its parent */

    void siftup(n)
            pre n>0 && headp(1,n-1)
            post heap(1,n)
        i=n
        loop
        /* invariant: heap(1,n) except perhaps between i and its parent */
            if i == 1
                break
            p = i/2
            if x[p] <= x[i]
                break
            swap(p,i)
            i = p

Siftdown:
    void siftdown(n)
            pre heap(2,n) && n >= 0
            post heap(1,n)
        i = 1
        loop
            /*invariant: heap(1,n) except perhaps between i and its (0,1 or 2) children
            c = 2*i
            if c>n
                break
            /* c is the left child of i */
            if c+1 <= n
                /* c+1 is the right child of i */
                if x[c+1] < x[c]
                    c++
            /* c is the lesser child of i */
            if x[i] <= x[c]
                break
            swap(i,c)
            i = c

### 14.3 Priority Queues
Every data structure has two sides: specification and implementation.
"multiset" or "bag" can contain multiple copies of the same element.
void insert(t)
    pre |S| < maxsize
    post current S = original S U{t}
Extract any extreme element under a total ordering.

O(n^2) vs O(nlgn) when n is one million, the run times of the two are three hours and one second.
    void insert(t)
        if n >= maxsize
            /* report error */
        n++
        x[n] = t
        /* heap(1,n-1) */
        siftup(n)
        /*heap(1,n)*/

code:
    for (i=n; i>1&&x[p=i/2]>x[i];i=p) swap(p,i)

    void extractmin()
    if n<1
        /* report error */
    t = x[1]
    x[1] = x[n--]
    /* heap(2,n)*/
    siftdown(n)
    /* heap(1,n)*/
    return t
code:
    for (i=1;(c=2*i)<=n;i=c) {
        if (c+1<=n&&x[c+1]<x[c]) c++;
        if (x[i]<x[c]) break;
        swap(c,i);
}

### 14.4 A sorting algorithm
The Heapsort algorithm is a two-stage process; the first n steps build the array into a Maxheap, and the next n stesp extract the elements in decreasing order and build the final sorted sequence, right to left.
    | heap, <=    i| sorted, >=  |n
   fist stage:
    for i = [2,n] 
        /* invariant: heap(1, i-1) */
        siftup(i)
        /* heap(1,i) */  // This will build up the heap in O(nlgn)
    // While we could build a heap bottom up, merge different heaps takes O(n), as different layers of heaps takes different steps to sift down.
    second stage:
    for (i=n; i>=2; i--)
        /* heap(1,i) && sorted(i+1,n) && x[1..i]<=x[i+1..n]
        swap(1,i)
        /*heap(2,i-1) && sorted(i,n) && x[1..i-1]<=x[i..n]
        siftdown(i-1)
        /* heap(1,i-1) && sorted(i,n)&& x[1..i-1]>=x[i..n]

### 14.5 Principles
- Efficiency. The shape property guarantees that all nodes in a heap are within log2n levels of the root; functions siftup and siftdown have efficient run times precisely because the trees are balanced. Heapsort avoids using extra space by overlaying two abstract structures(a heap and a sequence) in one implementatino array.
- Correctness. To write code for a loop we first state its invariant precisely; the loop then makes progress towards termination while preserving its invariant.
- Abstraction. Good engineers distinguish between what a component does(the abstraction seen by user) and how it does it(the implementation inside the black box).
    - Procedural abstraction.
    - Abstract Data types. What a data type does is given by its methods and their specifications; how it does is up to the implementation. Their implementation may, of course, have an impact on the problem's performance.

### 14.6 Problems
1.  Make the heap operation faster?
The siftdown function can be made faster by moving the swap assignments to and from the temporary variable out of its loop. The siftup function can be made faster by moving code out of loops and by placing a sentinel element in x[0] to remove the test if i==1.
2. Modify siftdown to have the following specification:
    void sidtdown(l,u)
        pre heap(l+1, u)
        post heap(l,u)
Build a heap in O(n)
    for (i=n-1; i >=1; i--)
        /Invariant: maxheap(i+1,n)*/
        siftdown(i,n)
        /* maxheap(i,n)*/
Because maxheap(l,n) is true for all l>n/2, the bound n-1 in the for loop can be changed to n/2, thus O(n).
3. Implement a heapsort to run as fast as possible.
    for (i=n/2; i>=1; i--)
        siftdown(i,n)
    for (i=n;i>=2;i--)
        swap(1,i)
        siftdown(1,i-1)
Its running time remains O(nlgn), but with a smaller constant than in the original heapsort.
4. How might the heap implementation of priority queues be used to solve the following problem? How do your answer change when the inputs are sorted?
a. Construct a Huffman code
Extract two minimums, merge and insert into heap. If sorted, it is linear time.
b. Compute the sumof a large set of floating point numbers.
Sum the smallest number first.
c. Find the million largest of a billion numbers stored on a file.
A million minHeap to store the million largest numbers seen so far.
d. Merge many small sorted files into none large sorted file( a disk-based merge sort)
A heap can be used to merge sorted files by representing the next element in each file; the iterative step selects the smallest element from the heap and inserts its successor into the heap. The next element to be output from n files can be chosen in O(lgn) times.
5. bin packing problem. assigning a set of n weights (each between zero and one ) to a mininal number of unit-capacity bins. 
A heap-like structure is placed over the sequence of bins; each node in the heap tells the amount of space left in the least full bin among its descendants. When deciding where to place a new weight, the search goes left if it can, and right if it must; that require time proportinal to the heap's depth of O(lgn). After the weight is inserted, the path is traversed up to fix the weights in the heap.
6. A common implementation of sequential files on disk has each block point to its successor, which may be any block. Add addiontal pointer to each node, to reduce the ith block to be read in logi.
    void path(n)
            pre n >= 0
            post path to n is printed
        if n == 0
            print "start at 0"
        else if even(n)
            path(n/2)
            print "double to ", n
        else 
            path(n-1)
            print "increment to ", n
7. On some computers the most expensive part of a binary search program is the division by 2 to find the center of the current range. Show how to replace that division with a multiplication by two, assuming that the array to be searched has been constructed properly. Given algorithms for building and search such a table.
The modified binary search begins with i=1, and at each iteration set i to eithr 2i or 2i+1. The element x[1] contains the median, x[2] contains the first quartile, x[3] the thrid quartile, and so on. 
Put an n-element sorted array into "heapsort" order in O(n) and O(1) extra space. to copy a sorted array a of size 2^k -1 into a "heapsearch" array b: the elements in odd position of a go, in order, into the last half of positions of b, positions congruent to 2 modulo 4 go into b's second's quarter, and so on.
9. The simultaneous logarithmic run times of insert and extractmin in the heap implementation of priority queues are within a constant factor of optimal.
10. The basic idea of heaps in familar to sports fans, what is the probability that the second-best player is in the champoinship match?
1/lg(n) or 1/rounds(elimination). All the players defeated by champion could be a second best player.


## Column 15: string of pearls
We are surrounded by strings.
### 15.1 Words
Balanced search tree operate on strings as indivisible objects; these structs are used in most implementations of STL's sets and maps. They always keep the elements in sorted order, so they can efficiently perform operatoins such as finding a predecessor or reporting elements in order. Hashing, on the other hand, peeks in side the characters to compute a hash function, and then scatters keys across a big table. It is very fast on the average, but it does not offer the worst-case performance guarantees of balanced trees, or support other operatoins involving order.
### 15.2 Phrases
Find the longest substrings in a text
suffix array: The pointers in the array together point to every suffix in the string.
then sort and scan through this array comparing adjacent elemtns to find th longtest repreated string.
It runs O(nlgn) time due tow sorting, which is much better than brute force O(n^2).
### 15.3 Generating text
Finte-state Markov chain with stationary transition probabilities.
Generate text based on the k words before it.
Implement a suffix array pointing to the characters, except that it starts only on word boundaries.
    word comparision:
    int wordncomp(char *p, char *q)
        n = k;
        for (; *p==*q; p++, q++)
            if (*p == 0 && --n == 0)
                return 0
        return *p - *q
Then qsort;
Generate random text:
    phrase = first phrase in input array
    loop
        perform a binary search for phrase in word[0..nword-1]
        for all phrases equal in first k words
            select one at random, pointed to by p
        phrase = word following p
        if k-th word of phrase is length 0
        break
        print k-th workd of prase

### 15.4 Principles
- String problems.
- Data structures for strings:
    - Hashing: fast on the average and simple to implement.
    - Balanced trees: guarantee good performance even on perverse inputs, and nicely packaged in most implementations of the C++ Standard Template Library's sets and maps.
    - Suffix arrays. Initialize an array of pointers to every character(or every word) in your text, sort them, and you have a suffix array. It can be scanned or used against binary search to find words or phrases.
- Libraries or Custom-made components. The sets, maps and strings of the C++ STL were very convevient to use, but their general and pwoerful interface meant that they were not as efficient as a special-purpose hash function.

