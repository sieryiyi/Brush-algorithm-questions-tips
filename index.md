## 6.17

开始尝试搭建个人博客

此时力扣：90/174/7，通过率58%

此项目用来记录刷算法所遇到的问题、知识点

Markdone使用方法如下↓↓↓↓↓

https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

## 排序算法

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

### 归并排序（分治思想）

时间复杂度O(nlogn)

空间复杂度，O(n)的自顶向下，O(1)的自底向上

自顶向下：先全部分割完，直到只有一个节点，再开始合并

自底向上：

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:

        # 归并排序，最适合链表的排序
        # 时间复杂度O(nlogn)
        # 自顶向下，自底向上两种，后者省空间
        # 需要俩函数，一个分割，一个排序
        

        # 本次题解抄官方的，先写自顶向下

        def backsort(head,tail):
            if not head:
                return head
            if head.next==tail: #tail的定义是取不到
                head.next=None  # 说明此时只有一个节点，不需要拆分
                return head
            
            slow,fast=head,head
            while fast!=tail:
                slow=slow.next
                fast=fast.next
                if fast!=tail: # 如果此时已经到了结尾，则不再移动两步
                    fast=fast.next
            mid = slow # 如果是偶数链表，则所在位置为中间两个的右边

            return merge(backsort(head,mid),backsort(mid,tail)) #这一步会不断跳到分割里，直到分割到只有一个点，开始合并

        def merge(l1,l2): # 合并两个有序链表，保证其仍然有序

            dummy=ListNode()
            temp,temp1,temp2=dummy,l1,l2
            # temp是最终存放合并后的链表的，在此处产生了空间复杂度

            while temp1 and temp2:
                if temp1.val>=temp2.val:
                    temp.next=temp2
                    temp2=temp2.next
                elif temp1.val<temp2.val:
                    temp.next=temp1
                    temp1=temp1.next
                temp=temp.next
            if temp1:
                temp.next=temp1
            elif temp2:
                temp.next=temp2

            return dummy.next

        return backsort(head,None)
        
```
