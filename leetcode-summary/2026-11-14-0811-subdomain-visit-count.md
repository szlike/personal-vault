# 811. Subdomain Visit Count

**Recorded:** 2026-11-14  
**Original practice:** 2026-10-19

## Pattern

For each `count domain`, split the domain by dots and add count to the full domain and all suffixes.

```js
function subdomainVisits(cpdomains) {
  const totals = new Map();
  for (const entry of cpdomains) {
    const [countText, domain] = entry.split(' ');
    const count = Number(countText);
    const parts = domain.split('.');
    for (let i = 0; i < parts.length; i++) {
      const suffix = parts.slice(i).join('.');
      totals.set(suffix, (totals.get(suffix) ?? 0) + count);
    }
  }
  return [...totals].map(([domain,count]) => `${count} ${domain}`);
}
```

## Complexity

If each domain has `d` labels and length `L`, generating all suffix strings costs up to O(d×L) per entry (or O(d²) in label-length abstraction); space is proportional to distinct suffix text. Clarity is more important than overoptimizing tiny domain depths.

## Past mistakes and feedback

- **Observed:** The earlier implementation used more complex split/join handling and an unused parameter.
- **Feedback:** Prefer `parts.slice(i).join('.')`, simplify Map accumulation, remove unnecessary parameters and helpers.

## Points to improve

1. Trace `9001 discuss.leetcode.com` into three suffixes.
2. Verify repeated suffixes accumulate counts.
3. Prefer straightforward code over optimizing before understanding the constraint.

