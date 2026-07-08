# Provider Firewall

## Overview

This document defines the provider-level firewall model used by `vpshub`.

The provider firewall provides network traffic filtering outside the VPS operating system and forms the external layer of the project's defense-in-depth security architecture.

The provider firewall operates independently of the host firewall. Provider-level and host-level firewall policies should complement each other without being treated as interchangeable security mechanisms.

## Firewall Platform

`vpshub` uses the firewall service provided by the VPS infrastructure provider as the provider-level network filtering layer.

The provider firewall should be attached to the VPS network interface and remain operational independently of the host operating system firewall.

Provider-specific implementation details may vary, but the security model should remain consistent with the architecture defined by this project.

## Baseline Principles

The provider firewall baseline follows these principles:

- Maintain a default-deny inbound security posture
- Permit only explicitly required inbound network traffic
- Keep firewall rules minimal and auditable
- Separate provider-level and host-level firewall responsibilities
- Avoid exposing services that are not required by the infrastructure architecture
- Validate provider firewall policy before and after significant changes
- Preserve verified administrative access during firewall changes
- Introduce additional network access only when required by documented infrastructure components

## Inbound Policy

The default inbound policy should deny traffic that is not explicitly permitted.

Inbound access should be granted only for services with a defined operational purpose.

The baseline inbound policy should account for:

- Remote administrative access
- Required network control traffic
- VPN transport traffic when the VPN infrastructure is deployed
- Additional infrastructure services only when explicitly documented

Rules should remain as narrow as practical with respect to protocol, port, address family, and source network.

## Outbound Policy

The outbound policy should support required operating system administration, package management, time synchronization, name resolution, infrastructure communication, and other documented operational requirements.

Outbound restrictions should not be introduced without evaluating their effect on system administration, updates, security services, and deployed infrastructure components.

## IPv4 and IPv6

Provider firewall policy should account explicitly for both IPv4 and IPv6 when both address families are enabled on the VPS.

Security policy should not assume that restricting one address family automatically restricts the other.

Required services should be evaluated independently for IPv4 and IPv6 exposure.

Unused or unintended exposure through either address family should be prevented.

## Administrative Access

Remote administrative access should remain explicitly permitted through the provider firewall.

Administrative access rules should be validated before restrictive firewall changes are treated as successfully deployed.

An existing verified administrative session should remain available during significant provider firewall changes whenever practical.

Provider console access may be used as an additional recovery path when normal remote administration is unavailable.

## Host Firewall Relationship

The provider firewall and host firewall form separate security layers.

The provider firewall filters traffic before it reaches the VPS operating system.

The host firewall controls traffic at the operating system level.

The security architecture should not rely exclusively on either layer.

Rules should be coordinated so that required traffic is permitted through both layers while unnecessary traffic remains denied.

## VPN Integration

VPN transport traffic should not be permitted until the corresponding VPN infrastructure component is ready for deployment.

When VPN services are introduced:

- The required transport protocol and port should be explicitly documented
- Provider firewall rules should be updated deliberately
- Host firewall rules should be updated separately
- IPv4 and IPv6 requirements should be evaluated
- Routing and forwarding requirements should be validated
- Network exposure should be reviewed after deployment

VPN deployment should not require weakening the default-deny inbound security model.

## Validation

Provider firewall state should be validated before and after significant network architecture changes.

Validation should include, where applicable:

- Firewall attachment to the expected VPS
- Default inbound policy
- Default outbound policy
- Explicit inbound rules
- IPv4 policy
- IPv6 policy
- Administrative access
- Required infrastructure traffic
- Coordination with the host firewall
- Unexpected network exposure

Validation results should be reviewed before additional network services are deployed.

## Security Considerations

The provider firewall is one component of the broader `vpshub` defense-in-depth security architecture.

Provider firewall rules should remain minimal, explicit, reproducible, and auditable.

Firewall access should not be expanded for convenience.

Every additional service, protocol, port, or source network should have a documented operational purpose.

Deployment-specific addresses, credentials, private keys, and other sensitive information must not be committed to the repository.

## Change Management

Provider firewall changes should remain explicit, reviewable, and reproducible.

Significant changes should be documented when they affect network exposure, administrative access, VPN connectivity, routing, or the broader security architecture.

Repository changes should be validated before commit.

Provider firewall documentation should evolve together with the broader `vpshub` architecture and should be reviewed when new infrastructure components introduce additional network requirements.
