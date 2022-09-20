Prometheus metrics endpoint for NGINX Unit
==========================================

Simple Python program to convert Unit's native JSON **/status** metrics into Prometheus format.

Create a new `listeners` object on the default Prometheus port:
```json
    "*:9090": {
        "pass": "applications/metrics"
    }
```

Create a matching `applications` object:
```json
    "metrics": {
        "type": "python",
		"path": "/Users/l.crilly/demo",
		"module": "metrics",
        	"environment": {
			"ADDR": "127.0.0.1:8080"
		}
	}
```
> NB: Set the `ADDR` environment variable to match the Unit control address. Unix sockets are not currently supported. A future version may `import requests_unixsocket`.

Request any URI on port 9090 to scrape the metrics.
```shell
$ curl -i localhost:9090
HTTP/1.1 200 OK
Content-Type: text/plain
Server: Unit/1.28.0
Date: Sun, 18 Sep 2022 20:14:33 GMT
Transfer-Encoding: chunked

unit_connections_accepted_total 36
…
```