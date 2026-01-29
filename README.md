# toolsemantic

Semantic indexing and retrieval for tools.

## Overview

toolsemantic provides interfaces and helpers for semantic search across tool
metadata. It is a pluggable library: **no network dependencies** and no hard
binding to a vector database. Callers provide embedding and storage backends.

## Design Goals

1. Pluggable semantic backends (BM25, vector, hybrid)
2. Deterministic scoring and ranking
3. Clear separation between indexing and querying
4. Minimal dependencies
5. Compatibility with `toolsearch` and `toolindex`

## Position in the Stack

```
toolindex --> toolsemantic --> search backends
```

## Versioning

toolsemantic follows semantic versioning aligned with the stack. The source of
truth is `ai-tools-stack/go.mod`, and `VERSIONS.md` is synchronized across repos.

## Next Steps

- See `docs/index.md` for usage and design notes.
- PRD and execution plan live in `docs/plans/`.
