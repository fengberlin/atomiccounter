# atomiccounter

English | [简体中文](README_zh.md)

[![License Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-red.svg)](COPYING)
[![Golang](https://img.shields.io/badge/Language-go1.18+-blue.svg)](https://go.dev/)
![Build Status](https://github.com/chen3feng/atomiccounter/actions/workflows/go.yml/badge.svg)
[![Coverage Status](https://coveralls.io/repos/github/chen3feng/atomiccounter/badge.svg?branch=master)](https://coveralls.io/github/chen3feng/atomiccounter?branch=master)
[![GoReport](https://goreportcard.com/badge/github.com/securego/gosec)](https://goreportcard.com/report/github.com/chen3feng/atomiccounter)

A High Performance Atomic Counter for Concurrent Write-More-Read-Less Scenario in Go.

Similar to [LongAdder](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/LongAdder.html) in Java, or
[ThreadCachedInt](https://github.com/facebook/folly/blob/main/folly/docs/ThreadCachedInt.md) in [folly](https://github.com/facebook/folly),
In scenarios of high concurrent writes but few reads, it can provide dozens of times the write performance than `sync/atomic`.

Benchmark(per 100 calls):

```console
BenchmarkNonAtomicAdd-10        47337121                22.14 ns/op
BenchmarkAtomicAdd-10             180942              6861 ns/op
BenchmarkCounter-10             14871549                81.02 ns/op
```

<!-- gomarkdoc:embed:start -->

<!-- Code generated by gomarkdoc. DO NOT EDIT -->

# atomiccounter

```go
import "github.com/chen3feng/atomiccounter"
```

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/chen3feng/atomiccounter"
	"sync"
)

func main() {
	counter := atomiccounter.NewInt64()
	var wg sync.WaitGroup
	for i := 0; i < 100; i++ {
		wg.Add(1)
		go func() {
			counter.Inc()
			wg.Done()
		}()

	}
	wg.Wait()
	fmt.Println(counter.Load())
	counter.Set(0)
	fmt.Println(counter.Load())
}
```

#### Output

```
100
0
```

</p>
</details>

## Index

- [type Int64](<#type-int64>)
  - [func NewInt64() *Int64](<#func-newint64>)
  - [func (c *Int64) Add(n int64)](<#func-int64-add>)
  - [func (c *Int64) Inc()](<#func-int64-inc>)
  - [func (c *Int64) Load() int64](<#func-int64-load>)
  - [func (c *Int64) Set(n int64)](<#func-int64-set>)
  - [func (c *Int64) Swap(n int64) int64](<#func-int64-swap>)


## type [Int64](<https://github.com/chen3feng/atomiccounter/blob/master/int64.go#L12-L15>)

Int64 is an int64 atomic counter.

```go
type Int64 struct {
    // contains filtered or unexported fields
}
```

### func [NewInt64](<https://github.com/chen3feng/atomiccounter/blob/master/int64.go#L22>)

```go
func NewInt64() *Int64
```

NewInt64 creates a new Int64 object.

### func \(\*Int64\) [Add](<https://github.com/chen3feng/atomiccounter/blob/master/int64.go#L27>)

```go
func (c *Int64) Add(n int64)
```

Add adds n to the counter.

### func \(\*Int64\) [Inc](<https://github.com/chen3feng/atomiccounter/blob/master/int64.go#L33>)

```go
func (c *Int64) Inc()
```

Inc adds 1 to the counter.

### func \(\*Int64\) [Load](<https://github.com/chen3feng/atomiccounter/blob/master/int64.go#L43>)

```go
func (c *Int64) Load() int64
```

Load return the current value.

### func \(\*Int64\) [Set](<https://github.com/chen3feng/atomiccounter/blob/master/int64.go#L38>)

```go
func (c *Int64) Set(n int64)
```

Set set the value of the counter to n.

### func \(\*Int64\) [Swap](<https://github.com/chen3feng/atomiccounter/blob/master/int64.go#L52>)

```go
func (c *Int64) Swap(n int64) int64
```

Swap returns the current value and swap it with n.



Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)


<!-- gomarkdoc:embed:end -->
