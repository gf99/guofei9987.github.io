
### 1. Two Sum

考点
1. 限制加和等于n。不要每一步都判断x+y=n，而是判断n-x=y
2. 两次遍历。考虑不写2个for，而是1次遍历，把元素放到hashtable中

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
    tmp_dict = dict()
    for i, j in enumerate(nums):
        if j in tmp_dict:
            return [tmp_dict[j], i]
        tmp_dict[target - j] = i
```


### 2. Add Two Numbers

考点
1. 对于链表，`curr.next` 指针向下走，这个不多说
2. `curr1=curr1.next if curr1 else curr1` 这个操作，让遍历到尾部后，值保持`curr1=None`，好处是少些几行代码。作为配合，在其它行，可以用 `if curr1` 来判断是否到尾部
3. 链表还有个好处，如本例，`l3` 是含一个空的头的，返回 `l3.next` 即可


```python
def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
    carry=0
    l3=ListNode(None)
    curr1,curr2,curr3=l1,l2,l3
    while curr1 or curr2 or carry:
        value=(curr1.val if curr1 else 0)+(curr2.val if curr2 else 0)+carry
        carry,val=divmod(value,10)
        curr3.next=ListNode(val)
        curr1=curr1.next if curr1 else curr1
        curr2=curr2.next if curr2 else curr2
        curr3=curr3.next
    return l3.next
```

### 3. Longest Substring Without Repeating Characters

考点：
1. 2 sum 那道题里面写过，这种貌似需要两次遍历的题目。都可以想一下能不能用 hashTable 存下历史遍历，从而变成一次遍历。


```python
def lengthOfLongestSubstring(self, s: str) -> int:
    used = dict()
    start = 0
    res = 0
    for i, c in enumerate(s):
        if c in used and start <= used[c]:
            start = used[c] + 1
        else:
            res = max(res, i - start + 1)
        used[c] = i
    return res
```



### 4. Median of Two Sorted Arrays


这个题目其实可以 cheat，俩列表加一起，然后用Python自代的sort，代码就不写了。

套路详解：https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/2481/Share-my-O(log(min(mn)))-solution-with-explanation  

考点：  
1. 中位数的定义：中位数两边的数量相等。
2. 因此，num1 的分割和num2的分割存在一个恒等关系，len(left1)+len(left2)=len(right1)+len(right2)
3. 这也就意味着，给定num1的分割，就立即能计算出num2的分割
4. 本质上还是把两次遍历变成一次
5. 如果if情况过多，考虑换一下两个变量，减少代码量。


(代码还是好难写，奇偶之类的)

```python
def median(A, B):
    m, n = len(A), len(B)
    if m > n:
        A, B, m, n = B, A, n, m

    imin, imax, half_len = 0, m, (m + n + 1) / 2
    while imin <= imax:
        i = (imin + imax) / 2
        j = half_len - i
        if i < m and B[j-1] > A[i]:
            # i is too small, must increase it
            imin = i + 1
        elif i > 0 and A[i-1] > B[j]:
            # i is too big, must decrease it
            imax = i - 1
        else:
            # i is perfect

            if i == 0: max_of_left = B[j-1]
            elif j == 0: max_of_left = A[i-1]
            else: max_of_left = max(A[i-1], B[j-1])

            if (m + n) % 2 == 1:
                return max_of_left

            if i == m: min_of_right = B[j]
            elif j == n: min_of_right = A[i]
            else: min_of_right = min(A[i], B[j])

            return (max_of_left + min_of_right) / 2.0
```

### 5. Longest Palindromic Substring

没有套路。中间往外拓，平方级复杂度。

### 6. ZigZag Conversion
没有套路。按照ZigZag的字面意思，在s上做遍历。  
（其实还可以先算出每个值所在的位置，不过有点儿费草稿纸）
```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows==1 or (len(s)<numRows):
            return s
        res_list = [''] * numRows
        location, direction = 0, -1 # 假想开头前面也迭代过程中，所以初始是负号
        for i in s:
            res_list[location] = res_list[location] + i
            if location == 0 or location == numRows - 1:
                direction = -direction
            location += direction
        return ''.join(res_list)
```

### 7. Reverse Integer

没有套路。  
按位数颠倒一个整数。考虑负号和零。Python用字符串反转就行了。

```python
class Solution:
    def reverse(self, x: int) -> int:
        sign = 1 if x >= 0 else -1
        y=sign*int(str(sign*x)[::-1])
        if y>2**31-1 or y<-2**31:
            return 0
        return y
```

简洁的写法
```
class Solution:
    def reverse(self, x: int) -> int:
        sign = (x > 0) - (x < 0)
        r = int(str(sign*x)[::-1])
        return sign*r * (r < 2**31)
```

### 8. String to Integer (atoi)
实在无聊的题，就是各种边界条件。不用看了
### 9.
无聊

### 10. Regular Expression Matching
好题！  
套路：
1. 递归（不是很容易想出）
2. 递归的某些节点被反复调用。考虑存下来。（下面代码里面没有体现）

https://leetcode.com/problems/regular-expression-matching/solution/

```python

def isMatch(text, pattern):
    if not pattern:
        return not text

    first_match= text and pattern[0] in {text[0],'.'}
    if len(pattern)>=2 and pattern[1]=='*':
        return isMatch(text,pattern[2:]) or first_match and isMatch(text[1:],pattern)
    else:
        return first_match and isMatch(text[1:],pattern[1:])

```

需要再过一遍：DP也能很好的解这个题。

### 11. Container With Most Water

好题！
1. 像 2-sum 那个题目，看起来需要2次遍历，立即想到暂存历史计算，减少计算量。
2. 先看看哪些情况是“不可能”的，把它们排除出去。左挡板从左向右遍历，如果后一个比前一个还短，那么这个不可能是 candidate，右挡板同样的道理。
3. （到上面这一步已经可以写代码了，最坏平方级复杂度）还可以继续优化（这个看答案才想出来）。我们上一步把不可能的情况删掉后得到了递增的左挡板，和递增（从右往左看）的右挡板。如果左挡板低于右挡板，那么右挡板向左移动就不可能是 candidate，于是又排除了很多情况，变成 O(n) 复杂度。


```python
a=[1,8,6,2,5,4,8,3,7]
i,j=0,len(a)-1
weight=0
while i<j:
    weight=max(weight,(j-i)*min(a[i],a[j]))
    if a[i]<a[j]:
        i+=1
    else:
        j-=1

weight
```

### 12. Integer to Roman
无聊的题
```python
all_symbols = [
    ['', 'I', 'II', 'III', 'IV', 'V', 'VI', 'VII', 'VIII', 'IX']
    ,['', 'X', 'XX', 'XXX','XL', 'L', 'LX', 'LXX', 'LXXX', 'XC']
    , ['', 'C', 'CC', 'CCC', 'CD', 'D', 'DC', 'DCC', 'DCCC', 'CM']
    ,['', 'M', 'MM', 'MMM']
]

''.join([all_symbols[i][num//(10**i)%10] for i in range(3,-1,-1)])
```

### 13~15 无聊的题目

### 16. 3Sum Closest
n-Sum 两种思路：
1. 复用2sum，加一层遍历。2sum复杂度是n，3sum复杂度是O(n^2)
2. 2sum还有另一种解法，先排序，然后使用 `Container With Most Water` 的思路，找解复杂度是 O(n),但排序的复杂度是 O(nlgn)，所以2sum复杂度是 O(nlgn),3sum复杂度 O(n^2+nlgn)=O(n^2)
3. 所以2sum和3sum都可以用这个思路，复杂度一样
4. 但3Sum Closest显然不能用1了，但可以用2


```python

nums=[0,-4,1,-5]
target=0

nums.sort()


def sum2_cloeset(nums,n):
    i,j=0,len(nums)-1
    res=nums[i]+nums[j]
    diff=abs(res-n)
    while i<j:
        sum2=nums[i]+nums[j]
        if sum2==n:
            return sum2
        diff_tmp=abs(sum2-n)
        if diff_tmp<diff:
            diff=diff_tmp
            res=sum2
        if sum2>n:
            j-=1
        else:
            i+=1
    return res


# nums=[1,2,3,4,5]
# sum2_cloeset(nums,5)


diff=abs(sum(nums[:3])-target)
res=sum(nums[:3])
for i in range(len(nums)-2):
    tmp=sum2_cloeset(nums[i+1:],target-nums[i])
    if abs(tmp+nums[i]-target)<diff:
        print(tmp,nums[i])
        res=tmp+nums[i]
        diff=abs(tmp+nums[i]-target)

res
```

### 17. Letter Combinations of a Phone Number

基础题，可以以此题为例子，把几种解法练熟了，甚至背下来。
1. 利用 itertools
```python
import itertools
def letterCombinations(self, digits: str) -> List[str]:
    if len(digits)==0:return []
    all_char=['','','abc','edf','ghi','jkl','mno','pqrs','tuv','wxyz']
    candidate=[list(all_char[int(button)]) for button in digits]
    return [''.join(i) for i in  itertools.product(*candidate)]
```
2. 递归。这个递归方法要求返回所有遍历到叶节点的路径。而不是成功访问1个叶节点就返回。
```python
all_char=['','','abc','edf','ghi','jkl','mno','pqrs','tuv','wxyz']

def letter_comb(digits):
    if len(digits)==0:
        return []
    if len(digits)==1:
        return list(all_char[int(digits[0])])
    prev=letter_comb(digits[:-1])
    lst=all_char[int(digits[-1])]
    return [s+l for s in prev for l in lst]

letter_comb(digits)
```
3. 递归（从前往后），另外，如果不考虑空的特殊情况，代码还可以再省一些
```python
all_char=['','','abc','edf','ghi','jkl','mno','pqrs','tuv','wxyz']

def letter_comb(digits):
    if len(digits)==0:
        return ['']
    prev=all_char[int(digits[0])]
    lst=letter_comb(digits[1:])
    return [p+l for p in prev for l in lst]
```


### 18. 4Sum

nSUM都有了，用递归
```python

def find_2sum(nums,target):
    i,j=0,len(nums)-1
    res=[]
    while i<j:
        sum2=nums[i]+nums[j]
        if sum2==target:
            res.append([nums[i],nums[j]])
            i+=1
        elif sum2>target:
            j-=1
        else:
            i+=1
    return res


def find_Nsum(nums,target,N):
    if N==2:
        return find_2sum(nums,target)
    res=[]
    for i in range(len(nums)-N+1):
        tmp=find_Nsum(nums[i+1:],target-nums[i],N-1)
        if len(tmp)>0:
            res.extend([[nums[i]]+t for t in tmp])
    return res


nums=[1,0,-1,0,-2,2]
target=0
nums.sort()
find_Nsum(nums,target,N=4)
```

### 19. Remove Nth Node From End of List

套路：
1. 凡是链表上的遍历问题，一律先想想能否使用单指针/双指针/多指针
2. 如果处理的链表是不含空的头节点那种，一律加上空的头节点。这样往往可以减少很多处理边界条件的代码量。

```python
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy=ListNode('')
        dummy.next=head
        curr1=dummy
        for i in range(n):
            curr1=curr1.next
        curr2=dummy
        while curr1.next:
            curr1=curr1.next
            curr2=curr2.next
        curr2.next=curr2.next.next
        return dummy.next
```

### 20. Valid Parentheses

用堆栈做，没什么难度
```
def isValid(self, s: str) -> bool:
    stack=list()
    pairs={'(':')','[':']','{':'}'}
    pairs2={')':'(',']':'[','}':'{'}
    for i in s:
        if i in pairs:
            stack.append(i)
        elif i in pairs2:
            if len(stack)==0:
                return False
            elif stack[-1]==pairs2[i]:
                stack.pop()
            else:
                return False
    return len(stack)==0
```






































---------------------------

```
class Solution(object):
    def isMatch(self, text, pattern):
        memo = {}
        def dp(i, j):
            print(memo)
            if (i, j) not in memo:
                if j == len(pattern):
                    ans = i == len(text)
                else:
                    first_match = i < len(text) and pattern[j] in {text[i], '.'}
                    if j+1 < len(pattern) and pattern[j+1] == '*':
                        ans = dp(i, j+2) or first_match and dp(i+1, j)
                    else:
                        ans = first_match and dp(i+1, j+1)

                memo[i, j] = ans
            return memo[i, j]

        return dp(0, 0)

```

### 20. Valid Parentheses


这也是个好题目，用stack做。


```python
def isValid(s):
    stack=list()
    pairs={'(':')','[':']','{':'}'}
    pairs2={')':'(',']':'[','}':'{'}
    for i in s:
        if i in pairs:
            stack.append(i)
        elif i in pairs2:
            if len(stack)==0:
                return False
            if stack[-1]==pairs2[i]:
                stack.pop()
            else:
                return False
    return len(stack)==0
```





----------------------------

## 趣题
### 浇水
https://leetcode.com/problems/container-with-most-water/

Brute Force 可以做，但考虑一些循环不需要做。。。

### 下雨

https://leetcode.com/problems/trapping-rain-water/submissions/

难度升级款，想出来后就很容易
```
class Solution:
    def trap(self, height: list) -> int:
        if not height:
            return 0
        max_height=max(height)
        len_height=len(height)
        cum_max_left=[0]*len_height
        cum_max_right=[0]*len_height
        max_left,max_right=0,0
        for i in range(len_height):
            max_left=max(max_left,height[i])
            cum_max_left[i]=max_left
            j=len_height-i-1
            max_right=max(max_right,height[j])
            cum_max_right[j]=max_right
        total_area=0
        for i in range(len_height):
            total_area+=(min(cum_max_left[i],cum_max_right[i])-height[i])
        return total_area
```


衍生两道题

https://leetcode.com/problems/product-of-array-except-self/

这个题，第一思路就是乘起来，然后除以各自的项。缺点是题目不让用除法，况且对0的处理略繁琐  
也可以参照上面下雨那题的思路  
```
class Solution:
    def productExceptSelf(self, nums):
        prod_left_list=nums.copy()
        prod_right_list=nums.copy()
        prod_left,prod_right=1,1
        len_nums=len(nums)
        for idx in range(len_nums):
            prod_left_list[idx]=prod_left
            prod_left*=nums[idx]
            j=len_nums-idx-1
            prod_right_list[j]=prod_right
            prod_right*=nums[j]
        return [prod_left_list[i]*prod_right_list[i] for i in range(len_nums)]
```
时间和空间消耗和除法解法相似。再改进就是，其实不需要right这个list。不过这样可读性差点儿。

衍生题目：https://leetcode.com/problems/maximum-product-subarray/
有点难想到，太漂亮了
```python
class Solution:
    def maxProduct(self, nums) -> int:
        imax=imin=r=nums[0]
        for i in nums[1:]:
            if i<0:
                imax,imin=imin,imax
            imax=max(i,imax*i)
            imin=min(i,imin*i)
            r=max(r,imax)
        return r
```

还有个漂亮的解
https://leetcode.com/problems/maximum-product-subarray/discuss/183483/In-Python-it-can-be-more-concise-PythonC%2B%2BJava
```Python
def maxProduct(self, A):
    B = A[::-1]
    for i in range(1, len(A)):
        A[i] *= A[i - 1] or 1
        B[i] *= B[i - 1] or 1
    return max(A + B)
```




这个题有3个衍生题目
https://leetcode.com/problems/maximum-subarray/
这是个经典题目，找到一个漂亮的答案 https://leetcode.com/problems/maximum-subarray/discuss/20396/Easy-Python-Way
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        for i in range(1, len(nums)):
            if nums[i-1] > 0:
                nums[i] += nums[i-1]
        return max(nums)
```

衍生题目
https://leetcode.com/problems/longest-turbulent-subarray/


https://leetcode.com/problems/subarray-product-less-than-k/


https://leetcode.com/problems/trapping-rain-water-ii/

##递归
https://leetcode.com/problems/house-robber/

我写的递归，超时，但这个就好用，有空要研究一下递归和这种写法的转化关系，以及为啥递归会超时
```
class Solution:
    def rob(self, nums):
        last, now = 0, 0
        for i in nums: last, now = now, max(last + i, now)
        return now
```





---------------------------

```
class Solution(object):
    def isMatch(self, text, pattern):
        memo = {}
        def dp(i, j):
            print(memo)
            if (i, j) not in memo:
                if j == len(pattern):
                    ans = i == len(text)
                else:
                    first_match = i < len(text) and pattern[j] in {text[i], '.'}
                    if j+1 < len(pattern) and pattern[j+1] == '*':
                        ans = dp(i, j+2) or first_match and dp(i+1, j)
                    else:
                        ans = first_match and dp(i+1, j+1)

                memo[i, j] = ans
            return memo[i, j]

        return dp(0, 0)

```
