# Automatic Domain Detection and Configuration Script Guide

The `Finding_Hostname.sh` script automates the detection of your public IP address, performs a reverse DNS lookup to determine the domain name, and updates all relevant `config.env` files across the project.

**Important**: This script is automatically called by `SSL_Details.sh` after SSL configuration. You typically don't need to run it manually.

## Prerequisites

- Internet access.
- Ability to install packages (`sudo` or appropriate privileges).
- `apt-get` or `yum` available for package installation (script will try to detect and use the appropriate package manager).

## What the Script Does

1. Ensures required tools (`curl`, `dig`) are installed.
2. Detects the public IP address (e.g., via `ifconfig.me` or similar service).
3. Performs a reverse DNS lookup to obtain a candidate hostname for the IP.
4. Reads `SSL_PROVIDER` (and other values) from the project's `config.env`.
5. Updates `DOMAIN=<resolved domain>` (or the IP fallback) in a set of config files used by the deployment.

Typical files updated:

- `deployment/kubeflow/manifests/common/cert-manager/cert-manager/overlay/<provider>/config.env` (provider-specific overlay)
- `deployment/mlflow/base/config.env`
- `deployment/kubeflow/manifests/apps/pipeline/upstream/base/pipeline/config.env`
- `deployment/monitoring/grafana/config.env`
- `deployment/monitoring/prometheus/config.env`

## Example Output

```bash
Detecting public IP address...
Detected IP address: 203.0.113.10
Looking up hostname from IP address...
Using domain: example.yourdomain.com
Updated DOMAIN in config.env files.
```

If reverse DNS lookup fails, the script falls back to using the IP address as the domain.

## Troubleshooting

- Permission errors: run `chmod +x detect_and_configure_domain.sh` and re-run the script.
- Reverse lookup fails: not all IPs have reverse DNS records; the script will continue and use the IP.
- Missing config files: the script will create missing files/directories where appropriate, but verify paths are correct for your deployment layout.

## How to run

```bash
chmod +x detect_and_configure_domain.sh
./detect_and_configure_domain.sh
```

Run the script from the repository root so relative paths resolve correctly.

## Notes

- Only files relevant to your chosen `SSL_PROVIDER` will be updated.
- The script is idempotent: running it multiple times updates existing `DOMAIN=` entries or appends them when missing.