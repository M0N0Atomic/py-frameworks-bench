# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-07-06

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-07-06)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B542520%2C521430%2C458235%2C397935%2C356820%2C320325%2C296850%2C222660%2C131160%2C112680%2C73620%5D%7D%5D%7D%7D' />

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.

3. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.


## The Results (2021-07-06)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [muffin](https://pypi.org/project/muffin/) `0.83.1` | 19405 | 3.78 | 4.04 | 3.26
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.8` | 19210 | 2.76 | 4.31 | 3.30
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 16180 | 3.17 | 5.20 | 3.92
| [starlette](https://pypi.org/project/starlette/) `0.15.0` | 15733 | 4.70 | 5.15 | 4.03
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 14041 | 3.58 | 6.10 | 4.53
| [fastapi](https://pypi.org/project/fastapi/) `0.66.0` | 11073 | 6.88 | 7.32 | 5.74
| [sanic](https://pypi.org/project/sanic/) `21.6.0` | 9266 | 5.30 | 9.04 | 6.92
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7777 | 8.21 | 8.30 | 8.23
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3534 | 18.11 | 18.21 | 18.11
| [quart](https://pypi.org/project/quart/) `0.15.1` | 3508 | 18.83 | 19.57 | 18.23
| [django](https://pypi.org/project/django/) `3.2.5` | 2130 | 29.83 | 32.95 | 30.07


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.8` | 10812 | 4.57 | 7.93 | 5.88
| [muffin](https://pypi.org/project/muffin/) `0.83.1` | 10635 | 4.60 | 8.09 | 5.98
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10515 | 4.71 | 8.22 | 6.06
| [starlette](https://pypi.org/project/starlette/) `0.15.0` | 8248 | 6.01 | 10.62 | 7.73
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 8084 | 8.97 | 10.08 | 8.05
| [sanic](https://pypi.org/project/sanic/) `21.6.0` | 7758 | 6.34 | 11.16 | 8.26
| [fastapi](https://pypi.org/project/fastapi/) `0.66.0` | 6440 | 7.83 | 13.60 | 9.91
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4690 | 13.43 | 14.08 | 13.64
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2947 | 21.71 | 21.88 | 21.72
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2159 | 29.23 | 29.85 | 29.63
| [django](https://pypi.org/project/django/) `3.2.5` | 1659 | 37.52 | 42.80 | 38.54

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.8` | 6146 | 8.01 | 14.27 | 10.42
| [muffin](https://pypi.org/project/muffin/) `0.83.1` | 4722 | 10.56 | 18.50 | 13.61
| [sanic](https://pypi.org/project/sanic/) `21.6.0` | 4331 | 11.29 | 20.19 | 14.77
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3854 | 13.12 | 22.07 | 16.69
| [starlette](https://pypi.org/project/starlette/) `0.15.0` | 2548 | 19.42 | 34.34 | 25.08
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2377 | 26.87 | 27.39 | 26.90
| [fastapi](https://pypi.org/project/fastapi/) `0.66.0` | 2277 | 21.51 | 38.22 | 28.07
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2263 | 28.26 | 28.41 | 28.28
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1845 | 34.68 | 35.42 | 34.67
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 1663 | 36.43 | 42.10 | 38.46
| [django](https://pypi.org/project/django/) `3.2.5` | 1119 | 56.80 | 62.63 | 57.01


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.8` | 542520 | 5.11 | 8.84 | 6.53
| [muffin](https://pypi.org/project/muffin/) `0.83.1` | 521430 | 6.31 | 10.21 | 7.62
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 458235 | 7.0 | 11.83 | 8.89
| [starlette](https://pypi.org/project/starlette/) `0.15.0` | 397935 | 10.04 | 16.7 | 12.28
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 356820 | 16.33 | 19.43 | 17.01
| [sanic](https://pypi.org/project/sanic/) `21.6.0` | 320325 | 7.64 | 13.46 | 9.98
| [fastapi](https://pypi.org/project/fastapi/) `0.66.0` | 296850 | 12.07 | 19.71 | 14.57
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 222660 | 16.17 | 16.59 | 16.26
| [tornado](https://pypi.org/project/tornado/) `6.1` | 131160 | 22.69 | 22.83 | 22.7
| [quart](https://pypi.org/project/quart/) `0.15.1` | 112680 | 27.58 | 28.28 | 27.51
| [django](https://pypi.org/project/django/) `3.2.5` | 73620 | 41.38 | 46.13 | 41.87

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)