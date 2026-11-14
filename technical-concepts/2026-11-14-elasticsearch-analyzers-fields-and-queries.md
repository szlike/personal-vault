# Elasticsearch — Analyzer, Tokenizer, text/keyword, match/term

**Recorded:** 2026-11-14  
**Related interview preparation:** September 2026

## Analyzer vs tokenizer

An analyzer is a **pipeline** (character filters → tokenizer → token filters). A tokenizer breaks a string into tokens; filters can lowercase, stem, remove stopwords, etc. Index-time and search-time analyzers may differ, which can explain unexpected matches.

## `text` vs `keyword`

- **`text`:** analyzed full-text content; supports relevance-oriented queries. Usually not suitable for exact aggregations/sorts without a keyword subfield.
- **`keyword`:** not full-text analyzed; suitable for IDs, symbols, exact filters, sorting and aggregations (normalizers may transform case).
- Common pattern: a `text` field with a `.keyword` multi-field for exact matching.

## `match` vs `term`

- **`match`:** analyzes query text according to field mapping; intended for full-text search.
- **`term`:** matches an exact indexed term; intended for identifiers, states, codes, and keyword fields. `term` on analyzed text frequently surprises because the indexed words differ from original full string.

```json
{ "query": { "match": { "description": "trading platform" } } }
```

```json
{ "query": { "term": { "status.keyword": "OPEN" } } }
```

Check actual mappings before using `.keyword`; a native keyword field is queried by its own name.

## Migration connection

An Elasticsearch 7→8 migration must check mapping templates, analyzer compatibility, client query syntax, remote reindex behavior and aliases. Search can silently change if analysis/mappings differ even when documents copy successfully.

## Past mistakes and feedback

- **Observed learning need:** You specifically asked to revisit analyzer vs tokenizer and the exact-match vs full-text search terminology.
- **Observed recall gap:** You remembered “keyword vs match” as if they were comparable settings; correct distinction is **field types** (`text`/`keyword`) versus **query types** (`match`/`term`).
- **Potential trap:** `match` on a keyword field does not magically provide full-text analysis; behavior depends on the mapping.

## Points to improve

1. Use `_analyze` to predict tokens for an input string.
2. Draw index-time analysis vs query-time matching with concrete terms.
3. Explain your real ES use: filtering, aggregations and migration validation.
4. Practice `match`, `term`, `terms`, `range`, `bool/filter` with a small sample mapping.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Related interview preparation: September 2026.
- **For follow-up:** Share [Elasticsearch — Analyzer, Tokenizer, text/keyword, match/term](https://github.com/szlike/personal-vault/blob/main/technical-concepts/2026-11-14-elasticsearch-analyzers-fields-and-queries.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
