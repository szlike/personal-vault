# 1038. Binary Search Tree to Greater Sum Tree

**Recorded:** 2026-11-13

## Pattern

Use reverse in-order traversal:

```
right → node → left
```

Maintain a running sum. When visiting each node, add its value to the sum and replace the node value with that sum.

## Why it works

The traversal visits values from largest to smallest, so the running sum already includes every greater value.

## Complexity

- Time: \(O(n)\)
- Extra space: \(O(h)\) from the recursion stack

## LeetCode representation

LeetCode displays trees using arrays, but the function receives `TreeNode` objects. Use `node.left`, `node.right`, and `node.val`.
