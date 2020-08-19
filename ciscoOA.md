# Cisco OA

### word search II 不同: 有方向以后 方向不能变

```python
def wordSearch(matrix,words):
    row = len(matrix)
    col = len(matrix[0])
    dict_t = {}
    for word in words:
        tmp = dict_t
        for char in word:
            if char not in tmp:
                tmp[char]= {}
            tmp = tmp[char]
        tmp["$"] = word
    result = []
    

    directions = {"up":(-1,0),
                    "down":(1,0),
                    "left":(0,-1),
                    "right":(0,1)
                    }    
    def dfs(direc,root,i,j):
        tmp =matrix[i][j]
        child = root[matrix[i][j]]
        matrix[i][j] = "#"
        
        if "$" in child:
            result.append(child["$"])
            child.pop("$")

        if not direc:
            for k,v in directions.items():
                x,y = v 
                
                if isValid(i+x,j+y,child):
                    
                    dfs(index,k,child,i+x,j+y)

        else:
            x,y = directions[direc]
            if isValid(i+x,j+y,child):
                dfs(direc,child,i+x,j+y)
        matrix[i][j] = tmp


    def isValid(i,j,child):
        
        return 0<=i<row and 0<=j<col and matrix[i][j] in child


    for i in range(row):
        for j in range(col):
            if matrix[i][j] in dict_t:
                dfs(False,dict_t,i,j)
                
    print(result)

```

### MaxSubarray LC 原题
```python
class LCMaxSubArray:
    def maxSubArray(self, nums) -> int:
            gmax = nums[0]
            prev = nums[0]
            for i in range(1,len(nums)):
                if prev <0:
                    curr = nums[i]
                else:
                    curr = nums[i]+ prev
                gmax = max(gmax,curr)
                prev = curr
                
            return gmax
            
            
    def dpv(self,nums):
        dp = [0]* len(nums)
        dp[0] = nums[0]
        gmax = nums[0]
        for i in range(1,len(nums)):
            if dp[i-1]<0:
                dp[i] = nums[i]
            else:
                dp[i] = dp[i-1] + nums[i] 
            
            gmax = max(gmax,dp[i])
            
        return gmax
```

### primes: 两种方法
``` python
def findPrimeON(arr):
    gmax = max(arr)
    prime=[]
    SPF= [None]* (gmax+1)
    isPrime= [True]*(gmax+1)
    for i in range(2,gmax+1):
        if isPrime[i] == True:
           prime.append(i)
           SPF[i] = i
        j = 0
        while j < len(prime) and i* prime[j]<gmax and prime[j]<= SPF[i]:
            isPrime[i*prime[j]]= False
            SPF[i* prime[j]] =prime[j]
            j+= 1
    print(prime)


def findPrime(arr):
    gmax = max(arr)
    primes=[True]* gmax
    for i in range(2,gmax):
        j = 2
        while j<gmax and i*j < gmax:
            primes[i*j] = False  
            j+= 1
```

### 找众数 平均数
平均数最后保留 4个小数
``` python
def modeMean(nums):
    count = [0]*(max(nums)+1)
    total = 0
    mode = 0
    for i in nums:
        count[i] += 1
        total += i
        if count[i] == mode:
            if i < mode:
                mode = i
        if count[i]>mode:
            mode = i
    avg = total/len(nums)
    print("{:.4f} {}".format(avg,mode))
```

### chocolate Jar 
lc robhouse 区别 jar有负数
```python
def nJar(nums):
    dp =[-float(inf)]* (len(nums)+1)
    dp[0] = 0
    dp[1] = max(0,nums[0])
    
    for i in range(2,len(nums)):
        dp[i] = max(dp[i-1],dp[i-2]+nums[i-1])

    
```
### count Bits
10 = 1010  output => 2
``` python
def countSetBits(n):
    count = 0
    while n:
        n &= (n-1)
        count += 1
    print(count)
```

### decode string 
LC 原题。 区别 数字 被{} 
``` python
def decodeString(s):
    #(ab(d){3}){2}
    
    sym = {"}":"{", ")":"(" }
    string = num = ""
    stack = []
    for char in s:
        if char == "}":
            prefix = stack.pop()
            string = prefix + int(num)* string
        elif char.isdigit():
            num += char
        elif char == "{":
            num = ""
        elif char == "(":
            stack.append(string)
            string = ""
        elif char == ")":
            pass
        elif char.isalpha():
            string += char

    print(string)
```

### decodeWay 
leetcode 原题。区别 A= 0 .... Z = 25
```python
def decodeWay(s):
    dp = [0]* (len(s)+1)
    dp[0] = 1
    dp[1] = 1
    for i in range(1,len(s)):
        if 10<=int(s[i-1]+s[i])<=25:
            dp[i+1] = dp[i-1]+dp[i]
        else:
            dp[i+1] = dp[i]
    
    print(dp[-1])
```

### Queen Attack

queen 攻击范围 : 米
``` python
def queenAttack(qx, qy, ox, oy):
    if qx == ox:
        return True
    
    if qy == oy:
        return True
    
    if abs(qx-ox) == abs(qy-oy):
        return True
    
    return False
   
 ```
### 反转矩阵 "M X N" , "N X N"
``` python
#N X N matrix
def rotateImageNXN(matrix):
   
        
    def dfs(index, matrix, row):
        if row <1:
            return
        
        for i in range(row-1):
            first = matrix[index][i+index]
            matrix[index][i+index] = matrix[index+row-1-i][index]
            matrix[index+row-1-i][index] = matrix[row-1+index][row-1+index-i]
            matrix[row-1+index][row-1+index-i] = matrix[index+i][row-1+index]
            matrix[index+i][row-1+index] = first
            
        dfs(index+1,matrix,row-2)
    dfs(0,matrix,len(matrix))

# M X N matrix
def rotateImageMN(matrix):
    m = len(matrix)
    n = len(matrix[0])
    result = [[0 for _ in range(m)]for _ in range(n)]
    for i in range(n):
        for j in range(m):
            result[i][j] =matrix[j][i]

    for r in result:
        print(r)
    print("==========")
    for i in range(n):
        result[i].reverse()

    for r in result:
        print(r)
```
### Max Difference

``` python
# input = [1,2,3,4,100,3,9]
#  max diff = 1,100

def maxDiff(arr, arr_size): 
    max_diff = arr[1] - arr[0] 
    min_element = arr[0] 
      
    for i in range( 1, arr_size ): 
        if (arr[i] - min_element > max_diff): 
            max_diff = arr[i] - min_element 
      
        if (arr[i] < min_element): 
            min_element = arr[i] 
    return max_diff 
```

### IP address
``` python

def validIPAddress(IP) :

    def validate_IPv4( IP) -> str:
        nums = IP.split('.')
        for x in nums:
            # Validate integer in range (0, 255):
            # 1. length of chunk is between 1 and 3
            if len(x) == 0 or len(x) > 3:
                return "Neither"
            # 2. no extra leading zeros
            # 3. only digits are allowed
            # 4. less than 255
            if x[0] == '0' and len(x) != 1 or not x.isdigit() or int(x) > 255:
                return "Neither"
        return "IPv4"
    
    def validate_IPv6( IP) :
        nums = IP.split(':')
        
        hexdigits = '0123456789abcdefABCDEF'
        
        for x in nums:
            # Validate hexadecimal in range (0, 2**16):
            # 1. at least one and not more than 4 hexdigits in one chunk
            # 2. only hexdigits are allowed: 0-9, a-f, A-F
            if len(x) == 0 or len(x) > 4 or not all(c in hexdigits for c in x):
                return "Neither"
        return "IPv6"

    if IP.count('.') == 3:
        return validate_IPv4(IP)
    elif IP.count(':') == 7:
        return validate_IPv6(IP)
    else:
        return "Neither"
```