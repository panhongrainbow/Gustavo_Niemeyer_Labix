# Vclock 下载函式库说明

> 这份文件是主要是从 https://labix.org/vclock 直接复制下来的内容

## Introduction

The vclock package offers full vector clock support for the Go language. Vector clocks allow recording and analyzing the inherent partial ordering of events in a distributed system in a comfortable way. The following pair of blog posts from Basho describes them in a nicely understandable way:

- [Why vector clocks are easy](http://blog.basho.com/2010/01/29/why-vector-clocks-are-easy/)
- [Why vector clocks are hard](http://blog.basho.com/2010/04/05/why-vector-clocks-are-hard/)

The following features are offered by the vclock package, in addition to basic event tracking:

- Compact serialization and deserialization
- Flexible truncation (min/max entries, min/max update time)
- Unit-independent update times
- Traditional merging
- Fast and memory efficient

## API documentation

The API documentation may be accessed via the package path itself:

- [labix.org/v1/vclock](http://labix.org/v1/vclock)

## Example

Here is a simple example simulating a few sequential and concurrent operations and comparing the resulting vector clocks.

```go
package main

import (
	"fmt"
	"labix.org/v1/vclock"
)

func main() {
	vc1 := vclock.New()
	vc1.Update("A", 1)

	vc2 := vc1.Copy()
	vc2.Update("B", 0)

	fmt.Println(vc2.Compare(vc1, vclock.Ancestor))   // true
	fmt.Println(vc1.Compare(vc2, vclock.Descendant)) // true

	vc1.Update("C", 5)

	fmt.Println(vc1.Compare(vc2, vclock.Descendant)) // false
	fmt.Println(vc1.Compare(vc2, vclock.Concurrent)) // true

	vc2.Merge(vc1)

	fmt.Println(vc1.Compare(vc2, vclock.Descendant)) // true

	data := vc2.Bytes()
	fmt.Printf("%#v\n", string(data))

	vc3, err := vclock.FromBytes(data)
	if err != nil {
		panic(err)
	}

	fmt.Println(vc3.Compare(vc2, vclock.Equal)) // true
}
```

## Source code

To obtain the source code, use [Bazaar](http://bazaar.canonical.com/) to download it from Launchpad:

```
 $ bzr branch lp:vclock/v1 
```

## Reporting bugs

Please report bugs at:

- https://launchpad.net/vclock

## Building and installing

Just use [the go command](http://golang.org/cmd/go): ` $ go get labix.org/v1/vclock `

## License

The vclock package is made available under the simplified BSD license.

## Contact

Gustavo Niemeyer <[gustavo@niemeyer.net](mailto:gustavo@niemeyer.net)>
