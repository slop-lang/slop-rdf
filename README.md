# slop-rdf

An RDF (Resource Description Framework) library written in [Slop](https://github.com/slop-lang/slop), providing type-safe, contract-first implementations of core RDF data structures and operations.

## Overview

slop-rdf compiles to a static C library (`slop_rdf.a`) for integration with C/C++ projects. It provides:

- **Core RDF types**: IRIs, blank nodes, literals, triples, and graphs
- **Indexed graphs**: Efficient triple indexing with SPO/PSO/OSP access patterns
- **RDF list utilities**: Traversal of `rdf:first`/`rdf:rest` chains
- **Standard vocabularies**: Pre-defined IRIs for RDF, RDFS, XSD, OWL, Dublin Core, SKOS, FOAF, Schema.org, and more
- **XSD datatype support**: Parsing and value-space equality for common datatypes

## Modules

| Module | Description |
|--------|-------------|
| `rdf.slop` | Core types (IRI, BlankNode, Literal, Term, Triple, Graph) and operations |
| `index.slop` | Indexed graph implementation with O(1) lookups |
| `list.slop` | RDF list traversal utilities |
| `vocab.slop` | Standard vocabulary constants |
| `xsd.slop` | XSD datatype parsing and comparison |

## Building

Requires the [Slop compiler](https://github.com/slop-lang/slop).

```bash
# Build the static library
slop build

# Run tests
slop test

# Verify contracts
slop verify
```

Output is written to `build/`:
- `slop_rdf.a` - Static library
- `slop_rdf.h` - Main header
- `slop_vocab.h`, `slop_xsd.h`, `slop_index.h`, `slop_list.h` - Module headers

## Usage

Link against `slop_rdf.a` and include the generated headers:

```c
#include "slop_rdf.h"
#include "slop_vocab.h"

// Create terms
IRI subject = make_iri("http://example.org/subject");
IRI predicate = RDF_TYPE;  // from vocab
IRI object = make_iri("http://example.org/Thing");

// Create a triple
Triple t = make_triple(
    term_from_iri(subject),
    term_from_iri(predicate),
    term_from_iri(object)
);

// Create a graph and add the triple
Graph g = make_graph();
graph_add(&g, t);
```

## License

Apache-2.0. See [LICENSE](LICENSE) for details.
