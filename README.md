# STX-ProofGuard Smart Contract

## Overview

**STX-ProofGuard** is a Clarity-based smart contract deployed on the Stacks blockchain to register and monitor web-based entities for potential threats. It enables a decentralized registry for verified sites, tracks sentinel performance, and facilitates secure reporting of malicious actors through strong validation, audit logging, and collateralized accountability.

This system ensures only verified entities are registered and continuously monitored by independent sentinels who earn credibility through accurate submissions.

---

## 📚 Features

* **Web Identifier Registration**
  Site owners can register secure sites with unique identifiers and security endorsements.

* **Collateral Requirement**
  Sites must lock STX tokens based on the current `protection_intensity` level.

* **Validation Logic**
  Strict input validation for web identifiers, security endorsements, threat magnitude, and proof documentation.

* **Malicious Site Reporting**
  Allows verified entities to flag malicious websites with proof and magnitude of the threat.

* **Sentinel Monitoring System**
  Sentinels are tracked for performance metrics including submission count and credibility scores.

* **Inspection Records**
  Maintains inspection history and compliance rating of registered websites.

* **Security & Access Control**
  Only the system controller can register new websites, with strict role-based access using `tx-sender`.

---

## 🧱 Contract Structure

### Constants

#### Error Codes

```clojure
ACCESS_FORBIDDEN             ;; u100
DUPLICATE_ENTRY_ERROR        ;; u101
ENTRY_MISSING_ERROR          ;; u102
...
INVALID_CONTROLLER_ADDRESS   ;; u405
```

#### System Parameters

```clojure
INACTIVITY_WINDOW              ;; 24 hours (u86400)
BASE_COLLATERAL_REQUIREMENT    ;; u1000000 (microSTX)
EVIDENCE_STRING_LIMIT          ;; u500
```

### Data Variables

* `system_controller` – The address with elevated privileges (e.g., can register sites)
* `entry_validation_cost` – Cost for validating each entry
* `protection_intensity` – Multiplier affecting collateral requirement

---

## 🗂️ Data Maps

### `registered_sites`

Stores verified websites with details like authentication level, threat metrics, and locked collateral.

### `malicious_site_registry`

Registry of sites flagged as malicious, including threat magnitude and proof.

### `sentinel_registry` & `sentinel_performance_log`

Tracks sentinels’ performance, activity, and credibility.

### `site_inspection_records`

Inspection frequency and safety ratings per registered site.

---

## 🔍 Read-Only Functions

* `check_threat_status`
  Check if a website is flagged as malicious.

* `fetch_sentinel_rating`
  Return credibility score for a sentinel.

* `fetch_site_status`
  Get full information about a registered site.

---

## 📝 Core Public Function

### `register_secure_site`

```clojure
(register_secure_site (web_identifier string-ascii-255) (security_endorsement string-ascii-50))
```

**Requirements:**

* Only `system_controller` can call
* Valid web ID and endorsement
* Collateral transferred to contract
* No duplicate entries allowed

**On Success:**

* Registers site with metadata
* Stores endorsement
* Logs epoch time and collateral

---

## ✅ Validation Rules

Each input is thoroughly validated to prevent misuse:

* **Web Identifiers** must:

  * Be 3–255 characters
  * Not include `.`, `/`, or spaces

* **Security Endorsements & Proof Docs**:

  * Have length limits
  * Disallow `<` and `>`

* **Threat Magnitudes**:

  * Between 1 and 100

* **Protection Levels**:

  * Between 1 and 10

---
