# vpshub

A security-focused, reproducible VPS infrastructure project built on Rocky Linux.

`vpshub` documents and automates the deployment of a hardened VPS that serves as a secure networking hub for remote access, VPN connectivity, and infrastructure services.

## Project Goals

- Build a secure VPS baseline from a clean Rocky Linux installation
- Apply a default-deny firewall model
- Harden SSH access
- Deploy and configure WireGuard
- Keep secrets and private configuration outside version control
- Document infrastructure decisions and deployment procedures
- Automate repeatable administrative tasks where appropriate

## Repository Structure

- `docs/` — project documentation and architecture notes
- `firewall/` — firewall configuration and documentation
- `ssh/` — SSH hardening configuration
- `wireguard/` — WireGuard configuration templates and documentation
- `scripts/` — deployment and administrative scripts
- `system/` — operating system configuration and hardening

## Security

This repository is designed to contain only public documentation, configuration templates, and reusable automation.

Private keys, credentials, environment files, and deployment-specific secrets must never be committed to the repository.

## Status

Early development.

## License

License information will be added before the first public release.
