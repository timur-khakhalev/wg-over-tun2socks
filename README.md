# wg-over-tun2socks

This project connects incoming WireGuard clients directly to an Xray
VLESS outbound using Xray's native `wireguard` inbound. The previous
`tun2socks` and `wg-easy` components have been removed to reduce
overhead.

## Instructions

### Prerequisites
1. A server with Docker installed.
2. Credentials for a remote Xray/VLESS endpoint (public key, port, SNI,
   etc.).

### Setup
1. Copy `.env.example` to `.env` and adjust the variables if needed. The
   defaults use the `teddysun/xray` image, container name `xray`, and
   expose WireGuard on UDP port `51820`.
2. Edit `config.json`:
   - replace `"<YOUR_SERVER_PRIVATE_KEY>"` with the WireGuard private key
     for this server.
   - replace `"<YOUR_CLIENT_PUBLIC_KEY>"` with the public key for a client
     allowed to connect.
   - fill in the VLESS outbound placeholders (`<YOUR_VLESS_IP>`, port,
     UUID, flow, public key, short ID and server name) with the values
     from your Xray server.
3. `docker-compose.yml` exposes the UDP port defined by `WG_PORT` for
   WireGuard (default `51820`). Adjust if necessary and add more peers in
   `config.json` as required.

### Run

```bash
docker compose up -d
```

If everything is configured correctly, WireGuard clients connecting to
this server will have their traffic forwarded through the specified
VLESS tunnel.
