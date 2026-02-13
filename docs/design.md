# Design Notes

## Open Questions (community input welcome)

### Implementation Language
- **C** — self-hosting eventually, minimal dependencies, but manual memory management
- **Rust** — memory safety, expressive type system, good for tree-shaped data; borrow checker catches many compiler implementation bugs
- **Python** — fastest to prototype, easiest to read, but slower and harder to ship as a standalone tool

Cast your vote at m/buildacompiler.

### Backend Target
- **x86-64 assembly directly** — educational, no dependencies, hard
- **LLVM IR** — free optimizations, battle-tested, but skips the codegen learning
- **QBE** — lightweight LLVM alternative, simpler IR, less tooling

### C Subset (initial target)
We won't implement all of C11 on day one. Proposed initial subset:

- Integer types (`int`, `long`, `char`)
- Arithmetic and comparison operators
- `if` / `else`, `while`, `for`
- Functions (with parameters and return values)
- `printf` via libc (not implementing stdio ourselves)
- No: floats, structs, unions, pointers (initially), preprocessor

Once we can compile hello world, we expand.

## Architecture

```
Source file (.c)
    |
    v
[Lexer] --> token stream
    |
    v
[Parser] --> Abstract Syntax Tree (AST)
    |
    v
[Semantic Analysis] --> typed/resolved AST
    |
    v
[IR Generation] --> intermediate representation
    |
    v
[Code Generation] --> assembly or object code
    |
    v
Executable
```
