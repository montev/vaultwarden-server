# Vaultwarden Self-Hosted (Fly.io)

This repository contains configuration for running Vaultwarden (Bitwarden-compatible password manager) on Fly.io with HTTPS and a custom domain.

## Features
- Vaultwarden server via Fly.io
- HTTPS via Let's Encrypt
- Custom domain: https://vault.vangenwood.com
- 2FA via authenticator app
- Auto-sleep to reduce cost
- DuckDNS or custom DNS (Domeneshop)

## Files
- `fly.toml`: Fly.io configuration
- `README.md`: You are here

## Deploy
```bash
fly deploy
