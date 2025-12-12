# SSL Creation Script Guide

This guide explains how to use the `SSL_Creation.sh` script to set up SSL for your Kubernetes cluster using cert-manager.

## Overview

The script performs the following:
- Validates and sources the configuration from `config.env`
- Installs or re-installs `cert-manager`
- Disables and re-enables cert-manager webhooks to ensure compatibility
- Applies the appropriate SSL configuration based on the `INSTALL_TYPE` (local or cloud)
- Supports both Let's Encrypt and ZeroSSL as providers

## Prerequisites

1. Ensure `config.env` exists at the project root.
2. Kubernetes cluster is running and `kubectl` is configured.
3. Internet access to pull cert-manager manifests from GitHub.
4. You have run the `SSL_Details.sh` script first to generate the configuration.

## Configuration File (`config.env`)

Location: `config.env` in the repository root.

Required keys (set by `scripts/SSL_Details.sh`):
- `INSTALL_TYPE`: either `local` or `cloud`
- `SSL_PROVIDER`: `letsencrypt` or `zerossl` (only for cloud)
- `EMAIL`: contact email for ACME registration
- `ZEROSSL_EAB_HMAC_KEY` and `ZEROSSL_ACCESS_KEY_ID` (only if using ZeroSSL)
- `DOMAIN`: automatically detected by `Finding_Hostname.sh`

## Running the Script

```bash
chmod +x SSL_Creation.sh
./SSL_Creation.sh
```

## What the Script Does

1. **Environment setup**: Copies `config.env` to `.platform/.config` and sources it for use.
2. **Validation**: Checks that required variables (`INSTALL_TYPE`, `SSL_PROVIDER` for cloud) are set.
3. **cert-manager cleanup**: Deletes existing cert-manager resources including:
   - All deployments, services, pods, and secrets in cert-manager namespace
   - The cert-manager namespace itself
   - CRDs and webhook configurations
   - Waits 15 seconds for complete removal
4. **cert-manager installation**: 
   - Installs cert-manager CRDs from GitHub (v1.12.0)
   - Applies base resources from `deployment/kubeflow/manifests/common/cert-manager/cert-manager/base`
   - Temporarily disables webhook validations for initial setup
   - Waits for cert-manager pods to be ready
5. **SSL Configuration (Cloud Only)**:
   - Processes individual YAML files from the provider overlay directory
   - Applies ClusterIssuer and Certificate resources
   - Re-enables webhooks
   - Applies the complete overlay with webhooks enabled
   - Displays status of ClusterIssuers and Certificates
6. **Local installations**: Skips SSL provider configuration entirely

## Troubleshooting

- Missing `config.env`: Ensure you ran the `SSL_Details.sh` script.
- Overlay not found: Confirm overlay directory exists under `cert-manager/overlay/{provider}`.
- Webhook errors: Script disables/enables webhooks; re-run if needed.

## Notes

- This script is designed for modular SSL setup as part of the platform deployment pipeline.