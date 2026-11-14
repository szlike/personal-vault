# 130. Surrounded Regions

**Recorded:** 2026-11-14  
**Original practice:** 2026-10-19

## Pattern

An `O` touching a boundary (or connected to one by `O`s) must **not** be captured. Start DFS/BFS from all border `O`s, mark those cells safe, then flip only remaining `O`s to `X`.

```js
function solve(board) {
  const rows = board.length;
  if (!rows) return;
  const cols = board[0].length;
  const visit = (r, c) => {
    const stack = [[r, c]];
    while (stack.length) {
      const [x, y] = stack.pop();
      if (x < 0 || y < 0 || x >= rows || y >= cols || board[x][y] !== 'O') continue;
      board[x][y] = '#';
      stack.push([x-1,y], [x+1,y], [x,y-1], [x,y+1]);
    }
  };
  for (let r = 0; r < rows; r++) { visit(r,0); visit(r,cols-1); }
  for (let c = 0; c < cols; c++) { visit(0,c); visit(rows-1,c); }
  for (let r = 0; r < rows; r++) for (let c = 0; c < cols; c++)
    board[r][c] = board[r][c] === '#' ? 'O' : 'X';
}
```

## Complexity

O(rows × cols) time, up to O(rows × cols) stack space.

## Past mistakes and feedback

- **Observed:** Earlier approach tracked boundary-connected cells with encoded coordinates and Set; iteration/visited-marking needed debugging.
- **Confirmed insight:** For encoding `r*factor+c`, any `factor >= cols` guarantees uniqueness within a rectangular board; `factor=cols` suffices.
- **Improvement:** Mark safe cells in place as soon as visited; iterative stack avoids deep recursion on large grids.

## Points to improve

1. Explain why searching from the **border** is easier than testing each region for enclosure.
2. Test all-O, all-X, single-row, single-column and nested regions.
3. If using encoded IDs, prove uniqueness and decode rules; otherwise use `[row,col]` pairs.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original practice: 2026-10-19.
- **For follow-up:** Share [130. Surrounded Regions](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-0130-surrounded-regions.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
