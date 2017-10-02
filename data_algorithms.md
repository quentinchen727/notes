## Binary Indexed Tree or Fenwick Tree
Assuming an array[0...n-1]:
1. Find the sum of first i elements;
2. Change value of a specified element of the array arr[i] = x.
Simple array or array of sum cost O(n) for find or change operation.

One Efficient solution is to use `Segment tree` that does both operations in O(lgn) time.
Using Binary Indexed Tree, we can do both in O(lgn). The advantages of binary indexed tree over segment are, requires less
space and very easy to implement.

### Representation
Binary Indexed Tree is represented as an array. Each node of binary indexed tree stores sum of some elements of given array. Size of Binary Indexed Tree is equal to n where n is size of input arry. We use size as n+1 for ease of implementation, this is BITree[n+1].

### Construction
First initilize all values in BITree[] as 0. The we call update() for all indexes to store actual sums.

### Operations
    getSum(index): Return sum of arr[0...index]
    // Returns sum of arr[0...index] using BITree[0...n]. It 
    // assumes that BITree[] is constructed for given array arr[0..n-1]
    1) Initialize sum as 0 and index as index+1.
    2) Do following ***while*** index is greater than 0.
        a) Add BITree[index] to sum.
        b) Go to parent of BITree[index]. Parent can be obtained by removing the last set bit from index, i.e.,
            index = index - (index & (-index))
    3) Return sum.

Important attributes of binary indexed tree:
1. Node at index 0 is a dummy node.
2. A node at index y is parent of a node at index x, if y can be obtained by removing last set bit from binary representation of x..
3. A child x of a node y stores sum of elements from y(exclusive y) and of x(inclusive x).

    update(index, val): Update BIT for operation arr[index] += val
    // Note that arr[] is not changed here. 
    // It changes only BI Tree for the already made change in arr[].
    1) Initialize index as index+1.
    2) Do following while index is smaller than or equal to n.
        a). Add value to BITree[index]
        b). Go to parent of BITree[index]. Parent can be obtained by removing the last set bit from index, i.e., 
            index = index + (index &(-index)).

### How does Binary Indexed Tree work?
The idea is based on the fact that all positive integers can be represented as a sum of powers of 2. For example 19 can be represented as 16+2+1. Every Node of BITree stores a sum of n elements where n is a power of 2. For example, in the above first diagram for getSum(), sum of first 12 elements can be obtained by sum of last 4 elements(from 9 to 12) plus sum of 8 elements(1 to 8). The number of set bits in binary representation of a number n is O(lgn). Therefore we traverse at most O(lgn) nodes in both getSum() and update(). Time complexity of construction is O(lgn) as it calls updates() for all elements.

#### More
Binary Indexed Tree was first used for data compression. Now it is often used for storing frequencies and manipulating cumulative frequency tables.
BIT - Binary Indexed Tree
MaxVal - maximum value which will have non-zero frequency
f[i] - frequency of value with index i, i=1...MaxVal
c[i] - cumulative frequency for index i(f[1] + f[2] + ... + f[i])
tree[i] - sum of frequencies stored in BIT with index i; sometimes we use `tree frequency` instead of sum of frequencies stored in BIT.
NOTE: often we put f[0] = 0, c[0] = 0, tree[0] = 0 , so we could ignore index 0.
Each tree[i] contains some successive number of non-overlapping frequencies. each idx is responsible for indexes from (idx-2^r+1) to idx.
***Responsibility*** is the key in our algorithms.
Update: update tree frequency at all indexes which are responsible for frequency whose value we are chaning.
Read frequency just for that index instead of cumulative one: find the path of intersection to accerlerate the process:
    int readSingle(int idx) {
        int sum = tree[idx];
        if (idx > 0) {
            int parent = idx - (idx &(-idx));
            idx--;
            while (idx != parent) {
                sum -= tree[idx];
                idx -= idx &(-idx);
            }
        }
        return sum;
    }
