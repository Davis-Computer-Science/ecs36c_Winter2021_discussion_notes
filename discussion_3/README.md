# Discussion 3

## Roadmap
1. Linked List
2. Data Structure Based on Linked List
3. File I/O: ifstream & ofstream

## Linked List

### Circular Linked List

<img src="img/circularly-linked-list.svg.png" height="40" align=center />

The last node of the linked list is linked with another node (instead of `NULL`) in the list. In the homework, you just need to print each node once if the linked list is circularly linked.

```
Linked list:
┌───┬───┐  ┌───┬───┐  ┌───┬───┐
│ 1 │  ─┼─→│ 2 │  ─┼─→│ 3 │ │ │
└───┴───┘  └───┴───┘  └───┴─┼─┘
  ↑                         |
  └─────────────────────────┘
Operation: p 0 4
Output: 1 2 3 (not 1 2 3 1)
```

### Singly & Doubly Linked List
<img src="img/singly-linked-list.png" height="30" align=center /> 

A node of singly linked list includes 1 field called `next`, which points to the next node in the linked list.

**Reverse** a singly linked list:
```
def reverse(l):
    if l.head == None or l.head.next == None:
        return
    prev = None
    while l.head.next != None:
        next = l.head.next
        l.head.next = prev
        prev = l.head
        l.head = next
    l.head = prev
```

<img src="img/doubly-linked-list.png" height="30" align=center />

A node of doubly linked list includes 2 field called `prev` and `next` which points to the previous and next node respectively.

**Reverse** a singly linked list:
```
def reverse(l):
    if l.head == None or l.head.next == None:
        return
    prev = None
    while l.head.next != None:
        next = l.head.next
        l.head.next = prev
        l.head.prev = next
        prev = l.head
        l.head = next
    l.head.next = prev
    l.head.prev = None
```

## Data Structure Based on Linked List
Linked list is a kind of basic data structures. And some other data structures can be builded with linked list. Comparing with array, linked list allows flexible memory allocation. 

However, linked list doesn't support random access to every node, and the time cost of search for the worst case is $O(n)$ ($O(1)$ for array-based data structures).

### Stack
```
A stack of size 3:
                 ┆      return: 3      ┆                     ┆  index
         ┌───┐   ┆            ┌───┐    ┆              ┌───┐  ┆
top    → │ 3 │   ┆            │   │    ┆     top    → │ 4 │  ┆  ←- 2
         ├───┤  Pop           ├───┤  Push 4           ├───┤  ┆
         │ 2 │  --→  top    → │ 2 │   ---→            │ 2 │  ┆  ←- 1
         ├───┤   ┆            ├───┤    ┆              ├───┤  ┆
bottom → │ 1 │   ┆   bottom → │ 1 │    ┆     bottom → │ 1 │  ┆  ←- 0
        ─┴───┴─  ┆           ─┴───┴─   ┆             ─┴───┴─ ┆
```

Can be reconstructed with a linked list:

```
Linked list based stack:
 Head
  ↓
┌───┬───┐  ┌───┬───┐  ┌───┬───┐
│ 3 │  ─┼─→│ 2 │  ─┼─→│ 1 │  ─┼─→ None
└───┴───┘  └───┴───┘  └───┴───┘
┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄  Pop ↓ ┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄  = Del first node
 Head          return: 3
  ↓
┌───┬───┐  ┌───┬───┐
│ 2 │  ─┼─→│ 1 │  ─┼──→ None
└───┴───┘  └───┴───┘
┄┄┄┄┄┄┄┄┄┄┄┄┄  Push 4 ↓ ┄┄┄┄┄┄┄┄┄┄┄┄┄┄  = Insert a new node
 Head
  ↓
┌───┬───┐  ┌───┬───┐  ┌───┬───┐
│ 4 │  ─┼─→│ 2 │  ─┼─→│ 1 │  ─┼─→ None
└───┴───┘  └───┴───┘  └───┴───┘
```
### Queue
```
A queue of size 6:
 Head        Tail
  ↓           ↓
┌───┬───┬───┬───┬───┬───┐
│ 2 | 3 | 1 | 4 |   |   |
└───┴───┴───┴───┴───┴───┘
┄┄┄┄┄┄┄┄┄  Dequeue ↓ ┄┄┄┄┄┄┄┄┄
           return 2
     Head    Tail
      ↓       ↓
┌───┬───┬───┬───┬───┬───┐
│   | 3 | 1 | 4 |   |   |
└───┴───┴───┴───┴───┴───┘
┄┄┄┄┄┄┄┄  Enqueue 2 ↓ ┄┄┄┄┄┄┄┄
     Head        Tail
      ↓           ↓
┌───┬───┬───┬───┬───┬───┐
│   | 3 | 1 | 4 | 2 |   |
└───┴───┴───┴───┴───┴───┘
┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄
  ↑   ↑   ↑   ↑   ↑   ↑
  0   1   2   3   4   5  ┄ index
```
Can be reconstructed with a linked list:

```
Linked list based stack:
 Head
  ↓
┌───┬───┐  ┌───┬───┐  ┌───┬───┐   ┌───┬───┐
│ 4 │  ─┼─→│ 1 │  ─┼─→│ 3 │  ─┼─→ │ 2 │  ─┼─→ None
└───┴───┘  └───┴───┘  └───┴───┘   └───┴───┘
┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄  Dequeue ↓ ┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄  = Del last node
 Head          return: 2
  ↓
┌───┬───┐  ┌───┬───┐  ┌───┬───┐
│ 4 │  ─┼─→│ 1 │  ─┼─→│ 3 │  ─┼─→ None
└───┴───┘  └───┴───┘  └───┴───┘
┄┄┄┄┄┄┄┄┄┄┄┄┄  Enqueue 2 ↓ ┄┄┄┄┄┄┄┄┄┄┄┄┄┄  = Insert a new node
 Head
  ↓
┌───┬───┐  ┌───┬───┐  ┌───┬───┐   ┌───┬───┐
│ 2 │  ─┼─→│ 4 │  ─┼─→│ 1 │  ─┼─→ │ 3 │  ─┼─→ None
└───┴───┘  └───┴───┘  └───┴───┘   └───┴───┘
```

## I/O Stream: file & string
```c++
using std;

char *input_path = "input/1.txt";
char *output_path = "output/1.txt";
ifstream input_file(input_path);
ofstream output_file(output_path);
string str;
string flag;

while (getline(input_file, str)) {
    stringstream ss(str);
    ss >> flag;
    output_file << str << endl;
}

input_file.close();
output_file.close();
```