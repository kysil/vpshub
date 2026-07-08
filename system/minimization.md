# System Minimization

## Overview

This document defines the operating system minimization model used by `vpshub`.

System minimization reduces unnecessary software, services, network exposure, and operational complexity while preserving the components required for reliable administration and intended infrastructure functions.

Minimization should be performed deliberately and incrementally. Packages and services should not be removed solely because they appear unnecessary without first evaluating their dependencies, operational purpose, and effect on the broader system architecture.

## Minimization Principles

The system minimization model follows these principles:

- Maintain only software and services with a defined operational purpose
- Reduce unnecessary network exposure
- Disable unnecessary services before considering package removal
- Inspect package dependencies before removal
- Preview package transactions before applying them
- Avoid uncontrolled bulk package removal
- Preserve required administrative, diagnostic, and recovery capabilities
- Validate system state after significant changes
- Keep minimization decisions explicit, reproducible, and auditable
- Evaluate changes against the intended infrastructure architecture

## Service Review

Enabled and running services should be reviewed as part of system baseline validation.

A service should remain enabled only when it has a defined purpose within the operating system baseline or the intended infrastructure architecture.

Service review should include:

- Enabled service units
- Active service units
- Failed units
- Socket-activated services
- Unexpected network listeners
- Services introduced by installed packages
- Services no longer required after infrastructure changes

Unnecessary services should normally be disabled and stopped before their packages are considered for removal.

This staged approach allows the effect of service removal to be validated before software packages are modified.

## Network Exposure Review

Listening network sockets should be reviewed before and after system minimization changes.

The review should identify:

- Listening TCP sockets
- Listening UDP sockets
- Associated processes
- Address families
- Bound network interfaces
- Unexpected externally reachable services

A package or service that introduces unnecessary network exposure should be investigated and removed or restricted when it has no defined operational purpose.

Network exposure should also be evaluated against host-level and provider-level firewall policies.

## Package Review

Installed packages should not be removed solely on the basis of package names or apparent function.

Before removing software, the review should determine:

- Why the package is installed
- Whether it was explicitly installed or introduced as a dependency
- Which installed packages require it
- Which services or system functions depend on it
- Whether removal affects administration, recovery, networking, logging, security, or future infrastructure components

Package manager dependency information and transaction previews should be reviewed before package removal.

## Transaction Preview

Package removal transactions should be previewed before execution whenever the expected dependency impact is not already fully understood.

The preview should be reviewed for:

- Intended package removals
- Dependent package removals
- Automatically selected unused dependencies
- Unexpected changes to core system components
- Changes that may affect administration or recovery

A transaction should not proceed when its effects are broader than intended or insufficiently understood.

## Automatic Dependency Removal

Automatic dependency removal should be treated conservatively.

A package manager may classify packages as unused based on dependency metadata without understanding the operational role of those packages within the infrastructure architecture.

Bulk automatic removal should therefore not be used as a substitute for explicit package review.

When automatic dependency removal identifies potentially unnecessary packages, the resulting list should be treated as review input rather than an instruction to remove all listed software.

Packages should be evaluated individually or in clearly understood functional groups.

## Removal Strategy

System minimization should use an incremental removal strategy.

The preferred process is:

1. Identify an unnecessary service or software component.
2. Determine its operational purpose and dependency relationships.
3. Disable and stop associated services when appropriate.
4. Validate system operation and network exposure.
5. Preview the package removal transaction.
6. Review all packages selected for removal.
7. Apply the removal only when the transaction is understood.
8. Validate the resulting system state.

Unrelated software components should not be grouped into a single removal transaction solely for convenience.

## Residual State

Package removal may leave residual runtime state or system resources from the current boot session.

Examples may include:

- Mounted virtual filesystems
- Generated service state
- Runtime directories
- Cached configuration
- Failed service state
- Loaded system manager metadata

Residual state should be investigated before manual removal.

The system should determine whether the resource is still required, which process or subsystem owns it, and whether cleanup can be performed safely.

System manager state should be reloaded when installed unit definitions change and validation indicates that a reload is appropriate.

## Administrative Tools

System minimization should preserve the tools required for reliable deployment, administration, troubleshooting, and validation.

Administrative tools may include:

- Version control utilities
- Text editors
- Network diagnostic tools
- Packet capture tools
- Process and socket inspection utilities
- Package management tools
- Security and policy validation tools

The presence of an administrative tool should be justified by operational use rather than removed solely to minimize package count.

## Recovery Capabilities

Minimization should not unnecessarily remove recovery capabilities required by the deployment environment.

Recovery requirements should be evaluated according to:

- VPS provider capabilities
- Console access
- Boot and filesystem recovery mechanisms
- Remote administrative access
- System logging
- Network troubleshooting requirements
- Infrastructure role

A recovery component that is unusable in the current deployment environment should be evaluated differently from a functioning recovery mechanism with a defined operational purpose.

## Validation

System state should be validated after significant minimization changes.

Validation should include, where applicable:

- Package manager consistency
- Failed system units
- Enabled and active services
- Listening network sockets
- Host firewall state
- Remote administrative access
- Security policy state
- Logging functionality
- Time synchronization
- Required administrative tools
- Residual mounts or runtime resources

Validation results should be reviewed before additional minimization changes are introduced.

## Security Considerations

System minimization is a security control only when performed deliberately.

Removing unnecessary software and services can reduce attack surface, but indiscriminate package removal can weaken administration, observability, recovery, or security controls.

The objective is not to achieve the smallest possible package count.

The objective is to maintain the smallest practical and well-understood system state that supports reliable operation, administration, security, and the intended infrastructure architecture.

Deployment-specific addresses, credentials, private keys, and other sensitive information must not be committed to the repository.

## Change Management

System minimization changes should remain explicit, reviewable, and reproducible.

Significant decisions should be documented when they affect the operating system baseline, administrative workflow, security architecture, or recovery model.

Repository changes should be validated before commit.

System minimization documentation should evolve together with the broader `vpshub` architecture and should be reviewed when new infrastructure components introduce additional operating system requirements.
