---
title: "The Sieve of Erathostenes and Why It Runs in O(n log log n)"
date: 2023-08-04T23:09:14+08:00
draft: false
math: true
---

Recently I took up the challenge of solving the first one hundred problems of
[Project Euler](https://projecteuler.net/) (again), but this time I was to
try using Java streams liberally. The idea was to familiarize myself with
Java streams in preparation of a class I will be taking that needs them.
Anyway, I hit Problem 7, which has a rather short description:

>  By listing the first six prime numbers: 2, 3, 5, 7, 11, and 13,
we can see that the 6th prime is 13.  
What is the 10001st prime number?

If you want to have a go at this question, stop here! There are spoilers ahead.

My final solution ended up being rather simple. First the imports:

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.function.*;
import java.util.stream.*;
```

The driver code is pleasantly short:
```java
public class Solution {
    public static void main(String[] primeExampleOfTheSieveOfErathostenes) {
        System.out.println(Stream
                .generate(new PrimeSupplier())
                .skip(10000)
                .findFirst()
                .orElse(-1)); // should not happen
    }
}
```

And the heart of the solution is the famous sieve of Erathostenes:
```java
class PrimeSupplier implements Supplier<Integer> {
    HashMap<Integer, ArrayList<Integer>> multiples = new HashMap<>();
    int q = 1;

    public Integer get() {
        while (true) {
            ++q;
            if (multiples.containsKey(q)) {
                for (int p : multiples.get(q)) {
                    if (!multiples.containsKey(p + q))
                        multiples.put(p + q, new ArrayList<>());
                    multiples.get(p + q).add(p);
                }
                multiples.remove(q);
            } else {
                multiples.put(q * q, new ArrayList<>());
                multiples.get(q * q).add(q);
                return q;
            }
        }
    }
}
```

The Sieve of Erathostenes is a way to get the first few prime numbers, up
to some specified upper limit. The (slightly hideous) code implements
what is known as an *incremental* sieve, which does not have an upper limit
barring memory overflow and integer overflow, but for simplicity's sake, we
will look at the classical sieve:

```java
final int UPPER_LIMIT = 0xF00DFACE; // will be changed
boolean[] sieve = new boolean[UPPER_LIMIT];

sieve[0] = sieve[1] = false;
for (int i = 2; i < UPPER_LIMIT; ++i)
    if (sieve[i])
        for (int j = 2 * i; j < UPPER_LIMIT; j += i)
            sieve[j] = false;
```
Obviously this can be optimized much further, but stick with me for now.
After this code fragment runs, `sieve[i]` will be true if and only if
`i` is a prime number. How do we know this? We observe that the outer for loop
obeys the following loop invariant:

> At the end of the $k$'th loop (i.e. `i`$ = k + 1$), `sieve[0]` to `sieve[k+1]`
inclusive will be the correct value.

Indeed, we can use induction to show this: the base case $k=0$ is clearly
true, and assuming the statement is true for some $k=m$, then we clearly have
that
* The values of `sieve[0]` to `sieve[m]` aren't modified in the $(m+1)$'th
loop; and
* `sieve[m+1]` can only be false if and only if there was an integer $i$ such
that $m+1$ is a multiple of $i$ larger than $i$; in other words, $m+1$ is
composite.

Having shown the correctness of the Sieve, we can go on to ask ourselves: how
quickly does this run? For most applications ($n < 10^9$) it runs in a couple
dozen milliseconds, which is pretty darn fast.
As you may have seen in the title of this post, the runtime of this code fragment
is $\mathcal{O}(n\log\log n)$. But to get this value requires a little bit of
mathematics. Buckle up.

We get an expression for our runtime just by asking how many times the
last line (`sieve[j] = false;`) runs. It certainly only runs when $i$ is prime,
and it is called once for all multiples of $i$ except $i$ itself. This is
roughly $\frac{n}{i}$ of the numbers. Repeating this for every prime $i$,
we can get the expression
$$
T(n) \sim \sum_{\substack{i\in\\{1,\ldots,n\\}\\\i\text{ prime}}} \frac{n}{i}.
$$
Note that for any positive integer $k$, we have
$$
\left(\frac{T(n)}{n}\right)^k \sim \sum_{p_1,\ldots,p_k<n}\frac{1}{p_1\ldots p_k}
$$
where $p_1,\ldots,p_n$ are prime numbers. Note that
* every positive integer is a unique product of primes by the
fundamental theorem of arithmetic,
* there are $k!$ permutations of $p_1,\ldots,p_k$; and
* $p_1\ldots p_k \leq n\ldots n = n^k$.  
Then
$$
\begin{aligned}
\left(\frac{T(n)}{n}\right)^k &\leq k! \sum_{1<x<n^k}\frac{1}{x}
\\\ &= k!\cdot O(\log n^k)
\\\ &= k! \cdot k\cdot O(\log n)
\end{aligned}
$$
Taking $k$'th roots,
$$
\frac{T(n)}{n} = \sqrt[k]{k!\sum_{1<x<n^k} k\cdot O(\log n)}
$$
Use the fact that $k!=1\cdot 2\cdot \ldots\cdot k\leq k^k$ and that
$\sqrt[k]{k} < 2$ to get
$$
\frac{T(n)}{n} = 2k\sqrt[k]{O(\log n)}.
$$
Since $k$ can be any positive integer, for $n$ large we set
$$k=\lfloor \log \log n + 1\rfloor $$
and get
$$
\frac{T(n)}{n} = 2\lfloor \log\log n + 1\rfloor
\sqrt[\lfloor \log \log n + 1\rfloor ]{O(\log n)}.
$$

A graphing calculator will reveal that $n \cdot n^{\frac{1}{\log log n}}$
is bounded
above by a constant, so we multiply out by $n$, use the power of big $O$ and 
drop all constants and floor functions to get
$$
T(n) = O(n\log\log n)
$$
as desired. Phew!

