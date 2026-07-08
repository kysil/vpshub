# Host Firewall

## Overview

This document defines the host firewall baseline used by `vpshub`.

The host firewall provides local network traffic filtering on the VPS and forms one layer of the project's defense-in-depth security architecture.

The host firewall operates independently of provider-level firewall controls. Provider-level and host-level firewall policies should complement each other without being treated as interchangeable security mechanisms.

## Firewall Platform

`vpshub` uses `firewalld` as the host firewall management framework.

The underlying packet filtering implementation is provided by `nftables`.

The `nftables` ruleset generated and managed by `firewalld` should not be modified directly unless a documented infrastructure requirement cannot be implemented safely through `firewalld`.

The `firewalld` service should be enabled and active during normal system operation.

## Baseline Principles

The host firewall baseline follows these principles:

- Maintain a default-deny inbound security posture
- Permit only explicitly required inbound network services
- Keep firewall configuration minimal and auditable
- Use `firewalld` as the authoritative host firewall manager
- Avoid direct modification of the managed `nftables` ruleset
- Separate provider-level and host-level firewall responsibilities
- Validate firewall state after configuration changes
- Verify remote administrative access before closing existing sessions
- Introduce additional network access only when required by documented infrastructure components

## Firewall Zone

The baseline uses the `public` zone as the default firewall zone.

The active external network interface should be associated with the expected firewall zone through the supported network management and `firewalld` integration mechanisms.

The effective zone assignment should be validated during deployment and after significant network configuration changes.

The baseline should not depend on deployment-specific interface names unless explicitly required by the target environment.

## Allowed Services

The baseline permits only services required for initial system administration and supported network operation.

The initial allowed service set consists of:

- `ssh`
- `dhcpv6-client`

SSH access is required for remote system administration.

The `dhcpv6-client` service may remain enabled where required by the supported IPv6 network configuration and operating system defaults.

Additional services and ports must not be opened without a defined infrastructure requirement.

Future services, including VPN endpoints, should be introduced only as part of their respective documented deployment stages.

## Unnecessary Network Services

Network services that are not required by the intended role of the VPS should not remain exposed.

Default operating system installations may include enabled services or socket-activated components that are unnecessary for `vpshub`.

Unnecessary network listeners should be identified, evaluated, and disabled when they have no defined operational purpose.

The baseline validation process should include inspection of listening TCP and UDP sockets.

## Runtime and Permanent Configuration

`firewalld` maintains runtime and permanent configuration states.

Firewall changes intended to survive service reloads and system reboots must be applied to the permanent configuration.

The effective runtime configuration should remain consistent with the intended permanent configuration.

After firewall changes:

- Validate the configuration
- Apply or reload the intended configuration safely
- Verify the active zone
- Compare runtime and permanent firewall state
- Inspect listening network sockets
- Confirm that required administrative access remains operational

## Remote Access Safety

Firewall changes affecting remote administration must be deployed carefully.

An existing verified administrative session should remain open while firewall changes are applied and validated.

After significant firewall or SSH changes, a new independent administrative session should be opened and tested before the existing session is closed.

Validation should confirm:

- Successful key-based SSH authentication
- Access through the expected administrative account
- Functional `sudo` privilege escalation
- Active host firewall state
- Continued access through all required network security layers

Provider console access may serve as an additional recovery mechanism, but it should not replace staged deployment and validation.

## Forwarding and Routing

Packet forwarding, routing, and masquerading requirements should be evaluated as part of the documented network architecture.

The initial host firewall baseline should avoid premature restrictions that could conflict with future VPN routing requirements.

Forwarding behavior should be reviewed and configured explicitly when WireGuard and inter-network routing are introduced.

Any changes to forwarding or masquerading should be documented as part of the corresponding infrastructure deployment stage.

## Provider Firewall Integration

The host firewall is one layer of the overall network security architecture.

Provider-level firewall controls should apply coarse-grained network access restrictions before traffic reaches the VPS.

The host firewall should provide an additional local enforcement layer.

The two firewall layers should be configured and documented separately.

A failure, configuration error, or policy change in one firewall layer should not automatically eliminate the protections provided by the other layer.

## Validation

The host firewall baseline should be validated before additional network services are deployed.

Validation should confirm:

- Valid `firewalld` configuration
- Enabled and active `firewalld` service
- Expected default firewall zone
- Expected active zone assignment
- Consistent runtime and permanent configuration
- Only explicitly required services and ports are allowed
- No unexpected TCP or UDP listeners are present
- The effective `nftables` ruleset is managed by `firewalld`
- Existing administrative access remains operational
- A new independent administrative session can be established successfully

Validation results should be reviewed before proceeding with additional infrastructure deployment stages.

## Security Considerations

Firewall rules should remain minimal, explicit, reproducible, and auditable.

Deployment-specific addresses, credentials, private keys, and other sensitive information must not be committed to the repository.

Firewall access should not be expanded for convenience.

Every additional service, port, forwarding rule, or network policy should have a documented operational purpose.

Changes to the host firewall baseline should be validated against the broader network architecture, including provider-level firewall controls, VPN connectivity, routing requirements, and infrastructure integrations.

## Rollback and Recovery

An existing verified administrative session should remain available during firewall configuration changes.

If configuration validation fails, the intended firewall changes should not be treated as successfully deployed.

If new administrative access fails after a firewall change, the existing session should be used to inspect the effective configuration, restore required access, and repeat validation.

Provider console access may be used as an additional recovery path when normal remote administration is unavailable.
