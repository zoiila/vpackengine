# Delivery and hosting

## External HTTPS (default)

Publish the exact validated ZIP bytes to an administrator-controlled CDN/web server at an immutable URL. Set exact hostnames in `remote.allowed-hosts`, including every allowed redirect destination; wildcards, private/link-local/loopback/multicast destinations and credential-bearing URLs are rejected. Imports use HTTPS port 443, at most three redirects, bounded connection/read timeouts, an overall stream deadline and bounded bytes. The target server's DNS and egress controls must be trusted; enforce outbound firewall rules when hosting domains are not under administrator control to prevent DNS rebinding between validation and connection.

No automatic retries occur during remote import; the finite application retry policy is separate. Remote source URLs and redirect locations may carry CDN query tokens; store only administrator-safe links, protect registry backups, and avoid secrets in public examples.

## Embedded HTTP behind an HTTPS reverse proxy

Disabled by default. Configure all fields before enabling:

```yaml
hosting:
  enabled: true
  bind: '127.0.0.1'
  port: 8091
  public-base-url: 'https://packs.example.com'
```

This deliberately binds the backend to loopback while publishing a distinct public HTTPS origin. Do not publish `localhost` as the player URL. Configure DNS, firewall and proxy routing, and test from a separate client network. The plugin cannot infer reachability from its bind address. Public origin must have no path prefix.

Nginx example:

```nginx
server {
  listen 443 ssl;
  server_name packs.example.com;
  ssl_certificate /etc/letsencrypt/live/packs.example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/packs.example.com/privkey.pem;
  client_max_body_size 1k;
  location /packs/ {
    limit_except GET HEAD { deny all; }
    proxy_pass http://127.0.0.1:8091;
    proxy_read_timeout 60s;
    proxy_send_timeout 60s;
    proxy_request_buffering off;
  }
  location / { return 404; }
}
```

Only registered SHA-256-named validated artifacts are served. GET/HEAD, Content-Length, ETag/If-None-Match, immutable Cache-Control, Accept-Ranges, single ranges and If-Range are supported. Multiple/invalid ranges return 416. There is no upload or listing endpoint. Four streaming workers, 32 queued exchanges, connection limits, JDK header timeouts and a 60-second exchange watchdog bound resource use. The JDK HttpServer timeout properties are process-wide; a server with another embedded HttpServer must coordinate these values or prefer external hosting.

Imported files remain under `artifacts/<sha256>.zip`; failed data and reasons go under `failed/`, outside the served directory. Keep filesystem permissions limited to trusted administrators. Cleanup of old artifacts/failed imports is an offline operator action after reviewing still-published immutable URLs. The plugin never rewrites published bytes.

`doctor` reports listener health and checks local artifacts. `validate` compares external delivery bytes. Embedded client reachability and HTTPS proxy setup need a real remote client check. When embedded hosting is disabled, local imports need a verified external delivery URL.
