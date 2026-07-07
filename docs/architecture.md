# Architecture

## Overview

`vpshub` is a security-focused and reproducible VPS infrastructure project.

The architecture is designed around layered security, explicit trust boundaries, minimal network exposure, and clear separation between reusable public infrastructure components and deployment-specific private data.

The VPS acts as a secure networking hub that can provide remote access, VPN connectivity, and infrastructure services.

## Architecture Principles

The project follows these core principles:

- Default-deny network access
- Defense in depth
- Least privilege
- Key-based authentication
- Explicit trust boundaries
- Minimal service exposure
- Separation of public templates and private deployment data
- Reproducible configuration
- Auditable infrastructure changes
- Automation where practical and safe

## Infrastructure Layers

The `vpshub` architecture consists of several independent security and infrastructure layers.

### 1. VPS Provider

The VPS provider supplies the compute instance, networking, storage, and external infrastructure controls.

Provider-specific configuration should remain separate from reusable operating system and application configuration.

### 2. Provider-Level Firewall

The provider-level firewall is the first network security boundary.

Its purpose is to:

- Apply a default-deny inbound policy
- Permit only explicitly required traffic
- Reduce unnecessary exposure before traffic reaches the VPS
- Provide an independent security layer outside the operating system

Provider-level firewall configuration should be documented separately from the host firewall.

### 3. Operating System

The VPS runs Rocky Linux.

The operating system layer provides:

- Package management
- User and privilege management
- System services
- Logging
- Security updates
- Host-level configuration and hardening

System changes should be documented and reproducible.

### 4. Host Firewall

The host firewall provides network filtering directly on the VPS.

It acts independently from the provider-level firewall and forms an additional security boundary.

The host firewall should:

- Follow a default-deny inbound model
- Permit only required services
- Keep rules explicit and auditable
- Remain synchronized with the intended network architecture

### 5. SSH Access

SSH provides administrative access to the VPS.

The SSH security model should use:

- Key-based authentication
- Restricted privileged access
- Least-privilege administration
- Minimal authentication exposure
- Explicit and documented hardening decisions

Private SSH keys must never be stored in the repository.

### 6. WireGuard

WireGuard provides secure VPN connectivity between the VPS and authorized peers.

The WireGuard layer should support:

- Secure peer-to-peer tunnels
- Explicit peer configuration
- Controlled network routing
- Reusable configuration templates
- Separation of private keys from public infrastructure code

Private keys and deployment-specific peer configurations must remain outside version control.

### 7. Peers and Connected Networks

Peers may include:

- Remote client devices
- Routers
- Trusted remote networks
- Other authorized infrastructure systems

Each peer should have an explicit purpose, defined network access, and documented trust boundary.

## Security Boundaries

The architecture uses multiple independent security boundaries:

1. Provider infrastructure controls
2. Provider-level firewall
3. Host operating system
4. Host firewall
5. SSH authentication and authorization
6. WireGuard authentication and routing
7. Peer-specific access controls

No single security layer should be treated as the only protection mechanism.

## Public and Private Configuration

The public `vpshub` repository contains:

- Documentation
- Architecture descriptions
- Configuration examples
- Reusable templates
- Administrative scripts
- Validation tools

The public repository must not contain:

- Private keys
- Credentials
- Secrets
- Private peer configurations
- Deployment-specific addresses
- Sensitive infrastructure identifiers
- Environment-specific authentication data

Private deployment data should be maintained separately from the public repository.

## Repository Structure

```text
vpshub/
├── docs/
├── firewall/
├── scripts/
├── ssh/
├── system/
└── wireguard/
    └── templates/
```
