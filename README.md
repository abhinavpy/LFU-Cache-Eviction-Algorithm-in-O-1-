# LFU-Cache-Eviction-Algorithm-in-O(1)

Source: 
* An O(1) algorithm for implementing the LFU cache eviction scheme, http://dhruvbird.com/lfu.pdf
* Java Implementation - https://deepakvadgama.com/blog/lfu-cache-in-O(1)/


The following notes are from the above mentioned research paper.

## Introduction
* Eviction Algorithms - Used in operating systems, databases and other systems that use caches to speed up execution data.
* Many policies such as.
  * MRU (Most Recently Used)
  * MFU (Most Frequently Used)
  * LRU (Least Recently Used)
  * LFU (Least Frequently Used)
Most widely used algorithm is LRU, both for its O(1) speed of operation as well as its close resemblance to the kind of behavior that is expected from most applications.
The LFU algorithms also has many applications.

## Dictionary Operations Supported by an LFU cache
Three different operations on cache data:
1. Set (or insert) an item in the cache
2. Retrieve or Lookup an item in the cache
3. Evict (or delete) the least frequently used item in the cache

## Currently best known complexity of the LFU algorithm:

- Insert O(logn)
- Lookup O(logn)
- Delete O(logn)

These complexity values follow directly from the complexity of the binomial heap implementation and a standard collision free hash table. An LFU caching strategy can be easily and efficiently implemented using a min heap data structure and a hash map. The minimum heap is created based on the usage count (of the item) and the hash table is indexed via the the element’s key.
All operations on a collision free hash table are of order O(1), so the runtime of LFU cache is governed by the runtime of operations on the min heap.

When an element is inserted into the cache, it enters with the usage count of 1 and since insertion into the minimum heap costs O(logn), inserts into the LFU cache take O(logn) time.

When an element is looked up, it is found via a hashing function which hashes the key to the actual element. Simultaneously, the usage count (the count in the max heap) is incremented by one, which results in the reorganization of the min heap and the element moves away from the root, since the element can move down up to logn levels at any stage, this operation too takes O(logn) time.
When an element is selected for eviction and then eventually removed from the heap, it can cause significant reorganization of the heap data structure. The element with the least usage count is present at the root of the min heap. Deleting the root of the min heap involves replacing the root node with the last leaf node in the heap, and building this node down to its correct position. This operation too has a complexity of O(log n).


## The proposed LFU algorithm

The proposed LFU algorithm has a runtime of O(1) for each of the dictionary operations (insertion, lookup, deletion) that can be performed on an LFU cache. This is maintained by maintaining two linked lists; one on the access frequency and one for all elements that have the same access frequency.  
A hash table is used to access elements by key. A doubly linked list is used to link together nodes which represent a set of nodes that have the same access frequency. We refer to this doubly linked list as the frequency list. This set of nodes that  have the same access frequency is actually a doubly linked list. We refer to the doubly linked list (which is local to a particular frequency) as a node list. Hence x and y will have a pointer back to node 1, nodes z and a will have a pointer back to node 2 and so on.…..

The hash table used to locate elements by key is indicated by the variable byKey. We use a Set in lieu of a linked list for storing elements with the same access frequency for simplicity of implementation. The variable items is a standard Set data structure which holds the keys of such elements that have the same frequency. Its Insertion, lookup, and deletion runtime complexity is O(1).
