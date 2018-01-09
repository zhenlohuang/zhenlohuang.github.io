---
layout: post
title: '2012微软暑期实习生笔试题'
date: 2012-7-5
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
# 2012 Microsoft Intern Hiring Written Test
**1. Suppose that a Selection Sort of 80 items has completed 32 iterations of the main loop. How many items are now guaranteed to be in their final spot (never to be moved again)?**   
A) 16 B) 31 C) 32 D) 39 E) 40

**2. Which Synchronization mechanism(s) is/are used to avoid race conditions among processes/threads in operating systems?**   
A) Mutex B) Mailbox C) Semaphore D) Local procedure call

**3. There is a sequence of n numbers 1, 2, 3,.., n and a stack which can keep m numbers at most. Push the n numbers into the stack following the sequence and pop out randomly. Suppose n is 2 and m is 3, the output sequence may be 1, 2 or 2, 1, so we get 2 different sequences. Suppose n is 7 and m is 5, please choose the output sequences of the stack:**   
A) 1, 2, 3, 4, 5, 6, 7    
B) 7, 6, 5, 4, 3, 2, 1    
C) 5, 6, 4, 3, 7, 2, 1    
D) 1, 7, 6, 5, 4, 3, 2    
E) 3, 2, 1, 7, 5, 6, 4    

**4. What is the result of binary number 01011001 after multiplying by 0111001 and adding 1101110?**   
A) 0001 0100 0011 1111    
B) 0101 0111 0111 0011    
C) 0011 0100 0011 0101    

**5. What is output if you compile and execute the following code?**  

``` cpp
void main()
{
int i = 11;
int const *p = &i;
p++;
printf("%d", *p);
}
```
A) 11 B) 12 C) Garbage value D) Compile error E) None of above

**6. Which of following C++ code is correct?**    
A) int f()
{
int *a = new int(3);
return *a;
}
B) int *f()
{
int a[3] = {1, 2, 3};
return a;
}
C) vector<int> f()
{
vector<int> v(3);
return v;
}
D) void f(int *ret)
{
int a[3] = {1, 2, 3};
ret = a;
return;
}

**7. Given that the 180-degree rotated image of a 5-digit number is another 5-digit number and the difference between the numbers is 78633, what is the original 5-digit number?**    
A) 60918 B) 91086 C) 18609 D) 10968 E) 86901

**8. Which of the following statements are true?**   
A) We can create a binary tree from given inorder and preorder traversal sequences.    
B) We can create a binary tree from given preorder and postorder traversal sequences.    
C) For an almost sorted array, insertion sort can be more effective than Quicksort.    
D) Suppose T(n) is the runtime of resolving a problem with n elements, T(n) = Θ(1) if n = 1; T(n) = 2T(n/2) + Θ(n) if > 1; so T(n) is Θ(n log n).    
E) None of the above.    

**9. Which of the following statements are true?**    
A) Insertion sort and bubble sort are not effcient for large data sets.    
B) Quick sort makes O(n^2) comparisons in the worst case.    
C) There is an array: 7, 6, 5, 4, 3, 2, 1. If using selection sort (ascending), the number of swap operation is 6.    
D) Heap sort uses two heap operations: insertion and root deletion.    
E) None of above.    

**10. Assume both x and y are integers, which one of the followings returns the minimum of the two integers?**    
A) y ^ ((x ^ y) & ~(x < y))    
B) y ^(x ^ y)    
C) x ^ (x ^ y)    
D) (x ^ y) ^ (y ^ x)    
E) None of the above    

**11. The Orchid Pavilion (兰亭集序) is well known as the top of "行书" in history of Chinese literature. The most fascinating sentence "Well I know it is a lie to say that life and death is the same thing, and that longevity and early death make no difference! Alas!" ("周知一死生为虚诞，齐彭殇为妄作。") By counting the characters of the whole content (in Chinese version), the result should be 391 (including punctuation). For these characters written to a text file, please select the possible file size without any data corrupt.**    
A) 782 bytes in UTF-16 encoding    
B) 784 bytes in UTF-16 encoding    
C) 1173 bytes in UTF-8 encoding
D) 1176 bytes in UTF-8 encoding    
E) None of the above    

**12. Fill the blanks inside class definition**

``` cpp
class Test
{
public:
____ int a;
____ int b;
public:
Test::Test(int _a, int _b) : a(_a) {b = _b;}
};
int Test::b;
int _tmain(int argc, __TCHAR *argv[])
{
Test t1(0, 0), t2(1, 1);
t1.b = 10;
t2.b = 20;
printf("%u %u %u %u", t1.a, t1.b, t2.a, t2.b);
}
```
Running result: 0 20 1 20    
A) static/const    
B) const/static    
C) --/static    
D) const static/static    
E) None of the above    

**13. A 3-order B-tree has 2047 key words, what is the maximum height of the tree?**   
A) 11 B) 12 C) 13 D) 14

**14. In C++, which of the following keyword(s) can be used on both a variable and a function?**   
A) static B) virtual C) extern D) inline E) const

**15. What is the result of the following program?**   

``` cpp
char* f(char *str, char ch)
{
char *it1 = str;
char *it2 = str;
while (*it2 != '\0') {
while (*it2 == ch) {
it2++;
}
*it1++ = *it2++;
}
return str;
}
void main(int argc, char *argv[])
{
char *a = new char[10];
strcpy(a, "abcdcccd");

cout << f(a, 'c');
}
```
A) abdcccd B) abdd C) abcc D) abddcccd E) Access Violation

**16. Consider the following definition of a recursive function, power, that will perform exponentiation.**

``` cpp
int power(int b, int e)
{
if (e == 0) return 1;
if (e %2 == 0) return power (b * b, e / 2);
return b * power(b * b, e / 2);
}
```
Asymptotically (渐进地) in terms of the exponent e, the number of calls to power that occur as a result of the call power(b, e) is*
A) logarithmic B) linear C) quadratic D) exponential

**17. Assume a full deck of cards has 52 cards, 2 black suits (spade and club) and 2 red suits (diamond and heart).
If you are given a full deck, and a half deck (with 1 red suit and 1 black suit), what's the possibility for each one getting 2 red cards if taking 2 cards?**    
A) 1/2, 1/2 B) 25/102, 12/50 C) 50/51, 24/25 D) 25/51, 12/25 E) 25/51, 1/2

**18. There is a stack and a sequence of n numbers (i.e., 1, 2, 3, ..., n). Push the n numbers into the stack following the sequence and pop out randomly. How many different sequences of the number we may get? Suppose n is 2, the output sequence may be 1, 2 or 2, 1, so we get 2 different sequences.**    
A) C_2n^n    
B) C_2n^n - C_2n^(n + 1)    
C) ((2n)!) / (n + 1)n!n!    
D) n!    
E) None of the above    

**19. Longest Increasing Subsequence (LIS) means a sequence containing some elements in another sequence by the same order, and the values of elements keeps increasing.
For example, LIS of {2, 1, 4, 2, 3, 7, 4, 6} is {1, 2, 3, 4, 6}, and its LIS length is 5.
Considering an array with N elements, what is the lowest time and space complexity to get the length of LIS?**    
A) Time: N^2, Space: N^2    
B) Time: N^2, Space: N    
C) Time: NlogN, Space: N    
D) Time: N, Space: N    
E) Time: N, Space: C    

**20. What is the output of the following piece of C++ code?**

``` cpp
#include <iostream>
using namespace std;*
struct Item
{
char c;
Item *next;
};
Item Routine1(Item *x)
{
Item *prev = NULL, *curr = x;
while (curr) {
Item *next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
}
return prev;
}
void Routine2(Item *x)
{
Item *curr = x;
while (curr) {
cout << curr->c << " ";
curr = curr->next;
}
}
void _tmain(void)
{
Item *x,
d = {'d', NULL},
c = {'c', &d},
b = {'b', &c},
a = {'a', &b};*
x = Routine1(&a);
Routine2(x);
}    
```
A) cbad B) badc C) dbca D) abcd E) dcba

