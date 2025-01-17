# About

Package [math/rand][mathrand] provides support for generating pseudo-random numbers.

Here is how to generate a random integer between `0` and `99`:

```go
n := rand.Intn(100) // n is a random int, 0 <= n < 100
```

Function `rand.Float64` returns a random floating point number between `0.0` and `1.0`:

```go
f := rand.Float64() // f is a random float64, 0.0 <= f < 1.0
```

There is also support for shuffling a slice (or other data structures):

```go
x := []string{"a", "b", "c", "d", "e"}
// shuffling the slice put its elements into a random order
rand.Shuffle(len(x), func(i, j int) {
	x[i], x[j] = x[j], x[i]
})
```

## Seeds

The number sequences generated by package `math/rand` are not truly random.  
They are completely determined by an initial `seed` value.
By default, the package uses the number `1` as a seed.

In order to get a different sequence of random numbers, we need to set a different seed.
A simple approach is to seed with the current computer time.

```go
rand.Seed(time.Now().UnixNano())
```

Setting the seed must be done **before** calling any of the functions that generate random numbers.
We usually only need to set the seed once in our program.
From one seed we can generate many random numbers.

### Warning

If you don't call `rand.Seed`, then the package will produce the same sequence of random numbers each time your program runs!

As an example, consider the following program:

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
    // rand.Seed(time.Now().UnixNano())
	a := rand.Intn(1000000)
	b := rand.Intn(1000000)
	c := rand.Intn(1000000)
	fmt.Println(a, b, c)
}
```

Every time this program runs, it will produce the same =>

```text
498081 727887 131847
```

Once we uncomment the first line in the `main()` function, the output varies between different runs:

```text
817603 745216 422387

265737 21561 640434

708506 627509 363987
```

~~~~exercism/caution

The output produced by package `math/rand` might be easily predictable regardless of how it is seeded.
For random numbers suitable for security-sensitive work, you should use the `crypto/rand` package, see link below.
~~~~

[mathrand]: https://pkg.go.dev/math/rand
