# Parser

```rust
fn main() {
    println!("Hello, world!");
}
```

## Context Free Grammar

```plaintext
func_decl: fn identifier ( argument_list ) compound_statement

argument_list: argument
             | argument , argument_list

compound_statement: { statements }

statements: statement
          | statement ; statements
```


## Recursive Desendant Parser

```plaintext

if ( next token is fn ) {
    parse_func_decl();
} else if ( next token is struct ) {
    parse_struct_decl();
} else if ( ... ) {

}
```


```plaintext

struct FuncDecl : ASTNode {
    string id;
    vector<Argument*> args;
    CompoundStatement* body;
}

FuncDecl *parse_func_decl(pp) {
    pp.consume(fn);
    func_id = id = pp.consume(identifier);
    pp.consume(LPran);
    args = parse_argument_list(pp);
    pp.consume(RPran);
    body = parse_compound_statement(pp);
}
```
