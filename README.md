# counters
A GoLang implementation of counters.

It provides an object: `counters.CounterBox` and `Counter`, `Min` and `Max` implementation.

Installation:

    $ go get -u github.com/orian/counters

The example usages:

    import "github.com/orian/counters"

    //... some code

    cb := counters.NewCounterBox()
    c := cb.GetCounter("ex")
    c.Increment()
    c.IncrementBy(6)
    c.Value()  // returns 7
    cb.GetCounter("ex").Value()  // Returns 7

For convenience there is also an `http.HandleFunc` provided which prints values of counters.

One may use subpackage `globals` if want to use a global counters.

    import "github.com/orian/counters/global"

    //... some code

    c := global.GetCounter("ex")
    c.Increment()
    c.IncrementBy(6)
    c.Value()  // returns 7
    global.GetCounter("ex").Value()  // Returns 7

The library and all objects are thread safe.

## Print into log or stdout on SIGINT

Please check the `example/example.go` to see how the interrupt can be handled in a nice way.

## Performance and benchmarks
The library has two benchmarks. It's blazingly fast. One should cache counters
which are used often. Every time the counter is requested, it's necessary to
look up in map for a counter, this slows down the lib by a factor of 4.

    $ go test --bench=. .
    PASS
    BenchmarkCounters        1000000      1287 ns/op
    BenchmarkCountersCached  5000000       252 ns/op
    ok      github.com/orian/counters   2.822s