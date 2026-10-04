# Linked List Lab Notes

This lab focuses on understanding how linked-list structures behave in C++.

## Singly linked list goals

Students should be able to:

- add nodes to the front and back;
- remove from the front and back;
- maintain a correct size counter;
- traverse the list safely;
- search for an element.

## Doubly linked list goals

Students should be able to:

- use sentinel header and trailer nodes;
- maintain correct previous and next links;
- add and remove from both ends;
- avoid breaking the list while deleting nodes.

## Important design points

- The singly linked list stores only a head pointer and a size counter.
- The doubly linked list uses a header and trailer sentinel to simplify edge-case logic.
- Both implementations must avoid memory leaks and must keep the list consistent after every update.

## Pointer management and sentinel-node design

### Memory ownership

Each list owns every node it creates. Nodes are allocated with 'new' in 'push_front' and 'push_back' and freed with 'delete' in 'pop_front' and 'pop_back'. 'clear()' calls 'pop_front()' until the list is empty, and each destructor calls 'clear()', so nothing leaks. The doubly linked list's destructor also deletes its two sentinels.

### Copying

A default copy would make two lists share the same nodes and cause a double free. The copy constructor instead builds new nodes for each value. The assignment operator copies into a temporary and swaps with it, so the temporary frees the old nodes when it is destroyed, and a failed allocation leaves the original list unchanged.

### Singly linked list

The list stores only 'head_' and 'size_'. Front operations are O(1). 'push_back' and 'pop_back' are O(n) because there is no tail pointer, so they walk from the head. Removing the only node must also reset 'head_' to 'nullptr'.

### Doubly linked list and sentinels

'header_' and 'trailer_' never hold data, so an empty list is just 'header_ <-> trailer_'. Real nodes always have a neighbor on each side, so inserts and removals use the same pointer updates whether the list is empty or not, with no 'nullptr' special cases. 'empty()' checks 'header_->next == trailer_', and all end operations are O(1).

### Edge cases

'front()' and 'back()' throw 'std::out_of_range' on an empty list. 'pop_front()' and 'pop_back()' return 'false' instead. Every insert and remove updates 'size_'.