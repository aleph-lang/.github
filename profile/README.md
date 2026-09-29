# AlephLang

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23021204.svg)](https://doi.org/10.5281/zenodo.23021204)

**A shared intermediate representation for cross-language transpilation — and a language layer designed for how LLMs actually read, write, and repair code.**

One architectural bet, applied twice. Most programming languages share a common core — bindings, conditionals, loops, functions, arithmetic, data structures. AlephLang captures that core in a single tree representation, the **AlephTree**, and builds on top of it wherever the leverage is highest.

## Part I — Cross-language transpilation

Translating between N languages point-to-point needs N×M translators. Routing everything through one shared IR needs N parsers and M generators instead:

```
Without AlephTree:        With AlephTree:

Java ──── Python          Java  ──► AlephTree ──► Python
Java ──── Erlang          COBOL ──► AlephTree ──► Erlang
COBOL ─── Python          Ada   ──► AlephTree ──► Gleam
COBOL ─── Erlang          ...
...
N × M components          N + M components
```

Seven parsers (Aleph, Python, JavaScript, COBOL, Ada, Forth, PL/I) and four generators (Python, Erlang, Elixir, Gleam) already exist. For constructs with no clean structural equivalent between two languages, an LLM-assisted path (Ollama, Claude, Mistral, OpenAI, OpenWebUI — same interface, swappable backends) fills the gap.

## Part II — Aleph-Next, a language for AI comprehension

The obvious instinct is that a language built for a model rather than a human could shed everything humans need — keywords, whitespace, familiar syntax — for raw token efficiency. Measurement says otherwise: unfamiliar syntax costs *more* tokens than dense syntax saves, because models fight unfamiliar grammar instead of solving the problem. The properties that actually help a model — unambiguous scope and typing, precise structured errors, no whitespace-sensitive layout — are the same properties that help a human reader.

**Aleph-Next** extends the AlephTree (not a fork — the same enum every Part I tool already consumes) with:
- **Gradual typing** — a typed Aleph-Next function calls, and is called by, permanently-untyped AlephTree from any of the other parsers
- **Bidirectional type checking + a separate effect system** (`pure`, `io`, `net`, `mut`, `act`), catching hidden side effects no type alone expresses
- **Content-addressed diagnostics** — every node carries a deterministic BLAKE3 hash; errors are located by hash, not by line and column, so they survive reformatting and are directly addressable by an automated repair loop
- **Exhaustiveness-checked pattern matching** with constructor patterns and wildcards

This isn't speculative — `aleph-syntax-tree`, `aleparser`, `alegen`, and `alephcheck` (a 75-test type/effect/exhaustiveness checker) ship it today.

Full argument, related work, and worked examples: **[read the paper](https://doi.org/10.5281/zenodo.23021204)** (Zenodo, DOI `10.5281/zenodo.23021204`).

## Ecosystem

### Core
- [**aleph**](https://github.com/aleph-lang/aleph) — the compiler/transpiler (Alephc): wires parsers and generators together
- [**aleph-syntax-tree**](https://github.com/aleph-lang/aleph-syntax-tree) — the AlephTree data structure shared by every parser and generator
- **alephcheck** — Aleph-Next's type/effect/exhaustiveness checker (in development, not yet published)
- [**aleparser**](https://github.com/aleph-lang/aleparser) / [**alegen**](https://github.com/aleph-lang/alegen) — parser and generator for the Aleph language itself

### Parsers (source → AlephTree)
[pythonparser](https://github.com/aleph-lang/pythonparser) · [jsparser](https://github.com/aleph-lang/jsparser) · [cobol_parser](https://github.com/aleph-lang/cobol_parser) · [adaparser](https://github.com/aleph-lang/adaparser) · [forthparser](https://github.com/aleph-lang/forthparser) · [pliparser](https://github.com/aleph-lang/pliparser)

### Generators (AlephTree → target)
[pythongen](https://github.com/aleph-lang/pythongen) · [erlanggen](https://github.com/aleph-lang/erlanggen) · [elixirgen](https://github.com/aleph-lang/elixirgen) · [gleamgen](https://github.com/aleph-lang/gleamgen)

### Transforms
[betareduction](https://github.com/aleph-lang/betareduction) · [constant_folding](https://github.com/aleph-lang/constant_folding)

### LLM-assisted translation
For constructs with no clean structural equivalent, swap in an LLM backend behind the same interface: [aleph_ollama](https://github.com/aleph-lang/aleph_ollama) · [aleph_claude](https://github.com/aleph-lang/aleph_claude) · [aleph_mistral](https://github.com/aleph-lang/aleph_mistral) · [aleph_openai](https://github.com/aleph-lang/aleph_openai) · [aleph_openwebui](https://github.com/aleph-lang/aleph_openwebui) · [aleph_web_translator](https://github.com/aleph-lang/aleph_web_translator) / [aleph_web_client](https://github.com/aleph-lang/aleph_web_client)

### Applications
[aleph2py](https://github.com/aleph-lang/aleph2py) — example transpiler built on AlephTree

## The road ahead

Parser coverage for Java, C, Rust, OCaml; generator coverage for TypeScript, Haskell, WebAssembly; generics and native `Option`/`Result` in Aleph-Next's type system; a GBNF export of the Aleph-Next grammar for grammar-constrained LLM decoding; an LSP server. Parsers, generators, transformers, and EmergenceSystem filters are all modular additions requiring no change to existing components — contributions welcome.

## Citing AlephLang

```bibtex
@misc{roques2026alephlang,
  title  = {AlephLang: A Shared Intermediate Representation for Cross-Language Transpilation and AI-Native Code},
  author = {Roques, Steve and Suzan, J{\'e}r{\'e}mie},
  year   = {2026},
  doi    = {10.5281/zenodo.23021204},
  url    = {https://doi.org/10.5281/zenodo.23021204}
}
```
