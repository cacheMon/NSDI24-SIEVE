<img width="300" src="doc/diagram/logo.svg" alt="sieve Logo" />

<hr/>

SIEVE is an eviction algorithm simpler than LRU for web caches.
- It is easy to implement and can be easily integrated into existing systems.
- It achieves state-of-the-art efficiency on skewed workloads.
- It can facilitate the design of advanced eviction algorithms.

Please check out NSDI'24 [paper](https://yazhuozhang.com/assets/publication/nsdi24-sieve.pdf) for more details.


## Repo Structure
- [Simulator](#install-libcachesim): a snapshot of [libCacheSim](https://github.com/1a1a11a/libCacheSim)
- [Traces](#traces)
- [Prototypes](#prototypes)

## Usage

### Install libCacheSim
```bash
cd scripts && bash install_dependency.sh && bash install_libcachesim.sh;
```

After building and installing libCacheSim, `cachesim` should be in the `_build/bin/` directory. You can use `cachesim` to run cache simulations.
```bash
# ./cachesim DATAPATH TRACE_FORMAT EVICTION_ALGO CACHE_SIZE [OPTION...]
# Run a single cache simulation
./_build/bin/cachesim data/trace.oracleGeneral.bin oracleGeneral sieve 1gb
# Run multiple cache simulations with different algorithms and differnt cache sizes (when CACHE_SIZE is 0)
./_build/bin/cachesim data/trace.oracleGeneral.bin oracleGeneral fifo,lru,clock,sieve 0 --ignore-obj-size 1
```
Results are recored at `result/TRACE_NAME`. (More detailed instructions can be found at [libCacheSim](https://github.com/1a1a11a/libCacheSim).)

### Quick Tryout with Zipfian Data
We offer a small example trace in `mydata/zipf/`, and you can test the miss ratio for varied algorithms.
```bash
python3 libCacheSim/scripts/plot_mrc_size.py --tracepath mydata/zipf/zipf_1.0 --trace-format txt --algos=fifo,lru,clock,sieve
```
<div style="text-align: left;">
  <img src="/doc/diagram/mr_zipf.png" alt="diagram" width="480"/>
</div>

Feel free to generate different zipfian traces and try out.
```bash
python3 libCacheSim/scripts/data_gen.py -m 10000 -n 1000000 --alpha 1.0 > mydata/zipf/zipf_1.0
```


## Traces
The traces we used in the paper can be downloaded in the following.
| Trace         | Approx Time | # traces | cache type | # request (million) | # object (million) |
|---------------|-------------|----------|------------|---------------------|--------------------|
| [Tencent Photo](https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/cacheDatasets/tencentPhoto/) | 2018        | 2        | object     | 5,650               | 1,038              |
| [Wiki CDN](https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/cacheDatasets/wiki/)      | 2019        | 3        | object     | 2,863               | 56                 |
| [Twitter KV](https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/cacheDatasets/twitter/)    | 2020        | 54       | KV         | 195,441             | 10,650             |
| [Meta KV](https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/cacheDatasets/metaKV/)       | 2022        | 5        | KV         | 1,644               | 82                 |
| [Meta CDN](https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/cacheDatasets/metaCDN/)      | 2023        | 3        | object     | 231                 | 76                 |


## Prototypes
We chose the most popular cache libraries/systems from five different languages: C++, Go, JavaScript, Python, and Rust, and replaced the LRU with SIEVE.
- [groupcache](https://github.com/cacheMon/groupcache) (Golang): 21 lines of code change.
- [mnemonist](https://github.com/cacheMon/mnemonist) (Javascript): 12 lines of code change.
- [lru-rs](https://github.com/cacheMon/lru-rs) (Rust): 16 lines of code change.
- [lru-dict](https://github.com/cacheMon/lru-dict) (Python + C): 21 lines of code change.
- [cachelib](https://github.com/Thesys-lab/cachelib-sosp23) (C++):  Cachelib is highly optimized for LRU-based algorithms. Many optimizations are not needed for SIEVE, making it impractical to quantify code modifications for integration with SIEVE.


### Reference
```bibtex
@inproceedings{zhang2024-sieve,
  title={SIEVE is Simpler than LRU: an Efficient Turn-Key Eviction Algorithm for Web Caches},
  author={Zhang, Yazhuo and Yang, Juncheng and Yue, Yao and Vigfusson, Ymir and Rashmi, K.V.},
  booktitle={USENIX Symposium on Networked Systems Design and Implementation (NSDI'24)},
  year={2024}
}
```