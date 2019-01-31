# Impatient

Wait for services or resources to become available. This implementation is based on similar functionality from [Dockerize](https://github.com/jwilder/dockerize).

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
