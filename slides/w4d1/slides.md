---
marp: true
theme: default
paginate: true
size: 16:9
style: |
  section {
    padding: 30px 50px;
    font-size: 36px;
  }
  section::after {
    content: attr(data-marpit-pagination) ' / ' attr(data-marpit-pagination-total);
    font-size: 18px;
    color: #555;
    position: absolute;
    bottom: 30px;
    right: 50px;
  }
  header {
    font-size: 20px;
    color: #888;
  }
  h1 {
    color: #2c3e50;
  }
  h2 {
    color: #34495e;
    border-bottom: 2px solid #3498db;
    padding-bottom: 10px;
  }
  footer {
    display: none;
  }
  .columns { display: flex; gap: 40px; align-items: center; }
  .col { flex: 1; }
---

# Parser Implementation
## From Tokens to Structure

Jian Weng  
CEMSE, KAUST  
Week 4, Session 1

---

# Today's Story

1. We finished lexer: now we have tokens
2. Parser purpose: organize tokens into structure
3. Theory: context-free grammar / context-free language
4. Grammar is recursive expansion
5. Recursion maps naturally to functions
6. Algorithm: recursive descent parser
7. Engineering details: lookahead + dispatch
8. Checkpoint #2 assignment

---

# Recap: What Lexer Gives Us

```text
Input:
  fn add(x, y) { x + y; }

Lexer Output:
  FN IDENT LPAREN IDENT COMMA IDENT RPAREN LBRACE IDENT PLUS IDENT SEMI RBRACE
```

- Lexer solved tokenization
- But token list alone has no hierarchy

---

# Parser Purpose

- Parser consumes lexer tokens
- Parser builds a structured data model
- That model is the AST (Abstract Syntax Tree)
- AST is what semantic analysis and codegen actually need

```text
Tokens -> Parser -> AST
```

---

# Why Tokens Are Not Enough

```text
IDENT LPAREN IDENT COMMA IDENT RPAREN
```

- Is this function call?
- Is this declaration?
- Are arguments complete?
- Where is body / scope?

Parser resolves these structural questions.

---

# Formal Language View

- In theory, parser works on a formal language definition
- We describe syntax with CFG (Context-Free Grammar)
- The language described by CFG is CFL (Context-Free Language)

```text
Grammar (rules) defines legal programs
Parser checks and reconstructs those rules from tokens
```

---

# CFG Example for Declarations

```ebnf
func_decl          := "fn" identifier "(" argument_list? ")" compound_statement
argument_list      := argument ("," argument)*
compound_statement := "{" statement_list? "}"
statement_list     := statement (";" statement)*
```

- Non-terminals: `func_decl`, `argument_list`, ...
- Terminals: `fn`, `identifier`, `(`, `)`, `{`, `}`

---

# Grammar Is Recursive Expansion

```text
func_decl
  -> "fn" identifier "(" argument_list ")" compound_statement

argument_list
  -> argument
  -> argument "," argument_list
```

- A rule expands into smaller rules
- Some rules call themselves recursively

---

# Key Insight

Each recursive grammar rule can map to one function.

```text
func_decl       -> parse_func_decl()
argument_list   -> parse_argument_list()
statement_list  -> parse_statement_list()
```

This gives us the most intuitive parser implementation strategy.

---

# Algorithm: Recursive Descent

```text
For each non-terminal:
  write one parser function
```

- Top-down parsing
- Human-readable control flow
- Easy to debug in class and in assignment

---

# Parser State and API

```cpp
Token Parser::peek(int k = 0) const;
bool  Parser::match(TokenKind kind);
Token Parser::expect(TokenKind kind, const string& message);
```

- `peek`: lookahead without consuming
- `match`: consume if next token matches
- `expect`: enforce syntax rule or throw parser error

---

# Lookahead Tokens

- Lookahead means "inspect upcoming token(s) first"
- Usually one-token lookahead is enough for our grammar
- This is how parser decides which rule to enter

```text
if peek() == FN      -> parse_func_decl()
if peek() == STRUCT  -> parse_struct_decl()
```

---

# Rule Dispatch in Practice

```cpp
Decl* Parser::parse_decl() {
  if (peek().kind == FN) return parse_func_decl();
  if (peek().kind == STRUCT) return parse_struct_decl();
  error("expected declaration");
}
```

- Dispatch is the central switchboard of recursive descent

---

# Implement `parse_func_decl`

```cpp
FuncDecl* Parser::parse_func_decl() {
  expect(FN, "expected 'fn'");
  string id = expect(IDENT, "expected function name").lexeme;
  expect(LPAREN, "expected '('");
  auto args = parse_argument_list();
  expect(RPAREN, "expected ')'");
  auto body = parse_compound_statement();
  return new FuncDecl{id, args, body};
}
```

---

# Implement `parse_argument_list`

```cpp
vector<Argument*> Parser::parse_argument_list() {
  vector<Argument*> args;
  if (peek().kind == RPAREN) return args;
  args.push_back(parse_argument());
  while (match(COMMA)) args.push_back(parse_argument());
  return args;
}
```

- Zero args: immediate return
- N args: parse first, then loop on comma

---

# AST Node Produced by Parser

```cpp
struct FuncDecl : ASTNode {
  string id;
  vector<Argument*> args;
  CompoundStatement* body;
};
```

- Parser output is typed structure
- Later stages should not care about token cursor logic

---

# Error Handling

- Syntax errors must include token + location
- Continue when possible to find more errors
- Recovery points: `;`, `}`, next declaration keyword

```text
error: expected ')' after argument list at line 12, column 19
```

---

# End-to-End Parsing Flow

```text
parse_program()
  -> parse_decl()
       -> parse_func_decl()
            -> parse_argument_list()
            -> parse_compound_statement()
```

This is exactly grammar expansion, but implemented as function calls.

---

# Checkpoint #2 (Assignment)

Goal: implement parser stage for the language subset in this course.

- Input: token sequence from your lexer
- Output: AST for declarations and statements
- Method: recursive descent with lookahead dispatch

---

# Checkpoint #2 Requirements

1. Implement parser helper APIs (`peek`, `match`, `expect`)
2. Implement declaration dispatch (`parse_decl`)
3. Implement `parse_func_decl` and `parse_argument_list`
4. Construct AST nodes correctly
5. Add parser tests for valid and invalid inputs

---

# Submission Checklist

- Parser code compiles and runs
- Tests include failure cases
- Error messages are readable
- One short note: what grammar subset you support

Bring questions next session and we will debug real parser failures together.
