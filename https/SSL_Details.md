# SSL Configuration Guide

This guide walks you through using the `SSL_Details.sh` script to configure SSL for your local or cloud-based deployment. The script supports both **ZeroSSL** and **Let's Encrypt** as certificate authorities and sets up the necessary configuration files.

**Important**: This script is the first step in SSL setup. It collects configuration details and writes them to:
- `config.env` (main configuration file)
- `deployment/kubeflow/manifests/common/cert-manager/cert-manager/overlay/<provider>/config.env`
- `deployment/mlflow/base/config.env`
- `deployment/kubeflow/manifests/apps/pipeline/upstream/base/pipeline/config.env`
- `deployment/monitoring/grafana/config.env`
- `deployment/monitoring/prometheus/config.env`

After configuration, it automatically runs `Finding_Hostname.sh` to detect your domain.

## Script Location

Ensure you're running the script from the project root. Typical layout:

```text
your-project/
├── scripts/
│   └── SSL_Details.sh
├── config.env
└── deployment/
```

## Prerequisites

- Bash shell (Linux/macOS)
- Valid email address
- (For ZeroSSL) EAB credentials (HMAC Key + Access Key ID)

## Running the Script

```bash
bash scripts/SSL_Details.sh
```

## Steps Performed

1. Prompt for installation type (`local` or `cloud`).
2. Prompt to choose SSL provider (ZeroSSL or Let's Encrypt).
3. Prompt for email address.
4. Write configuration to `config.env` and related deployment config files.
5. Invoke `Finding_Hostname.sh` to detect the hostname (if present).

## Errors and Troubleshooting

- If an invalid email is entered, the script will request it again.
- If `Finding_Hostname.sh` is missing, the script will warn and exit.

## Notes

- Re-run the script to update provider or email; it will overwrite the relevant `config.env` entries.
