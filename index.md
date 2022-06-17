### 创建于6.17

开始尝试搭建个人博客

此项目用来记录刷算法所遇到的问题、知识点

Markdone使用方法如下↓↓↓↓↓

[(https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)]

### 快速排序

剑指 Offer II 076. 数组中的第 k 大的数字  https://leetcode.cn/problems/xx4gT2/

O(nlogn)~O(n2)

思想：每一次设置一个锚点tag，两个左右哨兵left,right（右哨兵先动），right遇到<tag的数停下，将这个数字赋给left所在位置，left开始动，遇到>tag时停下，赋值给right所在位置，直到二者相遇，此时，将保存在tag中的数赋值给交叉点，此时，在交叉点左边都比tag小，交叉点右边都比tag大，再对左右分别再次快排，即分治
 

```
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        def quicksort(begin,end,arr,k):   # 此处手动实现快速排序
            if end<begin:  # 退出条件
                return 
            s=random.randint(begin,end)   # 此处是tips！！！防止特殊实例，如需要正序数组，但原数组正好倒序
            arr[s],arr[begin]=arr[begin],arr[s]
            tag=arr[begin]
            i,j=begin,end
            while i!=j:
                while j>i and arr[j]>=tag:
                    j-=1
                arr[i]=arr[j]  # 注意！！！！
                while j>i and arr[i]<=tag:
                    i+=1
                arr[j]=arr[i]  # 注意！！！

            arr[i]=tag
            
            if i==k:  # 此处是快速选择思想
                return arr[i]
            elif i<k:
                x=quicksort(i+1,end,arr,k)
            else:
                x=quicksort(begin,i-1,arr,k)
            return x

        return quicksort(0,len(nums)-1,nums,len(nums)-k)
```


### 堆排序
