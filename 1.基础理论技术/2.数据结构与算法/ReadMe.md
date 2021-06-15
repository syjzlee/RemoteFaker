<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [常用算法](#%E5%B8%B8%E7%94%A8%E7%AE%97%E6%B3%95)
  - [排序算法](#%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95)
  - [链表算法](#%E9%93%BE%E8%A1%A8%E7%AE%97%E6%B3%95)
  - [栈和队列算法](#%E6%A0%88%E5%92%8C%E9%98%9F%E5%88%97%E7%AE%97%E6%B3%95)
  - [二叉树算法](#%E4%BA%8C%E5%8F%89%E6%A0%91%E7%AE%97%E6%B3%95)
    - [四种遍历方式](#%E5%9B%9B%E7%A7%8D%E9%81%8D%E5%8E%86%E6%96%B9%E5%BC%8F)
    - [二叉树中的一些重要属性](#%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%AD%E7%9A%84%E4%B8%80%E4%BA%9B%E9%87%8D%E8%A6%81%E5%B1%9E%E6%80%A7)
  - [字符串算法](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%AE%97%E6%B3%95)
  - [哈希算法](#%E5%93%88%E5%B8%8C%E7%AE%97%E6%B3%95)
  - [日常算法思想](#%E6%97%A5%E5%B8%B8%E7%AE%97%E6%B3%95%E6%80%9D%E6%83%B3)
    - [回溯](#%E5%9B%9E%E6%BA%AF)
    - [分而治之](#%E5%88%86%E8%80%8C%E6%B2%BB%E4%B9%8B)
    - [动态规划](#%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

#   数据结构
* 主要参考
  - [problem-solving-with-algorithms-and-data-structure-using-python 中文版](https://facert.gitbooks.io/python-data-structure-cn/)
  - 五分钟算法公众号
* 常见数据结构[戳这里](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=2247485737&idx=1&sn=82fa3f103ef485a8ce28c5e1c5e012ea&chksm=fa0e66a8cd79efbeb88cb175fcc21a71964d975a47d3c87a8335506466937e811a066f8a1889&mpshare=1&scene=1&srcid=&sharer_sharetime=1564711256674&sharer_shareid=d8d88a642b42a12a54cd09298c974b8c#rd)
#   常用算法
##  排序算法
* 快排，堆排，归并排序详细原理参考这篇[这或许是东半球分析十大排序算法最好的一篇文章](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=2247485556&idx=1&sn=344738dd74b211e091f8f3477bdf91ee&chksm=fa0e67f5cd79eee3139d4667f3b94fa9618067efc45a797b69b41105a7f313654d0e86949607&scene=21#wechat_redirect)，下面我将用 Python 快速实现出来。话不多说，Show Me Code!
* 冒泡排序
```python
def bubble_sort(alist):
    for i in range(len(alist)-1):
        for j in range(len(alist)-1-i):
            if alist[j]>alist[j+1]:
                alist[j],alist[j+1]=alist[j+1],alist[j]
alist=[54,26,93,17,77,31,44,55,20]
bubble_sort(alist)
print(alist)
```
* 选择排序
```python
def select_sort(alist):
    for i in range(len(alist)-1):
        min_index=i
        for j in range(i+1,len(alist)):
            if alist[j]<alist[min_index]:
                min_index=j
        if i!=min_index:
            alist[i],alist[min_index]=alist[min_index],alist[i]
alist=[54,26,93,17,77,31,44,55,20]
select_sort(alist)
print(alist)
```
* 插入排序
```python
def insert_sort(alist):
    for i in range(1,len(alist)):
        for j in range(i,0,-1):
            if alist[j]<alist[j-1]:
                alist[j],alist[j-1]=alist[j-1],alist[j]
            else:
                break
alist=[54,26,93,17,77,31,44,55,20]
insert_sort(alist)
print(alist)
```
* 希尔排序
```python
# 变换步长（增量）的插入排序->(n ~ n^2,不稳定)
def shell_sort(alist):
    gap=len(alist)//2
    while gap>=1:
        for i in range(gap,len(alist)):
            j=i
            while(j-gap)>=0:
                if alist[j]<alist[j-gap]:
                    alist[j],alist[j-gap]=alist[j-gap],alist[j]
                    j-=gap
                else:
                    break
        gap//=2
alist=[54,26,93,17,77,31,44,55,20]
shell_sort(alist)
print(alist)
```
* 归并排序
```python
#分而治之，递归分解/递归合并 （nlogn）
def merge_sort(alist):
    if len(alist)<=1:
        return alist
    mid = len(alist)//2
    left_alist = merge_sort(alist[:mid])
    right_alist = merge_sort(alist[mid:])
    return merge(left_alist,right_alist)
def merge(left_list,right_list):
    result=[]
    j=0
    i=0
    while i<len(left_list) and j < len(right_list):
        if left_list[i]<=right_list[j]:
            result.append(left_list[i])
            i+=1
        else:
            result.append(right_list[j])
            j+=1
    result+=left_list[i:]
    result+=right_list[j:]
    return result
alist=[54,26,93,17,77,31,44,55,20]
print(merge_sort(alist))
```
* 快速排序
```python
# 快速排序使用分而治之来获得与归并排序相同的优点，而不使用额外的存储。
# 有可能列表不能被分成两半。当这种情况发生时，性能降低
def quick_sort(alist, start, end):
    if start>= end:
        return
    #基准
    mark = alist[start]
    left = start
    right = end
    while left < right:
        while left<right  and alist[right]>mark:
            right-=1
        alist[left]=alist[right]
        while left<right and alist[left]<mark:
            left+=1
        alist[right]=alist[left]
    alist[right]=mark
    quick_sort(alist,start,left-1)
    quick_sort(alist,left+1,end)
alist=[54,26,93,17,77,31,44,55,20]
quick_sort(alist,0,len(alist)-1)
print(alist)
```
##  [链表算法](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=2247484830&idx=1&sn=9d24fc787da4b49b82ac01c7f8de257b&chksm=fa0e6a1fcd79e309a2e7f3e09ec9913a55f1c077287c907f13528578b7785831a2effb3104e0&scene=21#wechat_redirect)
* 输出/删除 单链表倒数第 K 个节点
```python
# leetcode 19：快慢双指针
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        second = fist = head
        for i in range(n):  # 第一个指针先走 n 步
            fist = fist.next
 
        if fist == None:  # 如果现在第一个指针已经到头了，那么第一个结点就是要删除的结点。
            return second.next
 
        while fist.next:  # 然后同时走，直到第一个指针走到头
            fist = fist.next
            second = second.next
        second.next = second.next.next  # 删除相应的结点
        return head
```
* 有序链表合并
```python
# leetcode 21
# 解题思路：合并后的链表仍然是有序的，可以同时遍历两个链表，
# 每次选取两个链表中较小值的节点，依次连接起来，就能得到最终的链表
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1 or not l2:
            return l1 or l2
        head=cur=ListNode(0)
        while l1 and l2:
            if l1.val<l2.val:
                cur.next=l1
                l1=l1.next
            else:
                cur.next=l2
                l2=l2.next
            cur=cur.next
        cur.next=l1 or l2
        return head.next
```
* 链表有环问题
```python
# leetcode 142：判断链表是否有环/定位环入口/环长度，快慢双指针
    class Solution(object):
    def detectCycle(self, head):
        if head is None:
            return
        slow=fast=head
        has_cycle=False
        while fast and fast.next:
            slow=slow.next
            fast=fast.next.next
            if slow==fast:
                has_cycle=True
                break
        if has_cycle:
            slow_p=head
            fast_p=fast
            while slow_p !=fast_p:
                fast_p=fast_P.next
                slow_p=slow_p.next
            return slow_p
        return
        # 环长度
        slow=slow.next
        fast=fast.next.next
        length=1
        while slow!=fast:
            slow=slow.next
            fast=fast.next.next
            length+=1
        return length
```
* 使用链表实现大数加法
```python
# leetcode 2
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        flag=0
        root=n=ListNode(0)
        while l1 or l2 or flag:
            v1=v2=0
            if l1:
                v1=l1.val
                l1=l1.next
            if l2:
                v2=l2.val
                l2=l2.next
            flag,val=divmod(v1+v2+flag,10)
            n.next=ListNode(val)
            n=n.next
        return root.next  
```
* 删除链表中节点，要求时间复杂度为O(1)
```python
# leetcode 237
class Solution:
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        next_node=node.next
        next_nextnode=next_node.next
        node.val=next_node.val
        node.next=next_nextnode
```
* 从尾到头打印链表
```python
# 根据栈的特性
class Solution:
    def printListFromTailToHead(self, listNode):
        if listNode is None:
            return []
        sta=list()
        res=list()
        while listNode:
            sta.append(listNode.val)
            listNode=listNode.next
        while sta:
            res.append(sta.pop())
        return res
# 递归
class Solution:
    def printListFromTailToHead(self, listNode):
        if listNode is None:
            return []
        return self.printListFromTailToHead(listNode.next)+[listNode.val]
```
* 反转链表
```python
# leetcode 206
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        pre=None
        # 不断取出和向后移动头节点,并将头节点连接到新头节点后面
        while head:
            next_node=head.next
            head.next=pre
            pre=head
            head=next_node
        return pre

```
* LRU缓存机制
```python
# leetcode 146:
# 字典（哈希）+双端链表
class Node(object): 
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.next = None
        self.prev = None
        
class LRUCache(object):
 
    def __init__(self, capacity):
        self.dic = {}
        self.capacity = capacity
        self.dummy_head = Node(0, 0)
        self.dummy_tail = Node(0, 0)
        self.dummy_head.next = self.dummy_tail
        self.dummy_tail.prev = self.dummy_head
 
    def get(self, key):
        if key not in self.dic:
            return -1
        node = self.dic[key]
        self.remove(node)
        self.append(node)
        return node.val
 
    def put(self, key, value):
        if key in self.dic:
            self.remove(self.dic[key])
        node = Node(key, value)
        self.append(node)
        self.dic[key] = node
 
        if len(self.dic) > self.capacity:
            head = self.dummy_head.next
            self.remove(head)
            del self.dic[head.key]
 
    def append(self, node):
        tail = self.dummy_tail.prev
        tail.next = node
        node.prev = tail
        self.dummy_tail.prev = node
        node.next = self.dummy_tail
 
    def remove(self, node):
        prev = node.prev
        next = node.next
        prev.next = next
        next.prev = prev
```
##  栈和队列算法
* [以下几个算法原理详解](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=2247484846&idx=2&sn=e508da06e9f7a0b3d00db5415d7ce622&chksm=fa0e6a2fcd79e3397fe8083f9493ae639f47c9448ac2a0026026494d098c47ecc6e08f1f8e28&scene=21#wechat_redirect)
* 有效的括号
```python
# leetcode 20
class Solution:
    def isValid(self, s: str) -> bool:
        chars={'(':')','[':']','{':'}'}
        stack=[]
        for i in s:
            if i in chars:
                stack.append(i)
            else:
                if not stack or chars[stack.pop()]!=i:
                    return False
        return not stack
```
* 用两个栈实现队列
```python
# leetcode 232: 一个栈作为缓存区
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.stack1=[]
        self.stack2=[]
        

    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        self.stack1.append(x)
        

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        if len(self.stack2)==0:
            while(self.stack1):
                self.stack2.append(self.stack1.pop())
        return self.stack2.pop()

    def peek(self) -> int:
        """
        Get the front element.
        """
        if len(self.stack2)==0:
            while(self.stack1):
                self.stack2.append(self.stack1.pop())
        return self.stack2[-1]

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        return not self.stack1 and not self.stack2
```
* 栈的压入、弹出序列
```python
# leetcode 946:借用一个辅助的栈tmp，遍历压栈顺序
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        tmp=[]
        while pushed:
            tmp.append(pushed.pop(0))
            while tmp and tmp[-1]==popped[0]:
                popped.pop(0)
                tmp.pop()
        return not tmp
```
* 包含 min 函数的栈
```python
# leetcode 155：实际上就是实现最小栈（重点是连续弹出最小值时的问题）
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack=[]
        self.minstack=[]

    def push(self, x: int) -> None:
        self.stack.append(x)
        if self.minstack:
            self.minstack.append(x)
        else:
            if x<self.minstack[-1]:
                self.minstack.append(x)
            else:
                self.minstack.append(self.minstack[-1])

    def pop(self) -> None:
        self.stack.pop()
        self.minstack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.minstack[-1]
```
* 验证栈序列
```python
# leetcode 946
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        tmp=[]
        while pushed:
            tmp.append(pushed.pop(0))
            while tmp and tmp[-1]==popped[0]:
                popped.pop(0)
                tmp.pop()
        return not tmp
```
* 约瑟夫环（双端队列实现）
```python
from pythonds.basic.queue import Queue
def hotPotato(namelist,num):
    simple_queue = Queue()
    for name in namelist:
        simple_queue.enqueue(name)
    while simple_queue.size()>1:
        for i in range(num):
            simple_queue.enqueue(simple_queue.dequeue())
        simple_queue.dequeue()
    return simple_queue.dequeue()

print(hotPotato(["Bill","David","Susan","Jane","Kent","Brad"],2))
```
* 合并K个排序链表（我刚开始用的暴力解法2333）
    * 这个[解析](https://leetcode-cn.com/problems/merge-k-sorted-lists/solution/he-bing-kge-pai-xu-lian-biao-by-leetcode/)不错，我直接贴了其中采用最优队列的方式。
```python
# leetcode 23
from Queue import PriorityQueue
class Solution(object):
    def mergeKLists(self, lists):
        head = point = ListNode(0)
        q = PriorityQueue()
        for l in lists:
            if l:
                q.put((l.val, l))
        while not q.empty():
            val, node = q.get()
            point.next = ListNode(val)
            point = point.next
            node = node.next
            if node:
                q.put((node.val, node))
        return head.next
```
##  二叉树算法
### 四种遍历方式
* 二叉树的四种遍历方式分别是：前序、中序、后序和层次。它们的时间复杂度都是O(n)，其中n是树中节点个数，因为每一个节点在递归的过程中，只访问了一次。
* 三种深度优先遍历方法的空间复杂度是O(h)，其中h是二叉树的深度，额外空间是函数递归的调用栈产生的，而不是显示的额外变量。`空间复杂度，通常是指完成算法所用的辅助空间的复杂度，而不用管算法前置的空间复杂度。比如在树的遍历算法中，整棵树肯定要占O(n)的空间，n是树中节点的个数，这部分空间是“平凡”的，即肯定存在的，我们不讨论它。`
* 层次遍历的空间复杂度是O(w)，其中w是二叉树的宽度（拥有最多节点的层的节点数）。
![树的遍历.png](https://i.loli.net/2019/06/27/5d142fcec291f76765.png)
* 前序遍历
```python
# 递归解法
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        ret=[]
        if root:
            ret=ret+[root.val]
            ret=ret+self.preorderTraversal(root.left)
            ret=ret+self.preorderTraversal(root.right)
        return ret
# 迭代算法思路：使用栈的思想，从根节点开始以此使用ret添加根节点值，stack添加右节点，
# curr=左节点，如果左节点为None，则获取其上一个右节点（一直输出根节点，添加其右节点，
# 遍历左节点，右节点的输出顺序为从下往上）
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        ret=[]
        if root==None:
            return ret
        stack=[]
        curr=root
        while curr or stack:
            if curr:
                ret.append(curr.val)
                stack.append(curr.right)
                curr=curr.left
            else:
                curr=stack.pop()
        return ret
```
* 中序遍历
```python
# 递归解法同上
# 迭代解法
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res=[]
        if root==None:
            return res
        stack=[]  #添加根节点
        curr=root
        while curr or stack:
            if curr:
                stack.append(curr)
                curr=curr.left
            else:
                curr=stack.pop()
                res.append(curr.val)
                curr=curr.right
        return res
```
* 后序遍历
```python
# 递归解法同上
# 迭代算法思路：后序遍历方式为：左右中，将其进行反转 中右左，
# 那么我们可以实现一个中右左，其原理与前序遍历一样
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        ret=[]
        if root==None:
            return ret
        stack=[]
        curr=root
        while curr or stack:
            if curr:
                ret.append(curr.val)
                stack.append(curr.left)
                curr=curr.right
            else:
                curr=stack.pop()
        return ret[::-1]
```
* 层次遍历
```
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        level=[root]
        ret=[]
        if root==None:
            return []
        while level:
            ret.append([i.val for i in level])
            level=[kid for node in level for kid in (node.left,node.right) if kid]
        return ret
```
### 二叉树中的一些重要属性
* 二叉树的最小深度
```python
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        if root.left==None or root.right==None:
            return self.minDepth(root.left)+self.minDepth(root.right)+1
        else:
            return min(map(self.minDepth,(root.left,root.right)))+1
```
* 二叉树的层平均值
```python
class Solution:
    def averageOfLevels(self, root: TreeNode) -> List[float]:
        average=[]
        if root:
            level=[root]
            while level:
                average.append(sum(i.val for i in level)/len(level))
                level=[kid for node in level for kid in (node.left,node.right) if kid]
        return average
```
* 二叉树的直径
    * 采用分治和递归的思想：根节点为root的二叉树的直径 = max(root-left的直径, root->right的直径，root->left的最大深度+root->right的最大深度+1)
    * 分两种情况，1，最大直径经过根节点，则直径为左子树最大深度+右子树最大深度 2.如果不经过根节点，则取左子树或右子树的最大深度
```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        self.diameter=0
        def dfs(root):
            if root==None:
                return 0
            left=dfs(root.left)
            right=dfs(root.right)
            self.diameter=max(self.diameter,left+right)
            return max(left,right)+1
        dfs(root)
        return self.diameter
```
* 两个二叉树的最低公共祖先节点
```python
'''
思路：递归，如果当前节点就是p或q，说明当前节点就是最近的祖先；如果当前节点不是p或p，
就试着从左右子树里找pq；如果pq分别在一左一右被找到，那么当前节点还是最近的
祖先返回root就好了；否则，返回它们都在的那一边。
'''
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root==None:   
            return 
        if root==p or root==q:
            return root
        left=self.lowestCommonAncestor(root.left,p,q)
        right=self.lowestCommonAncestor(root.right,p,q)
        if left and right:
            return root
        if left:
            return left
        if right:
            return right
        else:
            return 
```
##  字符串算法
* 最长公共前缀
```python
# leetcode 14
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        temp_str=""
        zip_obj = zip(*strs)
        for obj in zip_obj:
            if(len(set(obj)))>1:
                break
            temp_str+=obj[0]
        return temp_str 
```
* 最长回文子串
```python
# leetcode 5
class Solution:
    def longestPalindrome(self, s: str) -> str:
        start=0
        maxstr=0
        for i in range(len(s)):
            if i-maxstr>=1 and s[i-maxstr-1:i+1]==s[i-maxstr-1:i+1][::-1]:
                start = i-maxstr-1
                maxstr+=2
                continue
            if  i-maxstr>=0 and s[i-maxstr:i+1]==s[i-maxstr:i+1][::-1]:
                start = i-maxstr
                maxstr+=1
        return s[start:start+maxstr]
```
* 无重复字符的最长子串
```python
# leetcode 3 ：滑动窗口法
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        max_str=0
        for i in range(len(s)):
            if len(s[i-max_str:i+1])==len(set(s[i-max_str:i+1])):
                max_str+=1
        return max_str
```
##  哈希算法
* 哈希函数特性：确定性、哈希冲突，不可逆性，混淆特性
    * 哈希表的 load factor（负载因子），越大表明哈希表中填的元素越多，越容易发生冲突
    * 解决哈希冲突：开放寻址法（线性探测、二次探测、双重哈希），链表法
    * 哈希洪水攻击
* 实现哈希表
```python
class HashMap:
    def __init__(self):
        self.size = 11
        self.slots = [None]*self.size
        self.data = [None]*self.size

    def put(self, key, data):
        hash_value = self.hash(key, len(self.slots))
        if self.slots[hash_value] == None:
            self.slots[hash_value] = key
            self.data[hash_value] = data
        else:
            # 键一样时替换
            if self.slots[hash_value] == key:
                self.data[hash_value] = data
            else:
                # 重新散列，+1线性探测
                next_slot = self.rehash(hash_value, len(self.slots))
                while self.slots[next_slot] != None and self.slots[next_slot] != key:
                    next_slot = self.rehash(next_slot, len(self.slots))
                if self.slots[next_slot] != None:
                    self.slots[next_slot] = key
                    self.data[next_slot] = data
                else:
                    self.data[next_slot] = data

    def get(self, key):
        start_slot = self.hash(key, len(self.slots))
        found = False
        data = None
        stop = False
        position = start_slot
        while self.slots[position] != None and not stop and not found:
            if self.slots[position] == key:
                data = self.data[position]
                found = True
            else:
                position = self.rehash(position,len(self.slots))
                if position==start_slot:
                    stop = True
        return data 

    def hash(self, key, size):
        return key % size

    def rehash(self, oldhash, size):
        return (oldhash+1) % size

    def __getitem__(self, key):
        return self.get(key)

    def __setitem__(self, key, data):
        return self.put(key, data)
hashmap=HashMap()
hashmap[0]='cat'
hashmap[1]='dog'
hashmap[2]='bird'
print(hashmap.slots)
print(hashmap.data  )
```
* 两数之和
```python
# leetcode 1
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        #enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)
        # 组合为一个索引序列，同时列出数据和数据下标
        demo_dict = dict((num,i)for i,num in enumerate(nums))
        for i,num in enumerate(nums):
            #get()方法和[key]方法的主要区别在于[key]在遇到不存在的key时会抛出KeyError错
            j=demo_dict.get(target-num)
            if j and i!=j:
                return [i,j]
```
* 三数之和
```python
# leetcode 15
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        # 排序后遍历
        for t in range(len(nums)- 2):
            # 跳过相同的情况
            if t > 0 and nums[t] == nums[t - 1]:
                    continue
            # i向后判断，j向前判断
            i, j = t + 1, len(nums) - 1
            while i < j:
                _sum = nums[t] + nums[i] + nums[j]
                if _sum == 0:
                    res.append([nums[t], nums[i], nums[j]])
                    i += 1
                    j -= 1
                    # 跳过相同的情况
                    while i < j and nums[i] == nums[i - 1]:
                        i += 1
                    while i < j and nums[j] == nums[j + 1]:
                        j -= 1
                elif _sum < 0:
                    i += 1
                else:
                    j -= 1
        return res
```
* 重复的 DNA 序列
```python
# leetcode 187
class Solution:
    def findRepeatedDnaSequences(self, s: str) -> List[str]:
        res=dict()
        for i in range(len(s)-9):
            res[s[i:i+10]]=res.get(s[i:i+10],0)+1
        return [k for k,v in res.items() if v>=2]
```
* 两个数组的交集
```python
# leetcode 350
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        res=[]
        num_dict={}
        for num in nums1:
            if num not in num_dict:
                num_dict[num]=1
            else:
                num_dict[num]+=1
        for num in nums2:
            if num in num_dict and num_dict[num]>0:
                num_dict[num]-=1
                res.append(num)
        return res
```
##  日常算法思想
### 回溯 
* 主要参考
    * [Backtracking回溯法(又称DFS,递归)全解](https://segmentfault.com/a/1190000006121957)
    * leetcode 22 / leetcode 39 / leetcode 40 /leetcode 46 / leetcode 79
```python
# leetcode 17
class Solution(object):
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        if not digits:
            return []
        d = {'2':"abc",
             '3':"def",
             '4':"ghi",
             '5':"jkl",
             '6':"mno",
             '7':"pqrs",
             '8':"tuv",
             '9':"wxyz"
            }
        res = []
        self.dfs(d, digits, "", res)
        return res
    
    def dfs(self, d, digits, tmp , res):
        if not digits:
            res.append(tmp)
        else:
            for num in d[digits[0]]:
                self.dfs(d, digits[1:], tmp + num, res)
```
### 分而治之
* leetcode 4 / leetcode 215 / leetcode 241
```python
# leetcode 215
'''
任意选定数组内一个数mark，将比其大和比其小的值分别放在左右两个数组里，接着判断两边数字
的数量，如果左边的数量大于k个，说明第k大的数字存在于mark及mark左边的区域，对左半区执行
partition函数；如果左边的数量小于k个，说明第k大的数字在mark和mark右边的区域之内，对右
半区执行partition函数。直到左半区刚好有k-1个数，那么第k大的数就已经找到了。
'''
class Solution(object):
    def findKthLargest(self, nums, k):
        mark = random.choice(nums)
        num1,num2=list(),list()
        for num in nums:
            if num>mark:
                num1.append(num)
            if num<mark:
                num2.append(num)
        if k<=len(num1):
            return self.findKthLargest(num1,k)
        if k>len(nums)-len(num2):
            return self.findKthLargest(num2,k-(len(nums)-len(num2)))
        return mark
```
### 动态规划
* 硬币找零
```python
# leetcode 322
class Solution(object):
    def coinChange(self, coins, amount):
    # 建一个长度是 amount + 1的dp数组，构成面额从0到 amount需要使用的最少硬币数量。
        dp = [0] + [-1] * amount
        for x in range(amount):
            if dp[x] < 0:
                continue
            for c in coins:
                if x + c > amount:
                    continue
                if dp[x + c] < 0 or dp[x + c] > dp[x] + 1:
                    dp[x + c] = dp[x] + 1
        return dp[amount]
```