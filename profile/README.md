# Network Plane

Small, focused networking tools and shared TUI libraries, all in Go. Each service ships as a single binary and uses the same terminal console UX.

## Projects

### Core daemons
- **dhcplane** — DHCPv4 server with JSON config, pools, reservations, sticky leases, live reload, CLI for leases/stats.  
  https://github.com/network-plane/dhcplane
- **dnsplane** — DNS resolver for labs/home. Queries multiple upstreams, prefers authoritative replies, exposes local control via UNIX socket and remote TUI over TCP.  
  https://github.com/network-plane/dnsplane
- **mdnsplane** — mDNS server / proxy / shadow-proxy.  
  https://github.com/network-plane/mdnsplane
- **dhcpdoc** — DHCP server debugging tool.  
  https://github.com/network-plane/dhcpdoc

### Shared libraries
- **planetui** — Composable TUI framework used across Network Plane tools (structured commands, middleware, async tasks, output channels).  
  https://github.com/network-plane/planetui
- **planeconsole** — Monitoring console components used by the platform tools.  
  https://github.com/network-plane/planeconsole

## Design principles
- **One UX everywhere**: common console and command patterns across all tools.
- **Single-binary deploys**: Go 1.21+ builds, no external runtime deps.
- **Readable configs**: strict JSON with validation and clear errors.
- **Operational clarity**: explicit logs, PID files, graceful shutdown, and CLIs for inspection.

## Quick start (example)

```bash
# Build a tool (example: dhcplane)
git clone https://github.com/network-plane/dhcplane
cd dhcplane
go build -o dhcplane .

# Prepare minimal config and leases
cp config.example.json config.json
echo '{"by_ip":{},"by_mac":{}}' > leases.json

# On Linux, allow binding to UDP:67 without root
sudo setcap 'cap_net_bind_service=+ep' "$(pwd)/dhcplane"

# Run with console enabled
./dhcplane serve --console
