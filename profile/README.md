# AlephLang

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23021204.svg)](https://doi.org/10.5281/zenodo.23021204)

A shared intermediate representation for cross-language transpilation and AI-native code.

Most programming languages share a common core — bindings, conditionals, loops, functions, arithmetic, data structures. AlephLang captures that core in a single tree representation, the **AlephTree**, so that translating between N languages needs N parsers and N generators instead of N×M point-to-point translators.

```
Without AlephTree:        With AlephTree:

Java ──── Python          Java  ──► AlephTree ──► Python
Java ──── Erlang          COBOL ──► AlephTree ──► Erlang
COBOL ─── Python          Ada   ──► AlephTree ──► Gleam
COBOL ─── Erlang          ...
...
N × M components          N + M components
```

Read the paper: **[AlephLang: A Shared Intermediate Representation for Cross-Language Transpilation and AI-Native Code](https://doi.org/10.5281/zenodo.23021204)** (Zenodo, DOI `10.5281/zenodo.23021204`).

## Ecosystem

### Core
- [**aleph**](https://github.com/aleph-lang/aleph) — the compiler/transpiler (Alephc): wires parsers and generators together
- [**aleph-syntax-tree**](https://github.com/aleph-lang/aleph-syntax-tree) — the AlephTree data structure shared by every parser and generator
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
