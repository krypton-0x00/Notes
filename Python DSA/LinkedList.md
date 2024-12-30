```python
  
class Node:  
    def __init__(self,value,next_node):  
        self.value = value  
        self.next = next_node  
class LinkedList:  
    def __init__(self,value):  
        node = Node(value,None)  
        self.head = node  
        self.tail = node  
    def push_back(self,value):  
        node = Node(value,None)  
        self.tail.next = node  
        self.tail = node  
  
    def push_front(self,value):  
        node = Node(value,self.head)  
        self.head = node  
  
    def pop(self):  
        old_head = self.head  
        self.head = self.head.next  
        old_head.next = None  
  
    def pop_front(self):  
        next_head = self.head.next  
        self.head.next = None  
        self.head = next_head  
  
    def print(self):  
        h = self.head  
        while h is not None:  
            print(h.value)  
            h = h.next
```