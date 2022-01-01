# Memory safety

Memory safety is the state of being protected from various software bugs and
security vulnerabilities when dealing with memory access, such as buffer
overflows and dangling pointers. For example, Java, Golang and Rust are said
to be memory-safe because its runtime error detection checks array bounds and
pointer dereferences. In contrast, C and C++ allow arbitrary pointer arithmetic
with pointers implemented as direct memory addresses with no provision for
bounds checking, and thus are potentially memory-unsafe
[[ref]](https://en.wikipedia.org/wiki/Memory_safety).

