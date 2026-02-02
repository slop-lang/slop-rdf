# slop-rdf

An RDF (Resource Description Framework) library written in [Slop](https://github.com/slop-lang/slop), providing type-safe, contract-first implementations of core RDF data structures and operations.

## Overview

slop-rdf compiles to a static C library (`slop_rdf.a`) for integration with C/C++ projects. It provides:

- **Core RDF types**: IRIs, blank nodes, literals, triples, and graphs
- **Indexed graphs**: Efficient triple indexing with SPO/PSO/OSP access patterns
- **RDF list utilities**: Traversal of `rdf:first`/`rdf:rest` chains
- **Standard vocabularies**: Pre-defined IRIs for RDF, RDFS, XSD, OWL, Dublin Core, SKOS, FOAF, Schema.org, and more
- **XSD datatype support**: Parsing and value-space equality for common datatypes
- **Turtle parser/serializer**: Read and write RDF in Turtle syntax
- **N3 parser/serializer**: Read and write RDF in Notation3 syntax

## Modules

| Module | Description |
|--------|-------------|
| `rdf.slop` | Core types (IRI, BlankNode, Literal, Term, Triple, Graph) and operations |
| `index.slop` | Indexed graph implementation with O(1) lookups |
| `list.slop` | RDF list traversal utilities |
| `vocab.slop` | Standard vocabulary constants |
| `xsd.slop` | XSD datatype parsing and comparison |
| `parse/common.slop` | Shared parsing utilities |
| `parse/ttl.slop` | Turtle format parser |
| `parse/n3.slop` | N3 format parser |
| `serialize/serialize-ttl.slop` | Turtle format serializer |
| `serialize/serialize-n3.slop` | N3 format serializer |

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
- `slop_parse_ttl.h`, `slop_parse_n3.h` - Parser headers
- `slop_serialize_ttl.h`, `slop_serialize_n3.h` - Serializer headers

## Usage

### Slop

```slop
(module my-app
  (import rdf (Graph make-iri make-triple make-graph graph-add))
  (import ttl (parse-ttl-string))
  (import serialize-ttl (SerializeConfig serialize-ttl-string))
  (import ttl (PrefixMap make-prefix-map prefix-map-add))

  (fn main ()
    (@spec (() -> Int))
    (with-arena 65536

      ;; Parse a Turtle string into a graph
      (let ((result (parse-ttl-string arena
                      "@prefix ex: <http://example.org/> .\nex:s ex:p ex:o .")))
        (match result
          ((ok g)
            ;; Serialize the graph back to Turtle
            (let ((pm (make-prefix-map arena))
                  (pm (prefix-map-add arena pm "ex" "http://example.org/"))
                  (config (record-new SerializeConfig
                            (prefixes pm)
                            (base-iri (none))
                            (indent-width 4))))
              (println (serialize-ttl-string arena g config))))
          ((error e)
            (println "Parse error"))))

      ;; Build a graph programmatically
      (let ((g (graph-add arena (make-graph arena)
                 (make-triple arena
                   (make-iri arena "http://example.org/s")
                   (make-iri arena "http://example.org/p")
                   (make-iri arena "http://example.org/o")))))
        0))))
```

### C (generated API)

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

#### Parsing Turtle

```c
#include "slop_parse_ttl.h"

// Parse a Turtle string into a graph
Graph g = ttl_parse_string(
    "@prefix ex: <http://example.org/> .\n"
    "ex:subject a ex:Thing .\n"
);
```

#### Serializing Turtle

```c
#include "slop_serialize_ttl.h"

// Serialize a graph to Turtle format
char* output = ttl_serialize(&g);
```

## License

Apache-2.0. See [LICENSE](LICENSE) for details.
