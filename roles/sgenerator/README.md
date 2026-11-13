# Role Description

Ansible role for simple sites config generation from the list

## Default Variables

For a comprehensive understanding of default variables, please refer to the `defaults/main.yml` file.

## Read-only HTTP endpoints

Set these keys on individual entries in `nginx_frontend_configurations`:

| Key | Default | Description |
| --- | --- | --- |
| `read_only` | `false` | Enable the policy for an `http` or `https` frontend. Rejected requests receive HTTP 403. |
| `read_only_allowed_methods` | `[GET, HEAD]` | Allowed HTTP methods. An empty list denies all methods for non-whitelisted clients. |
| `read_only_allowed_paths` | `[]` | Case-sensitive Nginx regex allowlist matched against the normalized `$uri`, without query parameters. Omitted/empty means all paths. Anchor regexes with `^` and `$` for exact endpoint matching. |
| `read_only_allow_ips` | `[]` | IPv4/IPv6 addresses or CIDRs that bypass both read-only method and path restrictions. |

For example, a CometBFT RPC frontend can expose only read methods and endpoints:

```yaml
nginx_frontend_configurations:
  - dns: rpc.example.com
    domain: example.com
    mode: http
    backend: "127.0.0.1:26657"
    read_only: true
    read_only_allowed_methods: [GET, HEAD]
    read_only_allowed_paths:
      - '^/(status|health|block|block_by_hash|block_results|blockchain|commit|validators|consensus_params|tx|tx_search|abci_info|abci_query|genesis|genesis_chunked|header|header_by_hash)/?$'
    read_only_allow_ips:
      - "192.0.2.10"

  - dns: lcd.example.com
    domain: example.com
    mode: http
    backend: "127.0.0.1:1317"
    read_only: true
    read_only_allow_ips:
      - "192.0.2.10"
```

With this RPC allowlist, public JSON-RPC `POST /`, transaction broadcasts
(including GET requests), and `/websocket` are denied. LCD allows GET/HEAD on all
paths but rejects POST/PUT/PATCH/DELETE and other methods. OPTIONS is denied by
default; explicitly add it to `read_only_allowed_methods` if CORS preflight is
needed. POST JSON-RPC cannot be made selectively read-only by HTTP method/path
filtering; allowing POST also permits write operations at allowed paths.

The whitelist uses `$remote_addr`, not client-supplied `X-Forwarded-For`.
If Nginx is behind another proxy, configure the real-IP module separately with
only trusted proxy addresses before relying on the whitelist. Whitelisted
clients retain the original proxy behavior, including RPC broadcasts and
WebSocket access. Existing `restrict` rules and other frontend settings still
apply to everyone; the whitelist bypasses only this read-only policy.

The role validates policy types before writing configuration. Nginx must still
validate IP/CIDR values and regex syntax with `nginx -t`; run the collection's
`reload` role after `sgenerator` as usual. Frontends without `read_only: true`
keep their existing behavior.

Run the rendering and policy-validation tests from the collection root
(requires the Python environment used by Ansible):

```bash
python3 -m unittest discover -s tests -p 'test_*.py'
```
