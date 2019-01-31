# Impatient

Wait for services or resources to become available. This package is based on similar functionality from [Dockerize](https://github.com/jwilder/dockerize), adapted to be used as a library rather than a command-line tool.

The package exports a single method, `Await`, which waits for a set of resource URLs to all become available. You may provide a timeout after which the method will give up and report an error.

```go
func Await(cxt context.Context, urls []string, timeout time.Duration) error
```

The following URL schemes are supported:
* `file` – wait until we can stat the file,
* `tcp`, `tcp4`, `tcp6` – wait until we can connect a TCP/IP socket,
* `unix` – wait until we can connect to a UNIX socket,
* `http`, `https` – wait until an HTTP request succeeds with status `200/OK`.

## Example Usage

```go
resources := []string{
  "http://localhost:8000/status", // Wait until this endpoint returns 200/OK
  "tcp4://localhost:5432/",       // Wait until we can connect to localhost on port 5432
  "file:///tmp/some_file.txt",    // Wait until we can stat this file
}

err := impatient.Await(context.Background(), resources, time.Minute)
if err == impatient.ErrTimeout {
  // All the services did not become available before a minute elapsed
} else if err != nil {
  // Somethingt else went wrong
}
```
