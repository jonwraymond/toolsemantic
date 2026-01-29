# toolsemantic

Semantic indexing and retrieval for tools.

## Overview

toolsemantic provides interfaces for semantic search across tool metadata and
schemas. It is pluggable and backend-agnostic: **no network dependencies** and
no required vector database. Callers bring embeddings and storage.

## Design Goals

1. Pluggable strategies (BM25, embeddings, hybrid)
2. Deterministic scoring and ranking
3. Clear separation between indexing and query
4. Minimal dependencies
5. Compatibility with `toolindex` and `toolsearch`

## Position in the Stack

```
toolindex --> toolsemantic --> search backends
```

## Core Types

| Type | Purpose |
|------|---------|
| `Indexer` | Adds/updates tool documents |
| `Searcher` | Queries tools by semantic similarity |
| `Embedder` | Generates embeddings for tool docs |
| `Strategy` | Combines BM25 + vector scores |

## Quick Start

```go
index := toolsemantic.NewIndex()
index.Add(toolDoc)

results, _ := toolsemantic.NewSearcher(index).
    Query(ctx, "find weather tools")
```

## Versioning

toolsemantic follows semantic versioning aligned with the stack. The source of
truth is `ai-tools-stack/go.mod`, and `VERSIONS.md` is synchronized across repos.

## Next Steps

- [Design Notes](design-notes.md)
- [User Journey](user-journey.md)
- [Plans](plans/README.md)
