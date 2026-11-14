# LeetCode Summary

## Interview approach

For eBay Goldin's frontend and backend coding rounds, prepare for LeetCode-style problem solving. Communicate assumptions, invariants, edge cases, time/space complexity, and tests aloud.

## 240. Search a 2D Matrix II

- Pattern: start at the top-right corner of a matrix whose rows and columns are sorted.
- If current value is too large, move left; if too small, move down.
- Complexity: \(O(m + n)\) time and \(O(1)\) extra space.
- Lesson: the recursive version has the same asymptotic time but uses call-stack space and has higher constant overhead than a loop.
- Common bug: only stop when out of bounds; do not stop merely because one direction is unavailable.

## 1038. Binary Search Tree to Greater Sum Tree

- Pattern: reverse in-order traversal: right → node → left.
- Maintain a running sum of values already visited.
- When visiting a node, add its original value to the sum, then replace the node value with the sum.
- Complexity: \(O(n)\) time and \(O(h)\) call-stack space.
- LeetCode's array-like tree display is converted to TreeNode objects, so use `node.left`, `node.right`, and `node.val`.

> Add one Markdown file per new problem or pattern as you review it.
