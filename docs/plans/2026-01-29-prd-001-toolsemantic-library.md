# PRD-001: toolsemantic Library Implementation

> **For agents:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a semantic search library for tools that supports pluggable
indexing and retrieval strategies (BM25, embeddings, hybrid).

**Architecture:** Provide interfaces for indexing and searching tool documents,
with optional embedding generation. No hard dependency on a vector database.

**Tech Stack:** Go 1.24+, optional integration with `toolsearch` (BM25)

**Priority:** P2 (Phase 4 in the plan-of-record)

---

## Context and Stack Alignment

toolsemantic extends discovery by enabling semantic retrieval over tool metadata
and schema summaries. It complements `toolsearch` and can consume tool documents
from `toolindex`.

---

## Scope

### In scope
- Tool document model
- Indexer and Searcher interfaces
- Embedding interface (caller-provided)
- Hybrid ranking strategy (BM25 + embeddings)
- Filtering by namespace/tags/category
- Unit tests for deterministic ranking
- Docs and examples

### Out of scope
- Vector database implementation
- Embedding provider SDK integration
- Online training or fine-tuning

---

## Design Principles

1. **Backend agnostic**: avoid vendor-specific dependencies.
2. **Deterministic results**: stable ranking and tie-breakers.
3. **Composable scoring**: BM25, vector, hybrid.
4. **Minimal dependencies**: core Go only.
5. **Safe defaults**: small indexes must be fast and memory-safe.

---

## Directory Structure

```
toolsemantic/
├── document.go
├── document_test.go
├── indexer.go
├── indexer_test.go
├── searcher.go
├── searcher_test.go
├── strategy.go
├── strategy_test.go
├── filter.go
├── filter_test.go
├── doc.go
├── README.md
├── go.mod
└── go.sum
```

---

## API Shape (Conceptual)

```go
// Document describes a tool for semantic indexing.
type Document struct {
    ID          string
    Namespace   string
    Name        string
    Description string
    Tags        []string
}

// Embedder provides embeddings for text.
type Embedder interface {
    Embed(ctx context.Context, text string) ([]float32, error)
}
```

---

## Tasks (TDD)

### Task 1 — Document Model

- Define `Document` and normalization helpers
- Tests: deterministic normalization

### Task 2 — Indexer Interface

- Define indexer interface and in-memory index
- Tests: add/update/remove

### Task 3 — Searcher Interface

- Define searcher with scoring + ranking
- Tests: deterministic ordering and tie-breaking

### Task 4 — Strategy Composition

- BM25-only, embeddings-only, hybrid strategy
- Tests: correct weighting behavior

### Task 5 — Filters

- Namespace/tag/category filters
- Tests: filter correctness

### Task 6 — Docs + Examples

- Update README and docs/index.md
- Add Mermaid flow diagram
- Add D2 component diagram in ai-tools-stack

---

## Versioning and Propagation

- **Source of truth**: `ai-tools-stack/go.mod`
- **Version matrix**: `ai-tools-stack/VERSIONS.md` (auto-synced)
- **Propagation**: `ai-tools-stack/scripts/update-version-matrix.sh --apply`
- Tags: `vX.Y.Z` and `toolsemantic-vX.Y.Z`

---

## Integration with metatools-mcp

- Provide semantic search provider for tool discovery.
- Use `toolindex` as a source of tool documents.
- Configuration should allow choosing BM25 vs semantic strategy.

---

## Definition of Done

- All TDD tasks complete with tests passing
- `go test -race ./...` succeeds
- Docs include quick start + diagrams
- CI green
- Version matrix updated after first release
