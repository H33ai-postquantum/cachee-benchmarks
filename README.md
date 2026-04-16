# Cachee Benchmark Suite

Open-source cache performance benchmark framework for comparing **[Cachee.ai](https://cachee.ai)**, Redis, Memcached, ElastiCache, and other caching solutions.

## Latest Results

| Metric | Cachee | Redis | Memcached | ElastiCache |
|--------|--------|-------|-----------|-------------|
| **GET Latency** | 17 ns | 1.0 ms | 0.8 ms | 1.2 ms |
| **SET Latency** | 22 ns | 1.1 ms | 0.9 ms | 1.3 ms |
| **P99 Latency** | 24 ns | 2.5 ms | 1.8 ms | 3.0 ms |
| **Throughput** | 59M ops/s | ~250K ops/s | ~300K ops/s | ~200K ops/s |
| **Hit Rate** | 98.1% | ~95% | ~93% | ~95% |

> Full methodology and reproduction steps: [cachee.ai/benchmark-methodology](https://cachee.ai/benchmark-methodology)

## Quick Start

```bash
# Clone and run benchmarks
git clone https://github.com/HapPhi/cachee-benchmarks.git
cd cachee-benchmarks
docker-compose up -d
./run-benchmarks.sh
```

## Benchmark Categories

### Latency Benchmarks
- **Single-key GET/SET** — Median, P95, P99, P999
- **Batch operations** — MGET/MSET with 10, 100, 1000 keys
- **Hot-key contention** — Zipfian distribution, 80/20 access pattern

### Throughput Benchmarks
- **Sustained load** — 1, 10, 100, 1000 concurrent connections
- **Burst capacity** — 10x traffic spikes sustained for 60 seconds
- **Mixed workload** — 80% read / 20% write realistic traffic

### Trading-Specific Benchmarks
- **Order lifecycle** — 14-lookup sequence simulating pre-trade risk checks
- **Market data fan-out** — 1:N subscriber reads from shared state
- **Smart order routing** — Venue selection with real-time latency tables

> See [trading solutions](https://cachee.ai/trading) for how Cachee achieves 17ns reads in production trading environments.

### AI/ML Benchmarks
- **KV cache lookup** — LLM inference serving with shared prefix optimization
- **Embedding retrieval** — RAG pipeline feature store access patterns
- **Agent chain** — Multi-model orchestration with 10-50 cache reads per request

> See [AI solutions](https://cachee.ai/ai) for how Cachee eliminates the GPU memory wall.

## Supported Backends

| Backend | Config Key | Notes |
|---------|-----------|-------|
| [Cachee.ai](https://cachee.ai) | `cachee` | L1 in-process + L2 Redis |
| Redis | `redis` | Standalone or Cluster |
| Memcached | `memcached` | Single node |
| AWS ElastiCache | `elasticache` | Redis-compatible |
| Azure Cache | `azure` | Redis-compatible |
| GCP Memorystore | `gcp` | Redis-compatible |
| Upstash | `upstash` | Serverless Redis |

> See all [supported integrations](https://cachee.ai/integrations)

## Configuration

```yaml
# benchmark-config.yaml
backends:
  cachee:
    host: cachee-proxy
    port: 6380
  redis:
    host: redis
    port: 6379

scenarios:
  - name: single-key-get
    operations: 1000000
    concurrency: 100
    key_distribution: zipfian
```

## Results Output

Results are written to `results/` as JSON and rendered as Markdown tables. See [full benchmark results](https://cachee.ai/benchmarks) for production numbers.

## Contributing

1. Fork the repo
2. Add your backend adapter in `backends/`
3. Run `./run-benchmarks.sh --backend=yours`
4. Open a PR with results

## Links

- **Website**: [cachee.ai](https://cachee.ai)
- **Documentation**: [cachee.ai/docs](https://cachee.ai/docs)
- **Benchmarks**: [cachee.ai/benchmarks](https://cachee.ai/benchmarks)
- **How It Works**: [cachee.ai/how-it-works](https://cachee.ai/how-it-works)
- **Pricing**: [cachee.ai/pricing](https://cachee.ai/pricing)
- **Blog**: [cachee.ai/blog](https://cachee.ai/blog)

## Used in Production

| Product | Use Case | Result |
|---------|----------|--------|
| [RevMine.ai](https://revmine.ai) | Token mining hot path — trust scores, leaderboards, swap quotes | <1µs cache hit, 1M entries, CacheeLFU eviction |
| [V100.ai](https://v100.ai) | Video transcoding pipeline cache | Sub-microsecond frame metadata lookups |
| [Auth1.ai](https://auth1.ai) | Session and rate limit cache | In-process, zero Redis in auth path |

## License

Apache 2.0