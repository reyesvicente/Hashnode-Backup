---
title: "4 Common Data Structures"
datePublished: Mon Jul 29 2019 17:20:38 GMT+0000 (Coordinated Universal Time)
cuid: ckgosxtlh05jvncs13djoat0q
slug: 4-common-data-structures
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682770849695/23732a25-47d0-4a0b-a02c-79d711686554.jpeg

---

*Originally posted on my* [*blog*](https://highcenburg.herokuapp.com)

# 1.) Arrays

* A collection of elements identified by an index or a key
    

### example:

```python
ex_arr = [1, 'string', 3, 'four']
print(ex_arr[3])
```

### answer:

```bash
four
```

# 2.) Linked Lists

* A collection of data elements, called nodes that contain a reference to the next node in the list and holds whatever data the application needs
    

### examples:

### the node class

```python
class Node(object):
    def __init__(self, val):
        self.val = val
        self.next = None

    def get_data(self):
        return self.val

    def set_data(self, val):
        self.val = val

    def get_next(self):
        return self.next

    def set_next(self, next):
        self.next = next
```

### the linkedList class

```python
class LinkedList(object):
    def __init__(self, head=None):
        self.head = head
        self.count = 0

    def get_count(self):
        return self.count

    def insert(self, data):
        new_node = Node(data)
        new_node.set_next(self.head)
        self.head = new_node
        self.count += 1

    def find(self, val):
        item = self.head
        while (item != None):
            if item.get_data() == val:
                return item
            else:
                item = item.get_next()
        return None

    def deleteAt(self, idx):
        if idx > self.count:
            return
        if self.head == None:
            return
        else:
            tempIdx = 0
            node = self.head
            while tempIdx < idx-1:
                node = node.get_next()
                tempIdx += 1
            node.set_next(node.get_next().get_next())
            self.count -= 1

    def dump_list(self):
        tempnode = self.head
        while (tempnode != None):
            print("Node: ", tempnode.get_data())
            tempnode = tempnode.get_next()
```

### create a linked list and insert some items

```python
itemlist = LinkedList()
itemlist.insert(38)
itemlist.insert(49)
itemlist.insert(13)
itemlist.insert(15)

itemlist.dump_list()
```

### exercise the list

```python
print("Item count: ", itemlist.get_count())
print("Finding item: ", itemlist.find(13))
print("Finding item: ", itemlist.find(78))
```

### delete an item

```python
itemlist.deleteAt(3)
print("Item count: ", itemlist.get_count())
print("Finding item: ", itemlist.find(38))
itemlist.dump_list()
```

### answer:

```python
Node:  15
Node:  13
Node:  49
Node:  38
Item count:  4
Finding item:  <__main__.Node object at 0x106568990>
Finding item:  None
Item count:  3
Finding item:  None
Node:  15
Node:  13
Node:  49
```

# 3.) Stacks and Queues

* Stacks is a collection of operations that supports push and pop operations. The last item pushed is the first one popped.
    

### example:

### create a new empty stack

```python
stack = []
```

### push items onto the stack

```python
stack.append(1)
stack.append(2)
stack.append(3)
stack.append(4)
```

### print the stack contents

```python
print(stack)
```

### pop an item off the stack

```python
x = stack.pop()
print(x)
print(stack)
```

### answer:

```python
[1, 2, 3, 4]
4
[1, 2, 3]
```

* A Stack is a collection of operations that supports push and pop operations. The last item pushed is the first one popped.
    

### example:

```python
from collections import deque
```

### create a new empty deque object that will function as a queue

```python
queue = deque()
```

### add some items to the queue

```python
queue.append(1)
queue.append(2)
queue.append(3)
queue.append(4)
```

### print the queue contents

```bash
print(queue)
```

### pop an item off the front of the queue

```bash
x = queue.popleft()
print(x)
print(queue)
```

### answer:

```bash
deque([1, 2, 3, 4])
1
deque([2, 3, 4])
```

# 4.) Hash Tables (Dictionary)

* A data structure that maps keys to its associated values
    

### Benefits:

* Key-to-value maps are unique
    
* Hash tables are very fast
    
* For small datasets, arrays are usually more efficient
    
* Hash tables don't order entries in a predictable way
    

### example:

#### create a hashtable all at once

```bash
items1 = dict(
        {
            "key1": 1, 
            "key2": 2, 
            "key3": "three"
        }
    )
print(items1)
```

### create a hashtable progressively

```bash
items2 = {}
items2["key1"] = 1
items2["key2"] = 2
items2["key3"] = 3
print(items2)
```

### replace an item

```bash
items2["key2"] = "two"
print(items2)
```

### iterate the keys and values in the dictionary

```bash
for key, value in items2.items():
    print("key: ", key, " value: ", value)
```

### Answer:

```bash
{'key1': 1, 'key2': 2, 'key3': 'three'}
{'key1': 1, 'key2': 2, 'key3': 3}
{'key1': 1, 'key2': 'two', 'key3': 3}
key:  key1  value:  1
key:  key2  value:  two
key:  key3  value:  3
```

#Real World Examples:

### Filter out duplicate items

### define a set of items that we want to reduce duplicates

```bash
items = ["apple", "pear", "orange", "banana", "apple",
         "orange", "apple", "pear", "banana", "orange",
         "apple", "kiwi", "pear", "apple", "orange"]
```

### create a hashtable to perform a filter

```bash
filter = dict()
```

### loop over each item and add to the hashtable

```bash
for item in items:
    filter[item] = 0
```

### create a set from the resulting keys in the hashtable

```bash
result = set(filter.keys())
print(result)
```

### output:

```bash
{
    'kiwi',
    'apple',
    'pear',
    'orange',
    'banana'
}
```

### Find a maximum value

### declare a list of values to operate on

```bash
items = [6, 20, 8, 19, 56, 23, 87, 41, 49, 53]

def find_max(items):
    # breaking condition: last item in list? return it
    if len(items) == 1:
        return items[0]

    # otherwise get the first item and call function
    # again to operate on the rest of the list
    op1 = items[0]
    print(op1)
    op2 = find_max(items[1:])
    print(op2)

    # perform the comparison when we're down to just two
    if op1 > op2:
        return op1
    else:
        return op2
```

### test the function

```bash
print(find_max(items))
```

### output:

```bash
6
20
8
19
56
23
87
41
49
53
53
53
87
87
87
87
87
87
87
```

### Counting Items

### define a set of items that we want to count

```bash
items = ["apple", "pear", "orange", "banana", "apple",
         "orange", "apple", "pear", "banana", "orange",
         "apple", "kiwi", "pear", "apple", "orange"]
```

### create a hashtable object to hold the items and counts

```bash
counter = dict()
```

### iterate over each item and increment the count for each one

```bash
for item in items:
    if item in counter.keys():
        counter[item] += 1
    else:
        counter[item] = 1
```

### print the results

```bash
print(counter)
```

### output:

```bash
{'apple': 5, 'pear': 3, 'orange': 4, 'banana': 2, 'kiwi': 1}
```