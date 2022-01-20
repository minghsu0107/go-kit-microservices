# Go Kit Microservices
Example microservices implemented with [go kit](https://github.com/go-kit/kit), a programming toolkit for building microservices in Go.

HTTP APIs:
- `/uppercase`: convert string to uppercase
- `/count`: return string length
- `/metrics`: Prometheus metrics endpoint
  - return sum and count of `my_group_string_service_request_latency_microseconds`
  - return sum and count of `my_group_string_service_count_result`

Build service:
```bash
go build -o server
```
Usage:
```
> ./server -h
Usage of ./server:
  -listen string
    	HTTP listen address (default ":8080")
  -proxy string
    	Optional comma-separated list of URLs to proxy uppercase requests
```
For example, we could start a server on `localhost:8080` and proxy all uppercase requests to `localhost:8081` and `localhost:8082`.
```
# in first terminal
./server --listen=localhost:8081

# in second terminal
./server --listen=localhost:8082

# in third terminal
./server --listen=localhost:8080 --proxy=localhost:8081,localhost:8082
```
Query example:
```
> curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"s":"hello"}' \
  http://localhost:8080/count
{"v":5}
> curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"s":"hello"}' \
  http://localhost:8080/uppercase
{"v":"HELLO"}
```
