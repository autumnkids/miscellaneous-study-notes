# B Tree

A balanced binary search tree that improves search and insertion operations with less memory and I/Os.

Each node in the tree can contains multiple keys that allows the tree to have large branching factor. And thus, the tree can come with smaller height.

A B Tree algorithm copies selected blocks from the disk to the memory, and writes back the blocks that have changed onto disk. It only keeps a constant number of blocks in the memory at any time, and thus does not limit the size of the tree that can be handled.

## Properties

Assuming that there is a B Tree with an order of `m`, and a minimum degree of `t` (which is usually defined by the block size of a disk):

* Each internal node (excluding root) has the number of child nodes within the range of `[ceil(m / 2), m]`, or `[t, 2t]`.
* The root node can have either 2 nodes, or 0 - if the tree has only one node, which is the root.
* The number of keys contained in each node equals to `the number of child nodes - 1`.
* The root node may contain a minimum of 1 key.
* All keys of a node are sorted in increasing order.
* The range of keys follows the property of a Binary Search Tree where the range of keys within a child node is bounded by the range of keys within its parent node. For example:
    * childNode[0].keys[i] < parentNode.keys[0], i.e. the first branch of a node may only contains keys that are less than the first key in the node.
    * childNode[1].keys[i] > parentNode.keys[0] && childNode[1].keys[i] < parentNode.keys[1], i.e. the second branch of a node may only contains keys that are greater than the first key in the node and less than the second key in the node.
    * and so on.
* All leaf nodes are at the same level.
* The time complexity for search, insertion and deletion operation is O(logn).

# B+ Tree

B+ Tree is an advanced usage of B Tree.

## Properties

Other than the properties maintained in a B Tree, a B+ Tree also comes with the following properties:

* The data pointers are all stored in the leaf nodes, while a B Tree may store both the data and the keys in each internal node.
* The internal nodes in a B+ Tree are used to guide the search.
* All leaf nodes form a linked list for efficient range-based queries.
* A B+ Tree usually comes with higher order - i.e. more keys in each node.
* A B+ Tree allows key duplication in leaf nodes, while a B Tree does not allow.
* A B+ Tree requires less disk I/O, since it supports sequential reads with the linked list structure in the leaf nodes.
