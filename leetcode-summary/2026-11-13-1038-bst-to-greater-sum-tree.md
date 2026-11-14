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

## Past mistakes and lessons

- **Traversing in ordinary in-order order.** `left → node → right` visits values from smallest to largest, so it cannot immediately accumulate the greater values. The required direction is `right → node → left`.
- **Mutating before preserving the running sum.** Add the original `node.val` to the accumulator, then assign the accumulator back to the node. Reversing that reasoning becomes error-prone after mutation.
- **Resetting state during recursion.** The sum must be shared across the whole traversal—via an outer variable, a mutable reference, or a helper closure—not recreated for each call.
- **Confusing the input display with the runtime shape.** Array examples illustrate a tree; the submitted solution follows object pointers. Trying to index an array inside the function solves the wrong problem.
- **Ignoring duplicate-value semantics.** The greater-sum-tree definition includes values greater than or equal to the node’s original value; the running sum naturally handles duplicate values when their BST representation is valid.

## Improvements and practice

1. Say the traversal order aloud and explain the invariant: before visiting a node, the accumulator contains all values already seen that are greater than or equal to it.
2. Hand-trace a three-node tree and a skewed tree, recording `sum` before and after each visit.
3. Practice an iterative reverse in-order version with an explicit stack so you can discuss the recursion/iteration trade-off.
4. Test null input, a single node, a left-skewed tree, a right-skewed tree, and duplicate values if the platform permits them.
