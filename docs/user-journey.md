# User Journey: Semantic Tool Search

## Scenario

You want to retrieve tools by intent rather than exact name matching. The query
"summarize meeting notes" should return tools tagged for summarization.

## Step 1: Build a semantic index

```go
idx := toolsemantic.NewInMemoryIndex()
_ = idx.Add(ctx, toolsemantic.Document{
    ID:          "docs:summarize",
    Namespace:   "docs",
    Name:        "summarize",
    Description: "Summarize a document",
    Tags:        []string{"summarize", "read"},
})
```

## Step 2: Query the index

```go
strategy := toolsemantic.NewBM25Strategy(nil)
searcher := toolsemantic.NewSearcher(idx, strategy)
results, _ := searcher.Search(ctx, "summarize meeting notes")
```

## Step 3: Filter results

```go
filtered := make([]toolsemantic.Result, 0, len(results))
for _, r := range results {
    if r.Document.Namespace == "docs" {
        filtered = append(filtered, r)
    }
}
```

## Flow Diagram

```mermaid
flowchart LR
  A[Query Text] --> B[Embedder]
  B --> C[Vector Query]
  C --> D[Score + Rank]
  D --> E[Filter/Sort]
  E --> F[Results]
```
