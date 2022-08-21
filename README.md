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


### floyd算法-------解决传递闭包、求最短路径问题

https://blog.csdn.net/beautiful_pain/article/details/105565509?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166096390416781667824721%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166096390416781667824721&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-2-105565509-null-null.142^v42^new_blog_pos_by_title,185^v2^control&utm_term=floyd%E9%97%AD%E5%8C%85%E4%BC%A0%E9%80%92&spm=1018.2226.3001.4187

胜负传递问题

定义：
```
dis[a][b]=1表示a比b强，等于0表示胜负不明，dis[a][b]=0=dis[b][a]表示根据已知信息，无法判断胜负

三维dp：f[k][x][y]表示只允许经过节点1-k的情况下，节点x到节点y的最短距离

f[k][x][y]=min(f[k-1][x][y],f[k-1][x][k]+f[k-1][k][y])

```
https://blog.csdn.net/jeffleo/article/details/53349825?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166096518616781683911152%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166096518616781683911152&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-53349825-null-null.142^v42^new_blog_pos_by_title,185^v2^control&utm_term=floyd%E7%AE%97%E6%B3%95%E6%B1%82%E6%9C%80%E7%9F%AD%E8%B7%AF%E5%BE%84&spm=1018.2226.3001.4187
