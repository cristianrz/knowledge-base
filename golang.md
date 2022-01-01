# Golang

## Why I chose golang

- Golang is open-source and backed up by a large corporation
- Golang syntax is simple
- Good cross platform support
- It is a compiled language
- Compiles into a single binary, therefore can install anywhere without
installing any dependencies.
- Static type system. Tells you about type mistakes on compiling
- Performs better than python
[[source]](https://benchmarksgame.alioth.debian.org/u64q/compare.php?lang=go&lang2=python3)
- Go has garbage collection (whatever that means)
- Go has a concurrency model (whatever that means)
- Go is more elegant than C++
- Compile errors are more readable than other languages

## Tips & tricks

- `go build -ldflags "-w -s"` makes the executable smaller
- `go get -u golang.org/x/lint/golint` for linting go code
- For encoding, `encoding/json` and `encoding/xml`
- `go build -pie` introduces ASLR.

## Implementations

### Goroutines

```go
go func(a int){
	...
} (x)
```

### Port scanner

```go
conn, err := net.Dial("tcp", "scanme.nmap.org:80")
if err != nil {
	fmt.Printf("Connection failed on port 80")
}

conn.Close()
```

### Wait

```go
// creates the struct
var wg sync.WaitGroup 

for {
	// +1 to the counter
	wg.Add(1)

	go func(j int){
		// at the end of the func, -1
		defer wg.Done()
		...
	} (a)
}

// waits for the counter to be 0
wg.Wait()
```

### Worker functions

```go
func worker(ports chan int, wg *sync.WaitGroup){
	for p := range ports {
		fmt.Println(p)
		wg.Done()
	}
}
```

### Buffered channel

```go
ch := make(chan int, 100)
```

