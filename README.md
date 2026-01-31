# toolsemantic

> **DEPRECATED**: This repository has been merged into [tooldiscovery](https://github.com/jonwraymond/tooldiscovery).
>
> Please use `github.com/jonwraymond/tooldiscovery/semantic` instead.

## Migration

See [MIGRATION.md](./MIGRATION.md) for import path changes and migration instructions.

## Archive Notice

This repository is archived and will no longer receive updates. All semantic indexing and retrieval functionality is now available in the `tooldiscovery/semantic` package, which provides:

- Semantic indexing and retrieval for tools
- Pluggable semantic backends (BM25, vector, hybrid)
- Deterministic scoring and ranking
- Clear separation between indexing and querying
- Full compatibility with `toolindex`

## New Installation

```bash
go get github.com/jonwraymond/tooldiscovery/semantic
```

## Quick Start (New Package)

```go
package main

import (
    "context"
    "fmt"

    "github.com/jonwraymond/tooldiscovery/semantic"
)

func main() {
    idx := semantic.NewInMemoryIndex()
    _ = idx.Add(context.Background(), semantic.Document{
        ID:          "docs:summarize",
        Namespace:   "docs",
        Name:        "summarize",
        Description: "Summarize a document",
        Tags:        []string{"summarize", "read"},
    })

    strategy := semantic.NewBM25Strategy(nil)
    searcher := semantic.NewSearcher(idx, strategy)
    results, _ := searcher.Search(context.Background(), "summarize documents")

    for _, r := range results {
        fmt.Println(r.Document.ID, r.Score)
    }
}
```
