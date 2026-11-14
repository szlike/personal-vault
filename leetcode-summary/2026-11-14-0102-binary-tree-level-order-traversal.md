# 102. Binary Tree Level Order Traversal

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
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

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original practice: 2026-10-18.
- **For follow-up:** Share [102. Binary Tree Level Order Traversal](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-0102-binary-tree-level-order-traversal.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
