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
  .columns { display: flex; gap: 40px; align-items: flex-start; }
  .col { flex: 1; }
  pre { font-size: 20px; }
  code { font-size: 20px; }
---

# correctness-oriented language features
## Ownership, Borrowing, and Conservative Defaults

Jian Weng  
CEMSE, KAUST  
Week 11, Session 2

---

# Today's Agenda

1. Why this matters in AI-assisted compiler work
2. Ownership and borrowing from C++ to Rust
3. Compile-time protection for iterator + mutation bugs
4. Conservative language defaults for correctness
5. Practical implications for your compiler code

---

# Why This Topic Now?

Many of you already ask AI to generate Rust compiler code in C++.

That is productive, but language features still decide correctness.

- ownership decides lifetime and mutation permissions
- borrowing decides aliasing and API shape
- conservative defaults remove classes of bugs early

This session is about those guarantees, not syntax trivia.

---

# What Does Ownership Change?

```cpp
void foo(int x) {
  // do something
}

struct Foo {
  int a[100];
};

void bar(Foo x) {
  // do something
}
```

- `foo(5)` is cheap
- `bar(f)` copies a large object
- value semantics can be expensive without moves/borrows

---

# C++: Borrow Instead of Copy

```cpp
void bar(const Foo& x) {
  // no copy
}

void baz(Foo* x) {
  // no copy
}
```

- both avoid large object copies
- reference: non-null, bound once
- pointer: nullable and re-bindable

---

# C++ Move Semantics (Rust-Like)

```cpp
void bar(Foo x) {
  // ownership-like parameter
}

int main() {
  Foo f;
  bar(std::move(f));
}
```

- `std::move` enables moving from `f`
- resource ownership transfers into `bar`
- this resembles Rust move semantics closely

---

# Rust: Move by Default

```rust
fn bar(x: Foo) {
    // x owns the value
}

fn main() {
    let f = Foo::new();
    bar(f);
    // f is no longer usable here
}
```

- ownership is moved into `bar`
- use-after-move is rejected at compile time
- no runtime check required

---

# Rust: Borrow When You Still Need `f`

```rust
fn bar(x: &Foo) {
    // borrowed read-only access
}

fn main() {
    let f = Foo::new();
    bar(&f);
    // f is still valid here
}
```

- `&Foo` is an immutable borrow
- ownership is not transferred
- after the borrow ends, `f` remains available

---

# Beyond Copy Cost: Iteration + Mutation Hazard

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};
for (auto& x : v) {
  v.push_back(x);
}
```

This can break in subtle ways.

- `push_back` may reallocate storage
- references/iterators can dangle
- behavior becomes undefined

---

# Why This Breaks

```cpp
auto iter = v.data();
auto end = v.data() + v.size();
while (iter != end) {
  v.push_back(*iter);
  ++iter;
}
```

If vector capacity grows:

1. new buffer is allocated
2. old elements are moved/copied
3. old buffer is released

Then `iter` / `end` may still point to old memory.

---

# Rust Rejects It Early

```rust
let mut v = vec![1, 2, 3, 4, 5];
for x in v.iter() {
    v.push(*x); // compile error
}
```

- `v.iter()` creates an immutable borrow
- `v.push(...)` needs a mutable borrow
- Rust forbids simultaneous mutable + immutable borrow

---

# Other Correctness-Oriented Features

Ownership:
- Manages data lifetimes
- Avoids some bugs from user usage

Next:
1. More examples of safety features
2. Why do we talk about this?

---

# `switch` Safety: Java vs C/C++

Java-style:

```java
switch (x) {
  case 1:
    // do something
    break;
  case 2:
    // do something
    break;
  default:
    // do something
}
```

`break` per case avoids accidental fallthrough.

---

# C/C++ Fallthrough Risk

```cpp
switch (x) {
  case 1:
    // do something 1
  case 2:
    // do something 2
    break;
  default:
    // do something
}
```

When `x == 1`, both case 1 and case 2 run.
Sometimes intentional, often a bug.

---

# Shadowing Rules Across Languages

```cpp
int x;
{
  int x; // allowed in C++
}
```

```rust
let x: i32 = 5;
let x: &str = "hello"; // allowed in Rust
{
  let x: u8 = 5; // also allowed
}
```

```java
int x = 5;
{
  int x = 10; // compile error in Java
}
```

---

## Why do we care about ownership and lifetime?

Because compiler optimization is a stateful and redundant process.

- **Immutably** analyze the intermeidate representation (IR)
- **Mutably** transform the IR to optimize it

which naturally fits the ownership and borrowing model.
All your compiler passes should be designed following the paradigm above.

---

# Takeaways

- correctness-oriented features prevent bug classes by construction
- ownership/borrowing prevents many memory and aliasing bugs
- conservative defaults can reduce control-flow and scope mistakes
- language design can shift failures from runtime to compile time
