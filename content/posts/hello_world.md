---
title: "Hello World!"
date: 2023-08-04T16:39:40+08:00
draft: false
math: true
---

Hello World! This is my first blogpost since ever. I decided to make this blog
because I wanted a platform to share about mathematics/CS related things,
and I feel that I always learn more when I try to write a coherent article
about it.

This blog is hosted on [Hugo](https://gohugo.io/),
with the [Geekblog](https://hugo-geekblog.geekdocs.de/) template.
It has been an absolute pleasure to set up thus far, and with some meddling,
I managed to get $\LaTeX$ set up properly in this blog with $\KaTeX$...
I will definitely have to customize the logo and colourscheme to match
my own taste though...

The last thing I need to set up is a comment section using
[utterances](https://utteranc.es/), but this will take a while to get going.

---

The rest of this post is going to be incoherent nonsense for me to ensure
every feature I want is working.

This is some $\KaTeX$: $(\forall X)(\exists!f:\emptyset\to X)$

Here is some more $\KaTeX$, in display mode:
$$
    \int_0^\infty \frac{\sin (x)}{x} dx=\frac{pi}{2}
$$
$$
    \int_0^\infty
    \frac{\sin (x)}{x}
    \frac{\sin (x/3)}{x/3}
    dx=\frac{\pi}{2}
$$
$$\vdots$$
$$
    \int_0^\infty
    \frac{\sin (x)}{x}
    \frac{\sin (x/3)}{x/3}
    \cdots
    \frac{\sin (x/13)}{x/13}
    dx=\frac{\pi}{2}
$$
$$
\begin{aligned}
 \int_0^\infty
    \frac{\sin (x)}{x} &
    \frac{\sin (x/3)}{x/3}
    \cdots
    \frac{\sin (x/15)}{x/15}
    dx\\\ = &
    \frac{467807924713440738696537864469}{935615849440640907310521750000}\pi
\end{aligned}
$$

Here is some syntax highlighted code blocks in various languages:

```python
# python
for i in range(10, -1, -1):
    print(f"this message will self-destruct in {i} seconds...")
    sleep(1)

print("BOOM")
```

```cpp
// C++
int main() {
    int a = 1;
    a += a++ - --a;
    cout << a << "\n";
}
```

```java
// Java
public class Main {
    public static void main(String[] iLoveJavaILoveJavaILoveJava) {
        Stream.iterate(1, x -> x+1)
              .limit(100)
              .map(x -> x % 15 == 0 ? "FizzBuzz" : x % 3 == 0 ?
                    "Fizz" : x % 5 == 0 ? "Buzz" : x.toString())
              .forEach(System.out::println);
    }
}
```
