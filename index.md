## 6.17

开始尝试搭建个人博客

此时力扣：90/174/7，通过率58%

此项目用来记录刷算法所遇到的问题、知识点

Markdone使用方法如下↓↓↓↓↓

https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

## 排序算法

### 快速排序（分治思想）

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

时间复杂度：O(nlogn)

需要两个函数：建堆+调整

调库的话：

```
import heapq

a=[3,1,2]
a=heapq.heapify(a)
heapq.heappush(a,val)
x=heapq.heappop()

```

手动实现：

```

def findKthLargest(self, nums: List[int], k: int) -> int:

        # 手动实现堆
            # 对于「升序排列」数组，需用到大根堆；
            # 对于「降序排列」数组，则需用到小根堆。
        # 建堆
        # 调整和交换
        
        def heap_heapify(arr,i,end) : # 调整arr中以end为结尾的一段为堆，其中，i是当前需要调整的根节点，需要调整的还有全部i以下的节点
            j=i*2+1 # 左节点
            while j<=end: # 存在子节点
                if j+1<=end and arr[j+1]>arr[j]: # 存在右节点
                    j+=1
                if arr[i]<arr[j]:
                    arr[i],arr[j]=arr[j],arr[i]
                    i=j # 交换了后，i重置为交换后的子节点，继续调整子节点及以下
                    j=2*i+1
                else:
                    break

        end=len(nums)-1
        for i in range((end-1)//2,-1,-1):
            heap_heapify(nums,i,end)

        # 开始找第k大的值

        for i in range(k):
            nums[0],nums[-i-1]=nums[-i-1],nums[0]  # 改变顺序后，需要重新调整堆
            heap_heapify(nums,0,end-i-1)
        return nums[-k]

```

### 归并排序（分治思想）（稳定算法）

时间复杂度O(nlogn)

空间复杂度，O(nlogn)的自顶向下，O(1)的自底向上-------------------递归调用的栈空间引起了空间复杂度

自顶向下：先全部分割完，直到只有一个节点停止，再开始合并

自底向上：求出链表长度，直接一次性分割成若干个长度为sublen=1的链表，进行合并，得到若干个长度为sublen=2的链表，再合并.......直到所有链表合并完毕

--------------------------------------

```
--------------------------自顶向下
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
        

        # 本次题解参照官方的，先写自顶向下

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
            # temp是最终存放合并后的链表的，在此处产生了空间复杂度？

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

自底向上：

```
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        def merge(l1,l2): # 合并两个有序链表，保证其仍然有序
            dummy=ListNode()
            temp,temp1,temp2=dummy,l1,l2
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

        if not head or not head.next: #特殊情况不用排序
            return head

        lenth=0  # 记录链表长度
        
        node=head
        while node:
            lenth+=1
            node=node.next
        

        sublenth=1
        dummy=ListNode(next=head)
        while sublenth<lenth: # 当有序数组的长度大于总长度时，说明全部排序完毕
            prev,curr=dummy,dummy.next

            while curr: # 说明没走到末尾
                head1=curr
                for i in range(1,sublenth):
                    if curr.next:
                        curr=curr.next
                    else:break  # 到么到了sublen退出，要么下一个为None，退出
                head2=curr.next
                curr.next=None

                curr=head2
                for i in range(1,sublenth):
                    if curr and curr.next: #这里的if curr是为了保证不存在上述那种为None退出的情况
                        curr=curr.next
                    else:
                        break
                succ=None
                if curr:
                    succ=curr.next # 保存剩下的链表
                    curr.next=None

                merge_l=merge(head1,head2)
                prev.next=merge_l  # 连接已经有序的部分

                while prev.next:
                    prev=prev.next # 移动到连接上一个有序部分后的末尾

                curr=succ
            sublenth*=2
        return dummy.next
```

### 冒泡排序

### 插入排序

### 桶排序（取决于对桶排序时的算法是否稳定）

剑指 Offer II 057. 值和下标之差都在给定的范围内 https://leetcode.cn/problems/7WqeDu/

思想：将现有的数字放在不同的桶中，当两数字位于同一桶中，则符合要求，如何算出分到哪个桶是重点！！全部遍历后（或遍历过程中），检查当前桶中的数是否满足要求，若不满足，检查当前桶和前后两个桶中的数是否满足要求，若不满足，继续遍历，直到结尾。

```
        def get_id(x):
            # 在0-t分一个？
            if x>=0:
                y=x//(t+1)
                return y
            else:
                return (x+1)//(t+1)-1

        n=len(nums)
        s=collections.defaultdict(list)
        for i in range(n):
            x=nums[i]
            id1=get_id(x)
            if i-k-1>=0:
                id2=get_id(nums[i-k-1])
                if id2 in s:
                    s.pop(id2)
            if id1 in s:
                return True
            else:
                s[id1]=x
            
            if id1-1 in s and s[id1]-s[id1-1]<=t:
                return True
            if id1+1 in s and s[id1+1]-s[id1]<=t:
                return True
        return False
```

此题是滑动窗口+桶排序进行解答，每一个桶中（非此题）如果有多个数字，同时有排序需求的话，可以用上面提到的快排、归并等O(nlogn)的排序算法进行排序，桶排序是否稳定、时间复杂度是多少，取决于在桶内使用的排序算法
