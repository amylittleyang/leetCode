# Guess Number High or Low
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API <code>guess(int num)</code> which returns 3 possible results (<code>-1</code>, <code>1</code, or <code>0</code>):
```
-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!
 ```
Example:
```
n = 10, I pick 6.

Return 6.
```
## Java Solution (Binary Search)
Eliminates half numbers after each call to <code>guess(number)</code> so code runs in <code>O(log(n))</code>. Note after each comparison <code>hi</code> or <code>lo</code>
better be <code>mid+/-1</code> instead of <code>mid</code> to reach the corner cases. (e.g. n=2 and we pick 2)
``` java
public int guessNumber(int n) {
        int lo=1;
        int hi=n;
        int mid=(lo+hi)/2;
        while(lo!=hi) {
            mid=lo + (hi-lo)/2;
            int result = guess(mid);
            if(result == -1) {
                hi=mid-1;
            }else if (result == 1) {
                lo=mid+1;
            }else {
                return mid;
            }
        }
        return lo;
    }
```
