# Sisilic OS — Public Showcase & Installer Release

**Enterprise Supply Chain & BOM Dependency Mapping Engine**

> ⚠️ **This is a public showcase / release distribution repository. NO source code is included here.**
>
> This repository contains documentation, sample BOM test templates, architecture specifications,
> and the official standalone installer. The full proprietary source code is maintained in a
> private repository.

![UI](https://img.shields.io/badge/UI-PyQt6-41CD52)
![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB)
![Tests](https://img.shields.io/badge/Tests-213%20Passing-brightgreen)
![Database](https://img.shields.io/badge/Database-SQLite%20%2F%20PostgreSQL-336791)
![Security](https://img.shields.io/badge/Security-HWID%20%2B%20PBKDF2%2FTOTP-6f42c1)
![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D4)

---

## Overview

Sisilic OS is a native, offline-first desktop application for procurement, supply-chain risk,
and hardware engineering teams. It answers one question well:

> *Which manufacturers and facilities does this BOM actually depend on, what changed,
> and what should a human do about it?*

The application combines automated BOM ingestion (CSV/XLSX), multi-tier MPN dependency resolution,
facility and smelter coverage analysis, and a COV-01/COV-02 resolution engine. Every displayed value
carries its evidence, source, observation time, and an explicit `UNKNOWN` state when the data does
not exist.

## Core Capabilities (V1)

| Capability | Description |
|---|---|
| **Automated BOM Ingestion** | CSV/XLSX upload with dynamic, case-insensitive column mapping. Every line reports `PARSED`, `PARTIAL`, or `UNREADABLE`; no line is silently discarded. |
| **Multi-Tier Dependency Resolution** | Customer part → MPN → manufacturer → facility/smelter, with explainable confidence and explicit `UNKNOWN` segments. |
| **COV-01 / COV-02 Coverage** | Authoritative coverage gateway reporting resolved segments and reachable sources with as-of timestamps. Gaps are reported as gaps, never fabricated. |
| **PyQt6 Desktop UI** | Responsive, offline-first desktop client with four core V1 surfaces (BOM, Dashboard, Compliance, Work Queue). |
| **Multi-Tenant Data Scoping** | Every tenant-scoped record carries `tenant_id`; PostgreSQL sessions set `app.tenant_id` for row-level security. |
| **Four-Role RBAC + TOTP MFA** | `ADMIN`, `REVIEWER`, `ANALYST`, `VIEWER` roles with PBKDF2-SHA256 password storage and RFC-6238 TOTP enrollment via QR code. |
| **Hardware-Locked Offline Licensing** | HMAC-SHA256-signed `.lic` files bound to a 32-character HWID (CPU, motherboard, network identifiers). |

## Installation

### Download the Installer

The official standalone installer is available as a GitHub Release asset:

**📦 [SisilicOS_Setup_v1.0.0.exe](https://github.com/carnoba/Sisilic-OS-Showcase/releases/download/v1.0.0-MVP/SisilicOS_Setup_v1.0.0.exe)**

Release: [v1.0.0-MVP — Sisilic OS v1.0.0 MVP Standalone Installer](https://github.com/carnoba/Sisilic-OS-Showcase/releases/tag/v1.0.0-MVP)

### System Requirements

- Windows 10 / 11 x64
- Python 3.10+ (bundled with installer via PyInstaller)
- PostgreSQL 15+ (optional; SQLite fallback available for development)

## Sample BOM Templates

The `sisilic_test_data/` directory contains standard BOM templates for testing and demonstration:

| File | Description |
|---|---|
| `sisilic_test_data/STANDARD_BOM_TEMPLATE.csv` | Standard BOM import template (CSV) |
| `sisilic_test_data/STANDARD_BOM_TEMPLATE.xlsx` | Standard BOM import template (Excel) |
| `sisilic_test_data/open_hardware_boms/` | Sample open-hardware BOMs for evaluation |

See the BOM Ingestion Guide in the application UI under **BOM & Dependencies → Import BOM**.

## Architecture

Sisilic OS is built around these non-negotiable data rules:

1. **No value without evidence.** Every resolved value carries its source, observation time, and confidence basis.
2. **Unknown is a first-class state.** Missing data is rendered as `UNKNOWN`, never as a blank, a guess, or a fabricated finding.
3. **No invented scores.** The product reports dependencies, coverage ratios, and evidence — not numeric risk scores or severity ratings.
4. **Tenant isolation everywhere.** Every customer-confidential record is scoped by `tenant_id`.

## Tech Stack

| Layer | Technology |
|---|---|
| Desktop UI | PyQt6, pyqtgraph |
| Data access | SQLAlchemy 2.x, psycopg2-binary |
| Database | PostgreSQL 15+ / SQLite fallback |
| Ingestion | pandas, openpyxl |
| Contracts | jsonschema |
| Graph analysis | networkx, numpy |
| MFA provisioning | segno |
| Packaging | PyInstaller, Inno Setup 6 |

## Security & Responsible Disclosure

- Passwords are stored as PBKDF2-SHA256 tokens (260,000 iterations, per-user salt).
- TOTP MFA is optional per user (RFC-6238).
- Licenses are HMAC-SHA256 signed and bound to machine HWID.
- `.env`, `*.db`, `*.lic`, and build outputs are never committed.

**Production hardening checklist:**
- [ ] Set a strong, private `SISILIC_LICENSE_SECRET`
- [ ] Set `ALLOW_SQLITE_FALLBACK=false` and point `DATABASE_URL` at PostgreSQL
- [ ] Change the default admin password immediately
- [ ] Enable TOTP MFA for administrative accounts

To report a security issue, contact the maintainer privately rather than opening a public issue.

## License

Sisilic OS is proprietary software. All rights reserved.

This repository is published as a source-available portfolio and competition showcase.
No license is granted for commercial redistribution, sublicensing, or resale without
prior written permission from the maintainer.

See `LICENSE` for full terms.

---

**Sisilic OS** — *Know what you depend on. Show the evidence. State what is unknown.*