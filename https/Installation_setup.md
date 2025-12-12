# HTTPS Setup Guide for Cloud Deployment

This guide explains how to configure HTTPS/SSL certificates for the MLOps platform when deploying to a cloud environment using cert-manager with either Let's Encrypt or ZeroSSL.

## Overview

The platform includes automated SSL/TLS certificate management through cert-manager. The setup process is managed through dedicated scripts that configure certificates based on your deployment environment:

- **Local deployments**: No SSL configuration required (uses `INSTALL_TYPE=local`)
- **Cloud deployments**: Requires SSL configuration with either Let's Encrypt or ZeroSSL (`INSTALL_TYPE=cloud`)

The configuration uses Kustomize overlays to apply provider-specific settings and automatically manages certificate issuance and renewal.

## Cloud Deployment

### Prerequisites

- A Kubernetes cluster running in a cloud environment (or accessible with a public IP/domain).
- `kubectl` configured to access your cluster.
- A valid email address for ACME registration.
- A public domain name or IP address.
- (Optional) For ZeroSSL: EAB HMAC Key and Access Key ID from ZeroSSL dashboard.
- Internet access to download cert-manager manifests.

### Setup Instructions

#### Step 1: Configure SSL Settings

Run the SSL configuration script:

```bash
bash scripts/SSL_Details.sh
```

The script will prompt you for:

1. **Installation Type**: Choose `cloud` for cloud deployment
   ```
   Are you installing on a local machine or a cloud instance? [local/cloud]: cloud
   ```

2. **SSL Provider**: Choose between ZeroSSL or Let's Encrypt
   ```
   [1] ZeroSSL
   [2] Let's Encrypt
   Enter your choice [1-2] (default is [2]): 2
   ```

3. **Email Address**: Provide a valid email for ACME registration
   ```
   Please enter your email address: your@email.com
   ```

4. **ZeroSSL Credentials** (only if ZeroSSL is selected):
   - EAB HMAC Key
   - Access Key ID

The script will:
- Save configuration to `config.env`
- Update all relevant deployment config files
- Automatically run `Finding_Hostname.sh` to detect and configure your domain

#### Step 2: Apply SSL Configuration to Cluster

After configuration is complete, apply the SSL setup to your cluster:

```bash
bash scripts/SSL_Creation.sh
```

This script will:
- Clean up any existing cert-manager installation
- Install cert-manager v1.12.0
- Apply the ClusterIssuer and Certificate resources for your chosen provider
- Configure webhooks and wait for resources to be ready

#### Step 3: Verify Certificate Issuance

Check that certificates are being issued correctly:

```bash
# Check ClusterIssuer status
kubectl get clusterissuers --all-namespaces

# Check Certificate status
kubectl get certificates --all-namespaces

# Describe a specific certificate
kubectl -n <namespace> describe certificate <certificate-name>

# Check cert-manager logs
kubectl -n cert-manager logs -l app=cert-manager
```

## Local Installation

For local deployments, SSL configuration is not required as the platform runs on localhost.

1. **Run the SSL Details Script** and select `local`:

   ```bash
   bash scripts/SSL_Details.sh
   ```
   
   When prompted, enter `local`:
   ```
   Are you installing on a local machine or a cloud instance? [local/cloud]: local
   ```

   The script will:
   - Set `INSTALL_TYPE=local` in `config.env`
   - Exit without SSL configuration
   - Skip cert-manager setup

2. **Run the Main Setup Script**:

   ```bash
   ./setup.sh
   ```

   The platform will deploy without SSL/TLS certificates, using HTTP on localhost.

## Choosing Between Let's Encrypt and ZeroSSL

### Let's Encrypt
- **Recommended** for most deployments
- Simple configuration (only requires email)
- No additional account setup needed
- Widely trusted and battle-tested
- Free automated SSL certificates

### ZeroSSL
- Requires External Account Binding (EAB) credentials
- Need to create account at ZeroSSL and obtain:
  - EAB HMAC Key
  - Access Key ID
- Good alternative if you need CA diversity
- Also provides free SSL certificates

**Note**: Both providers are fully supported by the platform's scripts and will automatically renew certificates before expiration.

## Troubleshooting

- **Certificate Not Issued**: Verify the `ClusterIssuer` status and cert-manager logs.

```bash
kubectl -n cert-manager describe clusterissuer <issuer-name>
kubectl -n cert-manager logs -l app=cert-manager
```