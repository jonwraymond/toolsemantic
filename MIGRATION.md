# Migration Guide: toolsemantic to tooldiscovery/semantic

This document provides instructions for migrating from `github.com/jonwraymond/toolsemantic` to `github.com/jonwraymond/tooldiscovery/semantic`.

## Import Path Changes

Update your import statements as follows:

| Old Import | New Import |
|------------|------------|
| `github.com/jonwraymond/toolsemantic` | `github.com/jonwraymond/tooldiscovery/semantic` |

## Migration Steps

### 1. Update go.mod

Remove the old dependency and add the new one:

```bash
go get github.com/jonwraymond/tooldiscovery/semantic
go mod tidy
```

### 2. Update Import Statements

Replace all imports in your codebase:

**Before:**
```go
import "github.com/jonwraymond/toolsemantic"
```

**After:**
```go
import "github.com/jonwraymond/tooldiscovery/semantic"
```

### 3. Update Package References

Replace package references in your code:

**Before:**
```go
idx := toolsemantic.NewInMemoryIndex()
doc := toolsemantic.Document{...}
strategy := toolsemantic.NewBM25Strategy(nil)
searcher := toolsemantic.NewSearcher(idx, strategy)
```

**After:**
```go
idx := semantic.NewInMemoryIndex()
doc := semantic.Document{...}
strategy := semantic.NewBM25Strategy(nil)
searcher := semantic.NewSearcher(idx, strategy)
```

## Automated Migration

You can use `sed` or `gofmt` to automate the import replacement:

```bash
# Replace imports across your codebase
find . -name "*.go" -exec sed -i '' \
    's|github.com/jonwraymond/toolsemantic|github.com/jonwraymond/tooldiscovery/semantic|g' {} +

# Run goimports to fix package references
goimports -w .

# Tidy dependencies
go mod tidy
```

## API Compatibility

The `tooldiscovery/semantic` package maintains full API compatibility with `toolsemantic`. All types, functions, and interfaces have been preserved:

- `Document` - Document struct for indexing
- `Index` - Index interface
- `NewInMemoryIndex()` - In-memory index constructor
- `Strategy` - Search strategy interface
- `NewBM25Strategy()` - BM25 strategy constructor
- `Searcher` - Search executor
- `NewSearcher()` - Searcher constructor
- `Result` - Search result struct

## Questions or Issues

If you encounter issues during migration, please open an issue in the [tooldiscovery repository](https://github.com/jonwraymond/tooldiscovery/issues).
