# 102. Binary Tree Level Order Traversal

**Recorded:** 2026-11-14  
**Original practice:** 2026-10-18

## Pattern

Breadth-first traversal, processing exactly the current queue level before advancing.

```js
function levelOrder(root) {
  if (!root) return [];
  const queue = [root], result = [];
  let head = 0;
  while (head < queue.length) {
    const size = queue.length - head;
    const level = [];
    for (let i = 0; i < size; i++) {
      const node = queue[head++];
      level.push(node.val);
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    result.push(level);
  }
  return result;
}
```

## Complexity

O(n) time; O(n) queue/output space in worst case. A head index avoids repeated `Array.shift()` O(n) movement.

## Past mistakes and feedback

- **Observed:** Used `node.value` instead of LeetCode `TreeNode.val`.
- **Observed:** `head =+ levelSize` is an assignment of positive value; the intended operation is `head += levelSize`, though the inner loop with `head++` is even clearer.
- **Observed general BFS mistake:** Mark visited on enqueue, not after dequeue (for graph BFS), to avoid repeated work.

## Points to improve

1. Explain the invariant: `size` is the number of nodes at the **current** level, before children are appended.
2. Dry-run empty tree, single node, sparse tree, one-sided chain.
3. Distinguish tree traversal (no cycles in normal input) from graph BFS (visited set needed).

