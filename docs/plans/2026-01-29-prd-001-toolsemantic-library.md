# PRD-001: toolsemantic Library Implementation

> **For agents:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a semantic search library for tools with pluggable indexing and
retrieval strategies (BM25, embeddings, hybrid).

**Architecture:** Provide a document model, indexing interface, search interface,
and composable scoring strategies. No vector database dependency is required.

**Tech Stack:** Go 1.24+, optional integration with `toolsearch` (BM25).

**Priority:** P2 (Phase 4 in the plan-of-record)

---

## Context and Stack Alignment

toolsemantic extends discovery by enabling semantic retrieval over tool metadata
and schema summaries. It complements `toolsearch` and consumes tool documents
from `toolindex`.

---

## Requirements

### Functional

1. Document model for tool metadata.
2. Indexer interface with add/update/remove.
3. Searcher interface with deterministic ranking.
4. Strategy composition: BM25-only, embedding-only, hybrid.
5. Filters by namespace/tags/category.

### Non-functional

- Deterministic ordering with explicit tie-breakers.
- Backend-agnostic (no vector DB dependency).
- Thread-safe for concurrent queries.

---

## Document Model

```go
// Document describes a tool for semantic indexing.
type Document struct {
    ID          string
    Namespace   string
    Name        string
    Description string
    Tags        []string
    Category    string
    Text        string // normalized combined text
}
```

Normalization rules:
- `Text` is built from name + description + tags.
- Tags are lowercased, sorted for stability.

---

## Scoring Rules

- BM25 score uses `toolsearch` (if configured).
- Embedding similarity uses cosine similarity.
- Hybrid score = `alpha * bm25 + (1 - alpha) * cosine`.
- Tie-breaker: lexicographic `Document.ID`.

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

## TDD Task Breakdown (Detailed)

### Task 1 — Document Model

**Files:** `document.go`, `document_test.go`

**Tests:**
- `TestDocument_NormalizeText`
- `TestDocument_TagsSorted`

**Commit:** `feat(toolsemantic): add document model`

---

### Task 2 — Indexer

**Files:** `indexer.go`, `indexer_test.go`

**Tests:**
- `TestIndexer_AddUpdateRemove`
- `TestIndexer_DedupByID`

**Commit:** `feat(toolsemantic): add in-memory indexer`

---

### Task 3 — Searcher

**Files:** `searcher.go`, `searcher_test.go`

**Tests:**
- `TestSearcher_DeterministicOrdering`
- `TestSearcher_TieBreakByID`

**Commit:** `feat(toolsemantic): add searcher interface`

---

### Task 4 — Strategy Composition

**Files:** `strategy.go`, `strategy_test.go`

**Tests:**
- `TestStrategy_BM25Only`
- `TestStrategy_EmbeddingOnly`
- `TestStrategy_HybridWeights`

**Commit:** `feat(toolsemantic): add scoring strategies`

---

### Task 5 — Filters

**Files:** `filter.go`, `filter_test.go`

**Tests:**
- `TestFilter_Namespace`
- `TestFilter_Tags`
- `TestFilter_Category`

**Commit:** `feat(toolsemantic): add filters`

---

### Task 6 — Docs + Examples

**Files:** `README.md`, `docs/index.md`, `docs/user-journey.md`

**Acceptance:** Mermaid diagram and quick start examples included. Add D2
component diagram in ai-tools-stack.

**Commit:** `docs(toolsemantic): finalize documentation`

---

## PR Process

1. Create branch: `feat/toolsemantic-<task>`
2. Implement TDD task in isolation
3. Run: `go test -race ./...`
4. Commit with scoped message
5. Open PR against `main`
6. Merge after CI green

---

## Versioning and Propagation

- **Source of truth:** `ai-tools-stack/go.mod`
- **Matrix:** `ai-tools-stack/VERSIONS.md` (auto-synced)
- **Propagation:** `ai-tools-stack/scripts/update-version-matrix.sh --apply`
- Tags: `vX.Y.Z` and `toolsemantic-vX.Y.Z`

---

## Integration with metatools-mcp

- Provide semantic search provider for tool discovery.
- Use `toolindex` as source of tool documents.
- Config allows BM25-only or hybrid strategy.

---

## Definition of Done

- All tasks complete with tests passing
- `go test -race ./...` succeeds
- Docs + diagrams updated in ai-tools-stack
- CI green
- Version matrix updated after first release
