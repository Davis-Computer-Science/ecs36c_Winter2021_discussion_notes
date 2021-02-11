# Discussion 5

## Roadmap
1. Implement a Tree
2. FAQ for HW2

## Implement a Tree
- Self-similarity structure of tree. The children of a tree is also a tree (subtree). 
- It's common to use iteration algorithm in self-similarity structure.


The structure of tree node and tree is as follow.

```c++
typedef struct TreeNode {
    int subtree;
    int val;
    TreeNode **children;
} TreeNode;

class Tree {
private:
    TreeNode *root;
};
```

### Some method for general tree

- Height of a tree
```
-----------------------------------------------------------------------------------
            tree      |         sub-tree             |     splited by level 1     |
             1        |                              |       1       |     1      |
            / \       |                              |---------------|------------|
           3   5      |       3             5        |     3   5     |            |
          / \   \     |      / \             \       |    / \   \    |            |
         7   4   3    |     7   4             3      |   7   4   3   |            |
          \     /     |     \        and     /       |    \     /    | max(4, 3)  |
           6   8      |      6              8        |     6   8     |            |
          /           |     /                        |    /          |            |
         2            |    2                         |   2           |            |
-----------------------------------------------------------------------------------
Height:      5        |       4             3        |       max(h_subtree)+1     |
-----------------------------------------------------------------------------------
```

The pseudocode for compute the height of a tree is
```
def height(root):
    if root == None:
        return 0
    elif root.children == []:
        return 1
    else:
        return max([height(i) for i in root.children]) + 1
```

- Levelorder traversal
```
    tree    |  level  |
     1      |    1    |
    / \     |---------|
   3   5    |    2    |
  / \   \   |---------|
 7   4   3  |    3    |
  \     /   |---------|
   6   8    |    4    |
  /         |---------|
 2          |    5    |

Level order traversal: 1 3 5 7 4 3 6 8 2
Splited by level: 1 | 3 5 | 7 4 3 | 6 8 | 2
```

Implementation: Use a queue.
```
Queue: 7 4 3
node = 5
Output: 1 3 5 

def levelorder(root, queue):
    queue.enqueue(root)
    while queue.head != None:
        node = queue.dequeue()
        print(node.val)
        for child in node.children:
            queue.enqueue(child)
```

## FAQ for HW2

### Instruction for input format

l a b:
```
l 2 4
---------------------
1 -> 2 -> 3 -> 4 -> 5
↑
head
---------------------
1 -> 2 -> 3 -> 4 -> 5
↑
ptr
is 2? F
is 4? F
move to ptr -> next
---------------------
1 -> 2 -> 3 -> 4 -> 5
     ↑
     ptr
is 2? T, node_a = 2
is 4? F,
move to ptr -> next
---------------------
1 -> 2 -> 3 -> 4 -> 5
          ↑
          ptr
is 2? F
is 4? F
move to ptr -> next
---------------------
1 -> 2 -> 3 -> 4 -> 5
               ↑
               ptr
is 2? F
is 4? T, node_b = 4
move to ptr -> next
---------------------
1 -> 2 -> 3 -> 4 -> 5
                    ↑
                    ptr
is 2? F
is 4? F
move to ptr -> next
---------------------
1 -> 2 -> 3 -> 4 -> 5
     node_a    node_b
---------------------
1 -> 2    3 -> 4 -> 5
     |         |
     -----------
---------------------
1 -> 2 -> 4 -> 5
```

n a b:
```
n 2 4
---------------------
1 -> 2 -> 3 -> 4 -> 5
↑
head
---------------------
1 -> 2 -> 3 -> 4 -> 5
↑
ptr
is 2? F. move to ptr -> next
---------------------
1 -> 2 -> 3 -> 4 -> 5
     ↑
     ptr
is 2? T.
---------------------
     a
1 -> 2 -> 3 -> 4 -> 5
     ↑
     ptr  4
          b
---------------------
     a
1 -> 2    3 -> 4 -> 5
     |    |
     4 ---
     b
---------------------
1 -> 2 -> 4 -> 3 -> 4 -> 5
```
p i j: Print the linked list starting from [i, j)
```
----------------------------------
index:       0    1    2    3    4
linked list: 1 -> 2 -> 3 -> 4 -> 5
             ↑
             head
```
**Other tips**

p
```
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
F    F    F    F
↑
head
i = 0
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
T    F    F    F
↑
ptr
is flag True? F, set it to T, i += 1 then move to ptr -> next
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
T    T    F    F
     ↑
     ptr
is flag True? F, set it to T, i += 1  then move to ptr -> next
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
T    T    T    F
          ↑
          ptr
is flag True? F, set it to T, i += 1  then move to ptr -> next
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
T    T    T    T
               ↑
               ptr
is flag True? F, set it to T, i += 1  then move to ptr -> next
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
T    T    T    T
     ↑
     ptr
is flag True? T, exit.
i = 4
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
F    T    T    T
↑
ptr
is i == 0? F, set flag to F, i -= 1 then move to ptr -> next
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
F    F    T    T
     ↑
     ptr
is i == 0? F, set flag to F, i -= 1 then move to ptr -> next
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
F    F    F    T
          ↑
          ptr
is i == 0? F, set flag to F, i -= 1 then move to ptr -> next
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
F    F    F    F
               ↑
               ptr
is i == 0? F, set flag to F, i -= 1 then move to ptr -> next
--------------------
      ------------
     ↓            |
1 -> 2 -> 3 -> 4 -
F    F    F    F
     ↑
     ptr
is i == 0? T, exit.
```

Don't forget to reset the flag after `print` and `is_circular`