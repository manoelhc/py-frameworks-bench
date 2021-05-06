# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-05-06

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp is an exception).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-05-06)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B426270%2C370725%2C347790%2C293430%2C263610%2C244320%2C218640%2C163320%2C95475%2C92010%2C53445%5D%7D%5D%7D%7D' />

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

2. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.

3. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.


## The Results (2021-05-06)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 15596 | 3.25 | 5.60 | 4.08
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 13741 | 3.66 | 6.42 | 4.64
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 12348 | 4.03 | 7.25 | 5.15
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 11304 | 4.43 | 7.86 | 5.64
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 10721 | 4.63 | 8.25 | 5.94
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 7877 | 6.29 | 11.34 | 8.10
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 6926 | 7.16 | 12.99 | 9.24
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 5423 | 11.62 | 12.03 | 11.80
| [quart](https://pypi.org/project/quart/) `0.14.1` | 3025 | 21.15 | 23.51 | 21.14
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2525 | 25.21 | 25.71 | 25.34
| [django](https://pypi.org/project/django/) `3.2.2` | 1451 | 43.27 | 49.22 | 44.08


</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 4605 | 10.77 | 19.56 | 13.87
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 3491 | 14.20 | 26.21 | 18.31
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 3459 | 14.24 | 25.93 | 18.51
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 3061 | 16.22 | 28.88 | 20.96
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2126 | 23.13 | 43.13 | 30.07
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2029 | 31.07 | 32.33 | 31.60
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 1950 | 24.91 | 46.64 | 32.78
| [tornado](https://pypi.org/project/tornado/) `6.1` | 1778 | 35.57 | 36.49 | 35.96
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 1324 | 45.45 | 54.10 | 48.29
| [quart](https://pypi.org/project/quart/) `0.14.1` | 1200 | 53.92 | 55.89 | 53.28
| [django](https://pypi.org/project/django/) `3.2.2` | 855 | 72.10 | 84.09 | 74.64


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 8217 | 6.05 | 10.93 | 7.76
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 7777 | 6.45 | 11.62 | 8.20
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 7515 | 6.72 | 11.86 | 8.49
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 6132 | 8.13 | 14.76 | 10.41
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 5871 | 8.50 | 15.48 | 10.89
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 5529 | 8.85 | 16.05 | 11.59
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 4749 | 10.41 | 19.05 | 13.45
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 3436 | 18.34 | 19.20 | 18.62
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2062 | 30.29 | 31.82 | 31.03
| [quart](https://pypi.org/project/quart/) `0.14.1` | 1909 | 33.68 | 34.92 | 33.51
| [django](https://pypi.org/project/django/) `3.2.2` | 1257 | 48.88 | 57.10 | 50.83

</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 426270 | 6.69 | 12.03 | 8.57
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 370725 | 8.21 | 14.74 | 10.55
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 347790 | 8.9 | 15.92 | 11.44
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 293430 | 11.9 | 21.92 | 15.37
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 263610 | 19.64 | 26.13 | 21.94
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 244320 | 9.95 | 18.23 | 12.81
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 218640 | 13.87 | 25.68 | 18.11
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 163320 | 20.34 | 21.19 | 20.67
| [tornado](https://pypi.org/project/tornado/) `6.1` | 95475 | 30.36 | 31.34 | 30.78
| [quart](https://pypi.org/project/quart/) `0.14.1` | 92010 | 36.25 | 38.11 | 35.98
| [django](https://pypi.org/project/django/) `3.2.2` | 53445 | 54.75 | 63.47 | 56.52

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)