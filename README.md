# Elasticsearch date_histogram aggregations benchmark

## Intro

This project is an ES Rally benchmark that measures performance of the `date_histogram` aggregation
for various workloads.

It was created to reproduce a performance issue reported Elastic [forums](https://discuss.elastic.co/t/slow-date-histogram-after-upgrading-to-7-3-0-on-dense-indexes/196475) and [GitHub](https://github.com/elastic/elasticsearch/issues/45702).

Test datasets have been uploaded [here](https://github.com/csoulios/date_histogram-benchmark/releases/tag/1.0).

## Run benchmarks

To run the benchmarks

1. Install the latest version of Rally, as described in the official Rally [documentation](https://esrally.readthedocs.io/en/stable/install.html).

2. [Configure Rally](https://esrally.readthedocs.io/en/stable/install.html#next-steps) using `esrally configure`.

3. Edit `~/.rally/rally.ini` and add the `data_histogram-benchmark` track in the `[tracks]` section as shown below (more details in the [Rally docs](https://esrally.readthedocs.io/en/stable/recipes.html?highlight=track-repository#changing-the-default-track-repository)):

    ```
    [tracks]
    default.url = https://github.com/elastic/rally-tracks
    date_histogram-benchmark.url = https://github.com/csoulios/date_histogram-benchmark
    ```

4. Run rally track with any of the supported challenges.

```
esrally --on-error=abort --track-repository=date_histogram-benchmark --distribution-version=[elasticsearch_version] --track date_histogram --challenge=[challenge_name]
```

A different challenge has been created for loading each of the datasets with different distributions of documents in time:

* **timestamps-gaussian-sameday**: this dataset represents the actual distribution of log data during a production day. It is a gaussian distribution centered around lunch time (more documents during the day than the night). All documents fit within the same day.
* **timestamps-uniform-sameday**: All documents fit within the same day but are evenly distributed (same amount of docs every hours).
* **timestamps-uniform-1s**: Documents are spaced a second apart (the first starts at 2000-01-01T00:00:00.000Z, next is 1 second later).
* **timestamps-uniform-10s**: 10 second gap between documents.


## Acknowledgements

Special thanks to Bertrand Renuart for reporting this issue and creating the benchmark dataset.
