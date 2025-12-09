# Automatic Domain Detection and Configuration Script Guide

This script helps automate the detection of your public IP address, performs a reverse DNS lookup to determine the domain name, and updates several `config.env` files across your project with the detected domain.

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

- `deployment/cert-manager/overlay/<provider>/config.env` (provider-specific overlay)
- `deployment/mlflow/base/config.env`
- `deployment/kubeflow/.../pipeline/config.env`
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
# Automatic Domain Detection and Configuration Script Guide

This script helps automate the detection of your public IP address, performs a reverse DNS lookup to determine the domain name, and updates several `config.env` files across your project with this domain.

---

## Prerequisites

Before running this script, make sure:

- You have **internet access**.
- You can run commands with `sudo` (for package installation).
- Your system uses either `apt-get` (Debian/Ubuntu) or `yum` (RHEL/CentOS).

## What This Script Does

1. **Checks and Installs Required Tools**: Ensures that `curl` and `dig` are installed.
2. **Detects Public IP Address**: Uses `curl ifconfig.me` (or similar) to fetch your current public IP.
3. **Performs Reverse DNS Lookup**: Uses `dig` to resolve the domain name from your IP address.
4. **Extracts SSL Provider**: Reads the `SSL_PROVIDER` from your project's main `config.env` file.
5. **Updates DOMAIN Value**: Writes or updates `DOMAIN=<resolved domain>` in the relevant config files across the project.

Files updated may include:

- cert-manager overlay for the current `SSL_PROVIDER`
- `deployment/mlflow/base/config.env`
- `deployment/kubeflow/.../pipeline/config.env`
- `deployment/monitoring/grafana/config.env`
- `deployment/monitoring/prometheus/config.env`

## Example Output

```
Detecting public IP address...
Detected IP address: 203.0.113.10
Looking up hostname from IP address...
Using domain: example.yourdomain.com
Updated DOMAIN in config.env files.
```

If the reverse DNS fails, the script will fall back to using the IP as the domain.

## Troubleshooting

- **Permission Denied?** Use `chmod +x` to make the script executable.
- **Reverse Lookup Fails?** Your IP may not have a reverse DNS set up. The script will still proceed using the IP.
- **Config File Missing?** The script will create missing config files and parent directories as needed.

## How To Run

```
chmod +x detect_and_configure_domain.sh
./detect_and_configure_domain.sh
```

Make sure you're in the correct directory so the relative paths resolve correctly.

## Notes

- Only files relevant to your chosen `SSL_PROVIDER` will be updated.
- This script is safe to run multiple times — it will update existing `DOMAIN=` entries or append new ones.
# Automatic Domain Detection and Configuration Script Guide

This script helps automate the detection of your public IP address, performs a reverse DNS lookup to determine the domain name, and updates several `config.env` files across your project with this domain.

---

## 🛠 Prerequisites

Before running this script, make sure:

- You have **internet access**.
- You can run commands with `sudo` (for package installation).
- Your system uses either `apt-get` (Debian/Ubuntu) or `yum` (RHEL/CentOS).

---

## 📄 What This Script Does

1. **Checks and Installs Required Tools**: Ensures that `curl` and `dig` are installed.
2. **Detects Public IP Address**: Uses `curl ifconfig.me` to fetch your current public IP.
3. **Performs Reverse DNS Lookup**: Uses `dig` to resolve the domain name from your IP address.
4. **Extracts SSL Provider**: Reads the `SSL_PROVIDER` from your project's main `config.env` file.
5. **Updates DOMAIN Value**: Writes or updates `DOMAIN=<resolved domain>` in the following config files:

    - `cert-manager overlay` for the current `SSL_PROVIDER`
    - `mlflow/base/config.env`
    - `kubeflow pipeline/config.env`
    - `monitoring/grafana/config.env`
    - `monitoring/prometheus/config.env`

---

## 🧪 Example Output

```bash
Detecting public IP address...
Detected IP address: 203.0.113.10
Looking up hostname from IP address...
Using domain: example.yourdomain.com
Updated DOMAIN in config.env files.
```

If the reverse DNS fails, the script will fallback to using the IP as the domain.

---

## 🧯 Troubleshooting

- **Permission Denied?** Use `chmod +x` to make the script executable.
- **Reverse Lookup Fails?** Your IP may not have a reverse DNS set up. The script will still proceed using the IP.
- **Config File Missing?** The script will create missing config files and parent directories as needed.

---

## 🏁 How To Run

```bash
chmod +x detect_and_configure_domain.sh
./detect_and_configure_domain.sh
```

Make sure you're in the correct directory so the relative paths resolve correctly.

---

## 📌 Notes

- Only files relevant to your chosen `SSL_PROVIDER` will be updated.
- This script is safe to run multiple times—it will update existing `DOMAIN=` entries or append new ones.

