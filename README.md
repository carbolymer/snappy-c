# snappy-c

![CI Status Badge](https://github.com/well-typed/snappy-c/actions/workflows/haskell-ci.yml/badge.svg)

Haskell bindings to the C API of [Snappy][snappy]: A fast compression library.

# Usage

> [!IMPORTANT]
> To use this package, you must have the Snappy library installed **and visible
> to `cabal`**.

If your build fails with a message like:
```
Error: cabal-3.10.1.0: Missing dependency on a foreign library:
* Missing (or bad) C library: snappy
```

You need to explicitly configure the include and library paths via cabal. One
way to do so is to add something like this to your `cabal.project`
configuration:

```cabal
package snappy-c
  extra-include-dirs:
    /path/to/snappy/include
  extra-lib-dirs:
    /path/to/snappy/lib
```

In my case, on a mac using homebrew, the following suffices:

```cabal
package snappy-c
  extra-lib-dirs:
    /opt/homebrew/lib
  extra-include-dirs:
    /opt/homebrew/include
```

We wouldn't need to do this if the Snappy library supported pkg-config
configuration, but as of writing [they do
not](https://github.com/google/snappy/pull/86#issuecomment-552237257).

## Installing Snappy

The [Snappy][snappy] library is available most package managers.

### Homebrew

```
brew install snappy
```

### APT

```
apt-get install libsnappy-dev
```


# Performance

snappy-c stays true to the speedy spirit of Snappy compression.

The package includes a benchmark suite that compares the compression,
decompression, and roundtrip performance of this package against
- a [pre-existing (unmaintained) Snappy
  implementation](https://hackage.haskell.org/package/snappy), and
- the [zlib package's](https://hackage.haskell.org/package/zlib) zlib and gzip
  compression format implementations.

Run the benchmarks with:
```
cabal run bench-snappy-c -- --time-limit 5 -o bench.html
```

> [!IMPORTANT]
> To build the benchmarks, you will need to set `extra-include-dirs` and
> `extra-lib-dirs` for the `snappy` package as you did for `snappy-c` (see
> [Usage](#usage)).

Here's a screenshot from the generated bench.html file:

![benchmark results](https://raw.githubusercontent.com/well-typed/snappy-c/main/.github/assets/bench-results.png)

## `snappy-cli` performance

<!-- Compressing and decompressing the the [enwik9 test
data](https://mattmahoney.net/dc/textdata.html) (~953MB) 5 times in a minimally
controlled yet consistent environment resulted in the following average times:

| Tool         | Compress time (s) | Decompress time (s)  |
| ------------ | ----------------- | -------------------- |
| `snappy-cli` | 2.074             | 0.61                 |
| `snzip`      | 2.312             | 1.07                 |
| `gzip`       | 26.51             | 1.38                 | -->

See
[snappy-cli/README.md](https://github.com/well-typed/snappy-c/tree/main/snappy-cli)
for a demonstration of the performance of this library in a CLI tool.

<!-- Links -->
[snappy]: https://github.com/google/snappy
