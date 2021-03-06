---
description: DP搞求不懂
---

# 剑指 Offer 14- I. 剪绳子

绳子段切分的越多，乘积越大，且3做因数比2更优。【不理解数学证明】

不断剪去 长度3 并用sum累乘

把绳子切为多个长度为 3 的片段，则留下的最后一段绳子的长度可能为 0,1,2 三种情况。

循环结束的三种情况：

```
  n=n-3==2 
```

n长度剩下2，因 n > 4 跳出循环，return 乘积为sum\*2。

```
  n=n-3==3 
```

长度刚好可以被剪完，因 n > 4 跳出循环，return 乘积为sum\*3。

```
  n=n-3==4 
```

再剪一次的话，长度剩下1，则最后一段绳子长度为 1； 需要把最后的 3 和 1 替换为 2 和 2，因为 2 \* 2 > 3 \* 1； 因 n > 4 跳出循环，return 乘积 sum\*4 即可。

```
public int cuttingRope(int n) {
         if(n<=3)
            return n-1;
        int sum=1;
        while (n>4){
            sum*=3;
            n-=3;
        }

        return sum*n;
    }
时间复杂度为O(n)，空间复杂度为O(1).
```

DP

```java
class Solution {
    public int cuttingRope(int n) {
        int[] dp = new int[n+1]; 
        // n<=3的情况，m>1必须要分段，例如：3必须分成1、2；1、1、1 ，n=3最大分段乘积是2, 同理2的最大乘积为1
        if(n <= 3) return n-1;
        /*解决大问题的时候用到小问题的解并不是这三个数
        真正的dp[1] = 0,dp[2] = 1,dp[3] = 2
        但是当n=4时，4=2+2 2*2=4 而dp[2]=1是不对的
        也就是说当n=1/2/3时，分割后反而比没分割的值要小，当大问题用到dp[j]时，说明已经分成了一个j一个i-j，这两部分又可以再分，但是再分不能比他本身没分割的要小，如果分了更小还不如不分
        所以在这里指定大问题用到的dp[1],dp[2],dp[3]是他本身*/
        dp[1] = 1;dp[2] = 2;dp[3] = 3;
        for(int i = 4; i <= n; i++) {
        //j<=i/2是因为1*3和3*1是一样的，没必要计算在内，只要计算到1*3和2*2就好了
            for(int j = 1; j <= i/2; j++) {
                dp[i] = Math.max(dp[i],dp[i-j]*dp[j]);
            }
        }
        return dp[n];
    }
}

时间复杂度为O(n^2),空间复杂度为O(n)。
```
