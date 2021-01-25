# 剑指 Offer 35. 复杂链表的复制
请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

## python 用dic存储老链表node和新列表node的对应关系
```python
# 执行用时：40 ms, 在所有 Python3 提交中击败了95.17% 的用户
# 内存消耗：14 MB, 在所有 Python3 提交中击败了50.42% 的用户

"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)
        self.next = next
        self.random = random
"""
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if head == None:
            return None

        new_head = Node(head.val)
        new_node = new_head

        # 此处注意加入None节点的对应关系
        dic = {head: new_head, None: None}

        while head:
            if head.next in dic:
                new_node.next = dic[head.next]
            else:
                dic[head.next] = Node(head.next.val, None, None)
                new_node.next = dic[head.next]
            if head.random in dic:
                new_node.random = dic[head.random]
            else:
                dic[head.random] = Node(head.random.val, None, None)
                new_node.random = dic[head.random]
            head = head.next
            new_node = new_node.next

        return new_head
```

## c++ map存储对应关系
```c++
// 执行用时：20 ms, 在所有 C++ 提交中击败了54.65% 的用户
// 内存消耗：11.2 MB, 在所有 C++ 提交中击败了9.80% 的用户

/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(! head) return nullptr;
        unordered_map<Node*, Node*> dic;

        Node* new_head = new Node(head->val); 
        Node* new_node = new_head;

        dic[head] = new_head;
        dic[nullptr] = nullptr;

        while(head){
             if(! dic.count(head->next)) dic[head->next] = new Node(head->next->val);
             new_node->next = dic[head->next];
             if(! dic.count(head->random)) dic[head->random] = new Node(head->random->val);
             new_node->random = dic[head->random];

             new_node = new_node->next;
             head = head->next;
            }

        return new_head;
    }
};
```

## python 不使用dic，直接在链表上扩展，但是过程中修改了原链表结构
```python
# 执行用时：44 ms, 在所有 Python3 提交中击败了86.53% 的用户
# 内存消耗：14 MB, 在所有 Python3 提交中击败了63.34% 的用户

"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)
        self.next = next
        self.random = random
"""
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if head == None:
            return None

        old_head = head
        # 在每个节点后面扩展拷贝一个新节点
        while head:
            new_node = Node(head.val, head.next, head.random)
            head.next = new_node
            head = new_node.next

        # 将新节点的random指向正确新节点
        head = old_head.next
        while True:
            if head.random:
                head.random = head.random.next
            if not head.next:
                break
            head = head.next.next

        # 拆分新节点为新链表
        new_head = old_head.next
        new_node = new_head
        head = old_head
        while new_node:
            head.next = new_node.next
            head = new_node
            new_node = new_node.next

        return new_head
```