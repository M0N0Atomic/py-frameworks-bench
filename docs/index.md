---
layout: home
list_title: Archive
description: Python frameworks benchmarks
---

<script src="https://cdn.jsdelivr.net/npm/chart.js@3.2.1/dist/chart.min.js"></script>

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

* Read about the benchmark: [The Methodic](methodic.md)
* Check complete results for the latest benchmark here: [Results (2022-11-21)](_posts/2022-11-21-results.md)

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

## Combined results

<canvas id="chart" style="margin-bottom: 2em"></canvas>
<script>
    var ctx = document.getElementById('chart').getContext('2d');
    var myChart = new Chart(ctx, {
        type: 'bar',
        data: {
            labels: ['blacksheep','muffin','sanic','falcon','starlette','baize','fastapi','aiohttp','tornado','quart','django',],
            datasets: [
                {
                    label: '# of requests',
                    data: ['526320','475020','468810','440055','359700','353775','248430','193440','124425','113880','45615',],
                    backgroundColor: [
                        '#4E79A7', '#A0CBE8', '#F28E2B', '#FFBE7D', '#59A14F', '#8CD17D', '#B6992D', '#F1CE63', '#499894', '#86BCB6', '#E15759', '#FF9D9A', '#79706E', '#BAB0AC', '#D37295', '#FABFD2', '#B07AA1', '#D4A6C8', '#9D7660', '#D7B5A6',
                    ]
                },
            ]
        }
    });
</script>

Sorted by sum of completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 526320 | 5.48 | 9.52 | 7.0
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 475020 | 6.41 | 11.12 | 8.13
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 468810 | 7.72 | 9.98 | 7.72
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 440055 | 7.54 | 13.0 | 9.64
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 359700 | 10.46 | 18.13 | 13.17
| [baize](https://pypi.org/project/baize/) `0.18.2` | 353775 | 11.82 | 13.62 | 12.24
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 248430 | 12.76 | 22.74 | 16.47
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 193440 | 18.73 | 18.86 | 18.78
| [tornado](https://pypi.org/project/tornado/) `6.2` | 124425 | 24.02 | 24.14 | 24.07
| [quart](https://pypi.org/project/quart/) `0.18.3` | 113880 | 27.39 | 28.19 | 27.35
| [django](https://pypi.org/project/django/) `4.1.3` | 45615 | 61.39 | 64.84 | 64.83


More details: [Results (2022-11-21)](_posts/2022-11-21-results.md)