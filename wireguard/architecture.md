# WireGuard Architecture

## Overview

This document defines the WireGuard architecture used by `vpshub`.

WireGuard provides the secure VPN transport layer between the VPSHub and authorized peers.

The VPSHub acts as a central VPN hub that can establish secure tunnels with remote routers, client devices, trusted networks, and other authorized infrastructure systems.

The WireGuard architecture is designed around explicit peer relationships, controlled routing, minimal network exposure, clear trust boundaries, and separation between reusable infrastructure documentation and deployment-specific private data.

## Architecture Principles

The WireGuard architecture follows these principles:

- Use the VPSHub as the central VPN connectivity point
- Define every peer explicitly
- Assign every peer a documented operational purpose
- Keep routing policy explicit and auditable
- Permit only required network reachability
- Separate VPN transport from access-control policy
- Maintain independent provider-level and host-level firewall controls
- Keep private keys outside version control
- Keep deployment-specific peer configurations outside the public repository
- Validate connectivity and routing incrementally
- Introduce forwarding only when required by the documented architecture

## VPSHub Role

The VPSHub acts as the central WireGuard routing and connectivity hub.

Its responsibilities may include:

- Terminating WireGuard tunnels
- Authenticating authorized peers through public-key cryptography
- Providing connectivity between explicitly authorized peers
- Routing traffic between approved networks
- Supporting remote administrative and infrastructure access
- Enforcing host-level network policy together with the host firewall

The VPSHub should not provide unrestricted connectivity between peers by default.

Connectivity between peers and networks should be introduced only when required by the intended infrastructure architecture.

## Peer Model

Every WireGuard peer should represent a defined device, router, network, or infrastructure system.

Peers may include:

- Router peers
- Remote client peers
- Trusted infrastructure peers
- Other explicitly authorized systems

Each peer should have:

- A defined operational purpose
- A unique WireGuard public key
- Explicit tunnel addressing
- Explicit routing requirements
- Explicit access requirements
- A documented trust boundary

Private keys must remain on the systems that use them and must never be committed to the public repository.

## Router Peers

Router peers may provide connectivity between the VPSHub and trusted remote networks.

A router peer may represent:

- A home network gateway
- A travel router
- Another trusted network gateway

Router peers may advertise or route traffic for networks located behind them.

Network reachability through router peers should be explicitly defined and should not be inferred solely from successful WireGuard tunnel establishment.

## Client Peers

Client peers represent individual authorized devices.

Examples may include:

- Mobile devices
- Workstations
- Administrative systems

Client peers should receive only the network access required for their intended purpose.

Client access should remain independent from router-peer access policy where practical.

## Tunnel Addressing

WireGuard tunnel addressing should use a dedicated address space that does not overlap with:

- Provider networks
- VPS external addressing
- Connected local networks
- Remote routed networks
- Other infrastructure address spaces

Tunnel addresses should be assigned explicitly and documented outside the public repository when they are deployment-specific.

Address allocation should remain stable, predictable, and reproducible.

## Routing Model

The routing model should remain explicit.

WireGuard tunnel establishment does not by itself authorize unrestricted routing between peers or networks.

Routing decisions should account for:

- Tunnel address assignments
- Peer AllowedIPs configuration
- Operating system forwarding state
- Host firewall forwarding policy
- Connected network routes
- Return-path requirements

Routing should be introduced incrementally and validated after each significant change.

## IP Forwarding

Operating system IP forwarding should remain disabled until routing between WireGuard peers or connected networks is required.

When forwarding is required, it should be enabled explicitly and documented as part of the WireGuard deployment architecture.

Forwarding configuration should be coordinated with host firewall policy.

Enabling IP forwarding does not by itself authorize traffic between networks.

## Firewall Integration

WireGuard network exposure should be controlled independently at both provider and host firewall layers.

The provider firewall should permit only the required WireGuard transport traffic.

The host firewall should:

- Permit required WireGuard transport traffic
- Control traffic entering the WireGuard interface
- Control forwarded traffic between authorized networks
- Prevent unintended network reachability

Firewall policy should evolve together with the WireGuard routing architecture.

## Deployment Sequence

WireGuard deployment should proceed incrementally.

The deployment sequence should include:

- Validate the operating system baseline
- Validate system minimization state
- Validate provider and host firewall state
- Install required WireGuard tooling
- Define the WireGuard architecture
- Define the tunnel addressing model
- Define peer roles and routing requirements
- Generate deployment-specific cryptographic keys
- Create the WireGuard interface configuration
- Introduce required firewall access
- Enable required forwarding only when needed
- Activate the WireGuard interface
- Validate tunnel establishment
- Validate routing and access policy
- Validate system state after deployment

Each stage should be validated before proceeding to the next stage.

## Public and Private Configuration

The public repository may contain:

- Architecture documentation
- Configuration templates
- Validation procedures
- Reusable administrative scripts
- Non-sensitive examples

The public repository must not contain:

- Private keys
- Pre-shared keys
- Deployment-specific peer configurations
- Deployment-specific addresses
- Credentials
- Secrets
- Sensitive infrastructure identifiers

Private deployment data should be maintained separately from the public repository.

## Validation

WireGuard deployment validation should include, where applicable:

- Required package availability
- WireGuard interface state
- WireGuard peer state
- Tunnel establishment
- Handshake state
- Routing table state
- IP forwarding state
- Provider firewall policy
- Host firewall policy
- Listening network sockets
- Expected network reachability
- Unexpected network reachability
- Failed system units
- Package manager consistency

Validation results should be reviewed before additional peers, routes, or network services are introduced.

## Security Considerations

WireGuard provides secure VPN transport but should not be treated as a complete access-control system.

Peer authentication, routing, forwarding, provider firewall policy, and host firewall policy form separate security controls.

Successful tunnel establishment should not automatically provide unrestricted access to the VPSHub, other peers, or connected networks.

Every peer, route, forwarding path, and network access rule should have a documented operational purpose.

## Change Management

WireGuard architecture changes should remain explicit, reviewable, and reproducible.

Significant changes should be documented when they affect peer relationships, tunnel addressing, routing, forwarding, firewall policy, or trust boundaries.

Repository changes should be validated before commit.

WireGuard architecture documentation should evolve together with the broader `vpshub` architecture and should be reviewed when new peers, networks, or infrastructure requirements are introduced.
