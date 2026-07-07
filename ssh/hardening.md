# SSH Hardening

## Overview

This document describes the SSH hardening model used by `vpshub`.

The hardening configuration is designed to reduce the SSH attack surface while preserving reliable administrative access to the VPS.

SSH hardening must be applied only after a dedicated unprivileged administrative user has been created, key-based authentication has been verified, and administrative access through `sudo` has been confirmed.

The reusable SSH configuration is provided in:

```text
ssh/10-vpshub-hardening.conf
```

The configuration is intended to be deployed as an OpenSSH server configuration drop-in under `/etc/ssh/sshd_config.d/`.

## Prerequisites

Before SSH hardening is applied, the following conditions must be satisfied:

- A dedicated unprivileged administrative user exists
- The administrative user has explicitly granted `sudo` privileges
- The administrative user's public SSH key is installed in the appropriate `authorized_keys` file
- SSH key-based authentication has been successfully tested in a separate session
- Administrative privilege escalation through `sudo` has been successfully tested
- An existing administrative session remains open during configuration changes and validation

SSH access restrictions must not be applied until an alternative, verified administrative access path is available.

## Hardening Controls

The `vpshub` SSH hardening configuration applies the following controls:

- Disable direct SSH login for the root account
- Disable SSH password authentication
- Explicitly enable public key authentication
- Reduce the maximum number of authentication attempts
- Disable X11 forwarding

The reusable configuration contains:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
X11Forwarding no
```

These controls reduce unnecessary authentication exposure while preserving key-based administrative access.

## Deployment

The reusable configuration file should be deployed to:

```text
/etc/ssh/sshd_config.d/10-vpshub-hardening.conf
```

The deployed file should contain only the intended SSH hardening directives.

The main `/etc/ssh/sshd_config` file should not be modified when a configuration drop-in can provide the required behavior.

Before the SSH service configuration is reloaded, the complete server configuration must be validated.

## Configuration Validation

The OpenSSH server configuration should be checked for syntax errors before it is applied.

The effective configuration should also be inspected to confirm that the intended directives are active.

Validation should confirm:

- `PermitRootLogin` is set to `no`
- `PasswordAuthentication` is set to `no`
- `PubkeyAuthentication` is set to `yes`
- `MaxAuthTries` is set to `3`
- `X11Forwarding` is set to `no`

Configuration changes must not be applied if syntax validation fails or the effective configuration differs from the intended security policy.

## Service Reload

After successful configuration validation, the SSH service configuration should be reloaded.

A reload is preferred over an unnecessary service restart when the running service supports safe configuration reloading.

Existing administrative sessions should remain open while the new configuration is tested.

## Access Validation

After the SSH service configuration has been reloaded, access must be tested using a new independent SSH session.

Validation should confirm:

- A new SSH session can be established using the dedicated administrative user
- Public key authentication succeeds
- Administrative privilege escalation through `sudo` remains functional
- A new direct SSH login attempt as root is rejected

Existing administrative sessions should not be closed until the positive and negative access tests have completed successfully.

## Security Considerations

Private SSH keys must never be stored in the repository.

The repository should contain only reusable configuration, documentation, templates, and other non-sensitive infrastructure components.

SSH port changes and additional forwarding restrictions should be introduced only when required by the documented network architecture and after their effect on remote access, VPN connectivity, and infrastructure integrations has been evaluated.

The SSH hardening configuration should remain minimal, explicit, auditable, and reproducible.

## Rollback and Recovery

An existing verified administrative session should remain open throughout SSH hardening deployment and validation.

If configuration validation fails, the SSH service configuration must not be reloaded.

If new administrative access fails after the configuration is reloaded, the existing session should be used to inspect the effective configuration, correct the problem, and repeat validation.

Provider console access may serve as an additional recovery path, but it should not replace careful staged deployment and access validation.
