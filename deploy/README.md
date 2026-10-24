# hk deployment

The Compose app reuses the shared `web-proxy` Docker network and keeps its Metric cache in a named volume. It publishes port 8767 only on that network; the shared Caddy container terminates TLS and proxies to it.

Running `start.py` locally still binds to `127.0.0.1` and rejects unknown `Host` headers. The public host is enabled only when `METRIC_PUBLIC_ORIGIN` is set.

On `hk`, put the repository at `/opt/metric-lp-guide`, then run:

```bash
cd /opt/metric-lp-guide
docker compose -f deploy/docker-compose.yml up -d --build
```

The shared Caddyfile at `/opt/nsdq-fund-monitor/Caddyfile` needs this site block:

```caddyfile
metric.dadaowuwei.ccwu.cc {
	reverse_proxy metric-lp:8767
}
```

Validate and reload Caddy after editing the shared file:

```bash
docker exec nsdq-fund-monitor-caddy-1 caddy validate --config /etc/caddy/Caddyfile
docker exec nsdq-fund-monitor-caddy-1 caddy reload --config /etc/caddy/Caddyfile
```

Certificates are managed automatically by the existing Caddy container.

## Environment variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `METRIC_LOCAL_PORT` | `8767` | Listening port |
| `METRIC_BIND_HOST` | `127.0.0.1` | Listening address; use `0.0.0.0` inside a container |
| `METRIC_PUBLIC_ORIGIN` | empty | Public HTTPS origin added to the `Host` / `Origin` allowlist |
