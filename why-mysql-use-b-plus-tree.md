---
type: slide
slideOptions: # 簡報相關的設定
  # theme: solarized   # 顏色主題
  transition: "fade" # 換頁動畫
  slideNumber: true
  # spotlight:
  # enabled: true
---

# Why MySQL use B+Tree

Shawn

---

## What the heck if I know this

---

![](https://hackmd.io/_uploads/SJXhrlpLh.png =700x)

---

![](https://hackmd.io/_uploads/rkYkXHRU2.png =550x)

---

## Potential data structure

![](https://hackmd.io/_uploads/HkS5weaL2.png)

---

### Difference between B Tree and B+Tree

---

### Data structure

- [B Tree](https://www.cs.usfca.edu/~galles/visualization/BTree.html)
  ![](https://hackmd.io/_uploads/HJwhBXC82.png =300x)
- [B+Tree](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)
  ![](https://hackmd.io/_uploads/HyFTHm0Ln.png =550x)

---

### B Tree in DB

![](https://hackmd.io/_uploads/S1dDl40Ih.png =800x)

- element has a key and a value (page + rowid)
- value can point to primary key or tuple
- a node = disk page

---

### B+Tree in DB

![](https://hackmd.io/_uploads/HkpNh4CLn.png =700x)

- values are only stored in leaf nodes
- internal nodes can fit more elements since they only store keys
- leaf nodes are "linked" so once you find a key you can find all values

---

## From two aspects

- read write
- I/O

---

## From read write

---

### B Tree, B+Tree

- single row CRUD: `O(log n)`
- good for range query or sort

---

### Hash

- single row CRUD: `O(1)`
- bad for range query or sort, can become `full table scan`
  - because the original orderly key value, after the hash algorithm, may `become discontinuous`
- [They are not used for comparison operators such as `<` that find a range of values](https://dev.mysql.com/doc/refman/8.0/en/index-btree-hash.html)

---

### Comparison

|                   | B Tree, B+Tree |      Hash       |
| ----------------- | :------------: | :-------------: |
| single row CRUD   |    O(log n)    |      O(1)       |
| range query, sort |     index      | full table scan |

---

## HASH

![](https://hackmd.io/_uploads/rJ1Fmx6U3.gif)

---

## From I/O

---

### What is I/O

- when reading document, computer uses `page` as unit to read data to memory

- when we execute a query, it's the same concept

- usually page size is `4KB` (different by OS)

```bash
> getconf PAGE_SIZE # use Mac m1 pro
16384               # 16KB
```

---

### I/O for B Tree

- if we want to find `4~9`, we need `4 times` I/O
  ![](https://hackmd.io/_uploads/ryAzWyaI2.png)

- all nodes maybe have values we want, so always I/O from root node to child node again and again

- cause much I/O and random I/O

---

### I/O for B+Tree

- all values store in leaf nodes, and leaf nodes have pointer connecting each other
  ![](https://hackmd.io/_uploads/SyEU-y6L3.png)

---

## B Tree

![](https://hackmd.io/_uploads/rJ1Fmx6U3.gif)

---

## Summary

- need range query and sort, so don't use Hash
- B Tree has much I/O
- as a result, InnoDB uses B+Tree as its data structure

---

## Reference

- <span style="font-size:0.7em">https://draveness.me/whys-the-design-mysql-b-plus-tree/</span>
- <span style="font-size:0.7em">https://www.cs.usfca.edu/~galles/visualization/BTree.html</span>
- <span style="font-size:0.7em">https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html</span>
- <span style="font-size:0.7em">https://medium.com/@mena.meseha/what-is-the-difference-between-mysql-innodb-b-tree-index-and-hash-index-ed8f2ce66d69</span>
- <span style="font-size:0.7em">https://dev.mysql.com/doc/refman/8.0/en/index-btree-hash.html</span>
- <span style="font-size:0.7em">https://www.udemy.com/course/database-engines-crash-course/</span>
- <span style="font-size:0.7em">https://memes.tw/</span>
- <span style="font-size:0.7em">https://tenor.com/</span>

---

## Q&A

---

## Why official docs use "B Tree"

---

### From [StackExchange](https://dba.stackexchange.com/a/204573)

- please ask MySQL or MariaDB engineer
- `+` is a operator
- not all MySQL users are supposed to know what B+Tree is

---

### From [Jeremy Cole](https://blog.jcole.us/innodb/)

- ex-Google MySQL team head
- [InnoDB uses a B+Tree structure for its indexes](https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/)
