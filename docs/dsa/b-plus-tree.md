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

