# Rethinking the Language Features

- I am assuming most of you only ask AI to write some Rust compiler stuff for you.
- But you do not understand the key language feature of Rust yet.
- Rust: Ownership, Borrowing, and Lifetimes.

- What is actually the ownership giving you?

```C++
void foo(int x) {
    // do something with x
}

int main() {
    foo(5); // its ok.
}
```

However, what if this happens?

```C++
struct Foo {
    int a[100];
};

void foo(Foo x) {
    // do something with x
}

int main() {
    Foo f;
    foo(f); // its ok, but it is expensive.
}
```

What is a proper way of doing this?

```C++
void foo(const Foo& x) {
    // do something with x
}

void foo(Foo *x) {
    // do something with x
}

int main() {
    Foo f;
    foo(f); // its ok, and it is cheap.
    foo(&f); // its ok, and it is cheap.
}
// The only difference between pointer and reference is that
// pointer can be null and can be rebound, but reference can only be bound once upon instantiation.
```

Actually, what in your Claude-generated C++ code, is already highly Rust-ized!

```C++
void foo(Foo x) {
    // do something with x
}

int main() {
    Foo f;
    foo(std::move(f)); // its ok, and it is cheap.
}
```

What does this `std::move` mean?
I do not know either, but I give you a Rust perspective on this.

```Rust
fn foo(x: Foo) {
    // do something with x
}

fn main() {
    let f = Foo::new();
    foo(f); // its ok, and it is cheap.
    // you can no longer use f after this. this f is destructed!
    // because the ownership of f is **moved** to foo.
}
```

What if I still want to use f?

```Rust
fn foo(x: &Foo) {
    // do something with x
}

fn main() {
    let f = Foo::new();
    foo(&f); // its ok, and it is cheap. `&` is basically the same as C++ reference.
             // in rust, an additional meaning is `f` is borrowed to `foo`.
             // after foo returns `f` is returned.

}
```

---

Beyond saving a memory copy, what does it give you?

```C++
std::vector<int> v = {1, 2, 3, 4, 5};
for (auto &x : v) {
    v.push_back(x); // this is a disaster, because you are modifying the vector while iterating it.
                    // why?
}
```

How is an vector implemented?
1. Allocate a chunk, C, of memory.
2. Put the elements into the chunk, and increase the `size`.
3. If exceeds the chunck, allocate `C * 2`, copy all the old elements to the new chunk, and deallocate the old chunk.

Mathematically, each element is copied at most twice, as sum(1, 2, 4, 8, 16, ..., 2^n) = 2^(n+1) - 1; by mean, each element is only copied at most twice.

However, if we expand the code, you may see:

```C++
iter = v.data;
end = v.data + v.size;
while (iter != end) {
    v.push_back(*iter);
    ++iter;
}
```

`iter` may iterate over some memory that is freed because of doubling the chunk.
`end` may still point to some old memory end.


What happens in Rust?

```Rust
let mut v = vec![1, 2, 3, 4, 5];
for x in v.iter() {
    v.push(*x); // this is a compile error, because you are modifying the vector while iterating it.
}
```

By default, each borrow is immutable, so when iterating over the vector, the vector is borrowed for iteration immutably.
Alll the modifications are forbidden because the vector is borrowed immutably.

---

To sum up, this is a good language feature to use language itself to guarantee the correctness.

---

There are some other features that guarantee the correctness of the code.

A famous language is Java, which conservatively manages the language.

```Java
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

Break is required for each case.

However, in C and C++, break is optional for fallthrough.

```C++
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

When it is 1, it does something 1 and something 2 --- sometimes it is intentional, but sometimes it is a bug.

---

For example, for the scope management:

```C++
int x;
{
    int x; // This is allowed.
}
```

```Rust
let x: i32 = 5;
let x: str = "hello"; // This is even allowed.
{
    let x: u8 = 5; // This is allowed.
}
```

```Java
int x = 5;
{
    int x = 10; // This is not allowed, because Java does not allow variable shadowing.
}
```