# Brush-algorithm-questions-tips
用来记录自己在力扣/牛客网刷算法遇到的好的思路、踩坑的点、常用函数等


### 二分图最大匹配问题（匈牙利算法）

HJ28 素数伴侣

https://www.nowcoder.com/practice/b9eae162e02f4f928eac37d7699b352e?tpId=37&rp=1&ru=%2Fexam%2Foj%2Fta&qru=%2Fexam%2Foj%2Fta&sourceUrl=%2Fexam%2Foj%2Fta%3FjudgeStatus%3D3%26page%3D1%26pageSize%3D50%26search%3D%26tpId%3D37%26type%3D37&difficulty=&judgeStatus=3&tags=&title=&gioEnter=menu

```
import collections
import functools
n=int(input())
nn=list(map(int,input().split()))
# nn.sort()
ans=0
d=collections.defaultdict(int)

@functools.lru_cache(None)
def check(num): #判断是否是素数
    if num==2:
        return True
    for i in range(2,int(num**0.5) + 2):
        if(num % i == 0):
            return False
    return True
def group_list(num_list):
    """分奇偶：因为素数一定是奇数+偶数"""
    odds, evens = [], []
    for num in num_list:
        if num % 2 == 0:
            evens.append(num)
        else:
            odds.append(num)
    return odds, evens

odds,evens=group_list(nn)

for i in evens:
    d[i]=0 # 未被匹配状态
    
def match(odd,evens,d,visited):
# for i,odd in enumerate(odds):
    for j,even in enumerate(evens):
        if check(odd+even) and not visited[j]:  # 注意这里！！！！！要标记是否已经找过！！
            visited[j]=True
            if d[even]==0 or match(d[even],evens,d,visited):
                d[even]=odd # 直接标记配对
                return True
    return False 

for odd in odds:
    visited=[False]*len(evens)
    if match(odd,evens,d,visited):
        ans+=1
print(ans)
            

```
