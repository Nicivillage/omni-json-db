# Security Policy for omni-json-db

## Supported Versions

Currently, only the latest stable release is actively supported for security updates. 

| Version                     | Supported          |
| ----------------------------| ------------------ |
| Latest `omni-json-db` release | :white_check_mark: |
| All prior versions          | :x:                |

## Reporting a Vulnerability

**Please DO NOT report security vulnerabilities through public GitHub issues, discussions, or pull requests.**

If you believe you have found a security vulnerability in `omni-json-db`, please report it confidentially using **GitHub's Private Vulnerability Reporting** feature on this repository.

Please include the following in your report:
- A clear description of the vulnerability and its potential impact.
- A realistic attack scenario demonstrating how untrusted external input leads to the security impact.
- Exact steps to reproduce (including a minimal Python script if possible).
- The version(s) of `omni-json-db` and Python affected.

**SLA & Disclosure:** The maintainers aim to acknowledge your report within 72 hours and will work with you on a fix and a coordinated disclosure on a mutually agreed timeline.

## Scope & Threat Model

`omni-json-db` is primarily designed as an **embedded, server-less database** for trusted environments. While it includes a network server mode (`run_files_server`), the overarching threat model assumes that the developer is responsibly sanitizing external inputs before routing them to the database layer.

For a report to be considered a valid `omni-json-db` vulnerability, it must demonstrate:
1. **A realistic attack chain** where untrusted external data bypasses expected boundaries and causes unintended security impact *through* `omni-json-db`'s core internal logic.
2. **`omni-json-db` as the root cause**, not merely a component downstream of an existing application-level vulnerability.

### 🚫 Explicitly Out of Scope (Please Read Carefully)

Given the Pythonic and dynamic nature of `omni-json-db`, the following scenarios are **strictly out of scope** and will not be accepted as valid vulnerabilities:

- **Insecure Deserialization via `Pickle` or `Marshal`:** `omni-json-db` allows configurations using `Pickle` (e.g., `J+P`, `S+P`) and `Marshal` (e.g., `J+M`, `S+M`) for serialization. These formats are inherently unsafe when used with untrusted data and can lead to Arbitrary Code Execution (ACE/RCE). **If your application stores untrusted external data, you MUST use safe formats like JSON or MsgPack (`J+J`, `J+S`, `S+S`).** Exploits relying on Pickle/Marshal deserialization are application-level architecture flaws, not database vulnerabilities.
- **Malicious Callables & Lambdas:** `omni-json-db` is deeply Pythonic and accepts callables (lambdas, functions) for queries and data manipulation by design. If an attacker can inject arbitrary Python callables into your application's database queries, you already suffer from arbitrary code execution.
- **Unauthenticated Network Server Exposure:** The `run_files_server` module is designed for fast, internal microservice communication within trusted boundaries (e.g., localhost, VPCs). Exposing this server directly to the public internet without a reverse proxy, authentication, or TLS wrapper is a misconfiguration by the deployer.
- **Local Filesystem Compromise:** `omni-json-db` reads and writes to local `.jdb` files specified by the developer. If an attacker already has sufficient OS-level privileges to modify these database files or control file paths (Path Traversal at the OS level), the system is already fully compromised.
- **Resource Exhaustion (Denial of Service):** Standard DoS attacks achieved by flooding the database with massive amounts of data or opening infinite connections to the network server are out of scope. *Exception: DoS issues caused by algorithmic complexity within `omni-json-db` triggered by exceptionally small, valid payloads MAY be considered.*

## Safe Harbor & Legal Disclaimer

**Good Faith Research:** We consider activities conducted consistent with this policy to constitute "good faith" research. We will not initiate or support legal action against researchers who discover and report vulnerabilities in accordance with this policy.

**Disclaimer:** `omni-json-db` is provided "AS IS", without warranty of any kind, express or implied. The maintainers shall not be liable for any claim, damages, or other liability arising from the use of this software.
