# cutePy Compiler

A fully functional compiler for **cutePy** — a statically-scoped, Python-like programming language — built from scratch in Python as part of a university Compilers course.

The compiler translates `.cpy` source files through all classic compilation stages: **lexical analysis → syntax analysis → intermediate code generation → final code generation (RISC-V assembly)**.

---

## Features

- **Lexer** — tokenizes cutePy source into tokens (identifiers, keywords, operators, literals)
- **Recursive Descent Parser** — validates syntax against the cutePy grammar
- **Symbol Table** — manages scopes, variable offsets, and function metadata with nesting levels
- **Intermediate Code Generator** — produces a sequence of quadruples (4-address code)
- **RISC-V Code Generator** — translates quadruples into RISC-V assembly instructions

---

## cutePy Language

cutePy is a simplified, Python-inspired language with:

- Integer variables and arithmetic (`+`, `-`, `*`, `//`)
- Relational operators (`==`, `!=`, `<`, `>`, `<=`, `>=`)
- Logical operators (`and`, `or`, `not`)
- Control flow: `if/else`, `while`
- Functions with parameters (pass-by-value and pass-by-reference)
- `print()` and `int(input())` for I/O
- Block delimiters: `#{` and `#}`
- Inline comments: `#$ ... #$`
- Entry point: `if __name__ == "__main__":`

### Example cutePy Program

```python
def main_program():
    #{
    # declare x, y, result
    x = int(input());
    y = int(input());
    if (x > y):
    #{
        result = x;
    #}
    else:
    #{
        result = y;
    #}
    print(result);
    #}

if __name__ == "__main__":
    main_program();
```

---

## Compilation Pipeline

```
Source (.cpy)
     │
     ▼
  Lexer (lex)
     │  Tokens
     ▼
  Parser (recursive descent)
     │  AST traversal
     ▼
  Intermediate Code Generator
     │  Quadruples (.int)
     ▼
  Final Code Generator
     │  RISC-V Assembly (.asm)
     ▼
  Output Files
```

---

## Output Files

| File | Description |
|------|-------------|
| `intermediate code.int` | Quadruples — 4-address intermediate representation |
| `final code.asm` | RISC-V assembly — ready to run on a RISC-V simulator |

### Sample Intermediate Code (Quadruples)
```
0 begin_block main_program _ _
1 inp x _ _
2 inp y _ _
3 > x y 5
4 jump _ _ 7
5 = x _ result
6 jump _ _ 8
7 = y _ result
8 out result _ _
9 halt _ _ _
```

---

## How to Run

### Requirements
- Python 3.x

### Usage
```bash
python cutePy_final.py <source_file>.cpy
```

### Example
```bash
python cutePy_final.py program.cpy
```

This produces:
- `intermediate code.int` — the quadruple representation
- `final code.asm` — the RISC-V assembly output

---

## Symbol Table

The compiler maintains a **scoped symbol table** that tracks:
- **Variables** — name, data type, memory offset
- **Temporary variables** — auto-generated during expression evaluation
- **Parameters** — pass-by-value (`CV`) and pass-by-reference (`ret`)
- **Functions** — name, starting quad, frame length, nesting level

Scoping uses **static nesting levels** with access links for non-local variable resolution (`gnvlcode`).

---

## RISC-V Code Generation

The final code generator targets **RISC-V RV32I** and handles:
- Arithmetic and logical operations
- Conditional branches (`beq`, `bne`, `bge`, `bgt`, `ble`, `blt`)
- Function calls with activation record management (stack frames)
- Parameter passing via stack
- Return values via dedicated register

---

## Project Structure

```
Compilers/
└── cutePy_final.py    # Complete compiler implementation (~1500 lines)
```

---

## Academic Context

Built as a university project for the **Compilers** course at the
**Department of Computer Engineering, University of Ioannina, Greece**.

Key concepts implemented:
- Finite automaton-based lexical analysis
- LL(1) grammar parsing using recursive descent
- Backpatching for control flow (if/else, while)
- Activation record design and stack frame management
- Non-local variable access via static chain (access links)
