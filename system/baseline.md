# System Baseline

## Overview

This document defines the operating system baseline for `vpshub`.

The baseline describes the minimum system state expected before additional infrastructure components, security controls, and network services are deployed.

Its purpose is to provide a clear, reproducible, and auditable starting point for all subsequent configuration.

## Baseline Principles

The system baseline follows these principles:

- Start from a clean operating system installation
- Apply available system updates before deployment
- Keep the installed software set minimal
- Use unprivileged accounts for routine administration
- Restrict privileged access
- Maintain explicit and documented configuration
- Validate system state before and after significant changes
- Keep deployment-specific secrets and identifiers outside version control

## Operating System

`vpshub` uses Rocky Linux as its operating system platform.

The operating system should be installed from a clean image supplied by the VPS provider or another trusted installation source.

Before additional services are deployed, the system should:

- Have all available package updates applied
- Run a supported Rocky Linux release
- Use the expected system architecture
- Have a valid hostname configured
- Have the system timezone configured appropriately
- Have time synchronization enabled
- Have required administrative tools installed
- Be rebooted when required after significant system updates

The operating system version, kernel version, architecture, hostname, timezone, and time synchronization state should be validated as part of the baseline verification process.

## System Updates

The system should be fully updated before infrastructure services are configured.

Available package updates should be applied using the native Rocky Linux package management tools.

After significant updates, especially kernel or core system package updates, the system should be rebooted when required.

The update process should include:

- Refreshing package metadata
- Applying available package updates
- Reviewing the update transaction for unexpected changes
- Rebooting the system when required
- Verifying the active kernel after reboot
- Confirming that critical system services are operational

System updates should be performed regularly throughout the lifecycle of the VPS.

## Administrative Access

Routine system administration should be performed using an unprivileged user account with explicitly granted administrative privileges.

Direct use of the root account should be minimized.

Administrative access should follow these principles:

- Use a dedicated unprivileged account for routine administration
- Grant elevated privileges only when required
- Use `sudo` for privileged administrative operations
- Keep authentication methods explicit and auditable
- Avoid shared administrative accounts
- Review administrative access periodically
- Remove unnecessary accounts and privileges

SSH-specific authentication and access controls are documented separately in the `ssh/` directory.

## Time and Synchronization

Accurate system time is required for logging, authentication, security monitoring, and reliable operation of infrastructure services.

The system should:

- Use an explicitly configured timezone
- Maintain synchronized system time
- Use the default supported time synchronization service unless there is a documented reason to replace it
- Verify synchronization status as part of baseline validation

Timezone configuration and time synchronization state should be documented and validated without exposing deployment-specific information.

## Required Administrative Tools

Only tools required for deployment, administration, troubleshooting, and validation should be installed.

The baseline software set should remain minimal and should avoid unnecessary packages and services.

Administrative tools may include:

- Version control tools
- Network diagnostic utilities
- Text editors
- Process and system monitoring utilities
- Security and validation tools

Installed tools should have a documented operational purpose.

Additional software should be introduced only when required by a defined infrastructure component or administrative workflow.

## Service Management

Only services required for the intended operation of the VPS should be enabled and running.

System services should be managed using the native service management facilities provided by Rocky Linux.

The service baseline should follow these principles:

- Disable unnecessary services
- Enable services only when they have a defined operational purpose
- Verify service state after configuration changes and system reboots
- Review listening network sockets when introducing new services
- Keep service configuration explicit and auditable
- Investigate unexpected service failures or network listeners

Service state and network exposure should be validated regularly and after significant system changes.

## Logging and Auditing

System logging should provide sufficient information for administration, troubleshooting, security analysis, and validation.

The logging baseline should follow these principles:

- Maintain system and service logs using supported operating system facilities
- Preserve sufficient information to investigate failures and security events
- Review logs when unexpected system behavior occurs
- Keep log access restricted according to the principle of least privilege
- Avoid recording credentials, private keys, or other sensitive authentication data
- Verify logging functionality after significant system changes
- Introduce additional auditing capabilities only when they have a defined operational purpose

Logging and auditing configuration should remain explicit, maintainable, and appropriate for the intended role of the VPS.

## Baseline Validation

The system baseline should be validated before additional infrastructure components and network services are deployed.

Validation should confirm:

- The expected operating system release and architecture
- The active kernel version
- The configured hostname
- The configured timezone
- The system time synchronization state
- The system update state
- The availability of required administrative tools
- The configured administrative access model
- The state of enabled and running services
- The set of listening network sockets
- The operational state of system logging

Validation results should be reviewed for unexpected conditions before proceeding with additional deployment stages.

Baseline validation procedures and reusable validation tools should be documented separately from this baseline definition.
