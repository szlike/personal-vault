# Java OOP, Collections, HashMap, and Equality

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Original preparation:** 2026-09-11; reviewed 2026-10-29

## OOP quick answers

- **Interface vs abstract class:** interface defines a contract (and can include default/static methods); abstract class can share state, constructors and common implementation. Choose based on abstraction/ownership rather than syntax alone.
- **Overload vs override:** overload changes parameter signature and is resolved at compile time; override supplies runtime polymorphic implementation for inherited instance methods.
- **`==` vs `equals`:** object `==` checks reference identity; `equals` may check logical equality if implemented. For primitives, `==` compares values.

## HashMap internals

1. `hashCode()` determines a bucket after hash spreading/indexing.
2. Within the bucket, `equals()` selects the logically equal key.
3. Collisions are handled by nodes; heavily populated buckets can be treeified under suitable conditions.
4. Average `get`/`put` is O(1); a pathological collision chain is O(n), tree bucket operations can be O(log n).
5. Resizing typically happens after threshold `capacity × loadFactor` (commonly 0.75).

**Equality contract:** If `a.equals(b)` is true, both must have the same hash code. The converse is not required. Keys whose equality/hash changes after insertion are dangerous.

## Collections

| Collection | Strength | Cost/pitfall |
| --- | --- | --- |
| ArrayList | O(1) indexed access, compact locality | Middle inserts/deletes shift O(n) elements |
| LinkedList | O(1) link update once node known | Finding indexed node O(n), memory overhead |
| HashMap | Average O(1) lookup | Unordered; equality/hash contract; not thread-safe |
| PriorityQueue | O(log n) insert/remove top | Java default min-heap; cannot retrieve arbitrary sorted order in O(1) |

## Past mistakes and feedback

- **Observed:** On Sep 11, you requested concise memorization-ready explanations due time pressure; the HashMap and `equals`/`hashCode` contract were explicitly reviewed.
- **Knowledge to reinforce:** Java collections/memory/concurrency were comparatively weaker than frontend and Kafka in overall interview profile.
- **Potential trap:** `HashMap` lookup O(1) is average/expected, not a worst-case universal guarantee.

## Points to improve

1. Explain HashMap put/get with collision and a mutable key in 45 seconds.
2. Practice choosing Map vs List vs heap in real system examples.
3. State equality contract without saying “equal hash codes imply equal objects.”
4. Connect OOP choices to testability and maintainable service boundaries.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original preparation: 2026-09-11; reviewed 2026-10-29.
- **For follow-up:** Share [Java OOP, Collections, HashMap, and Equality](https://github.com/szlike/personal-vault/blob/main/technical-concepts/2026-11-14-java-oop-collections-and-hashmap.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
