# REPRODUCIBILITY RECORD & EXPERIMENTAL SPECIFICATION
**Institution:** Edinburgh Napier University  
**School:** School of Computing, Engineering & the Built Environment  
**Programme:** Master of Research (MRes) in Cybersecurity  
**Candidate:** James Olushola Atolagbe (James Atolagbe)  
**Dissertation:** *Blockchain-Based Identity Management Systems: Enhancing Privacy, Security and Usability in Digital Identity Verification with Post-Quantum Consideration*  
**Repository Tested:** Xenoverify  
**Tested Code Commit:** `2f8ac68a2234c5467ba59ca470158c6bf05453b9`  
**Evaluation Dates:** October 25, 2026 (Feasibility & Comparative Benchmarks); November 9, 2026 (Verification & Security Test Battery)  

---

## 1. Hardware, Host Platform & Operating System Details
*   **Processor:** Intel Core i7-1165G7 @ 2.80 GHz (4 Physical Cores, 8 Logical Threads, Max Turbo 4.70 GHz, 12 MB Intel Smart Cache)
*   **System Memory (RAM):** 16.0 GB DDR4-3200 MHz Dual-Channel
*   **Operating System:** Microsoft Windows 11 Professional 64-bit (Version 23H2, OS Build 22631.4317)
*   **Host Environment Control:** Benchmarks executed on a dedicated host platform with non-essential user applications, indexing, and background services paused to minimize CPU contention.

---

## 2. Software Runtime, Interpreters & Dependency Lock Specification
*   **Python Runtime:** Python 3.11.7 (64-bit, Anaconda distribution / MSC v.1916 64 bit)
*   **Web Framework:** Django 5.0.0
*   **REST API Framework:** Django REST Framework (DRF) 3.14.0
*   **API Schema Documentation:** drf-yasg 1.21.7 (OpenAPI / Swagger UI)
*   **Classical Cryptography:** `cryptography` v42.0.0 (OpenSSL 3.2.0 backend, OpenSSL C-assembly acceleration)
*   **Post-Quantum Signature Primitive:** `dilithium-py` v1.4.0 (Git Commit: `5275e7a`, Module: `dilithium_py.ml_dsa.ML_DSA_65`, Authored by Giacomo Pope)
*   **Frontend Client SPA:** React 19.3.0, Vite 8.3.0, TailwindCSS 3.4.1
*   **Database Management:** SQLite 3.45.1 (Simulated ledger models for DIDRegistry, IssuedCredential, VerifierNonce)

---

## 3. Warm-up Configuration, Sampling & Measurement Boundaries
*   **Timing Instrumentation:** Monotonic high-precision hardware timer via Python `time.perf_counter()` (nanosecond resolution, immune to system clock adjustments).
*   **Warm-up Protocol:** A formal warmup sequence of **10 unrecorded consecutive trials** was executed prior to logging to eliminate cold-start bias arising from Python bytecode compilation, OS file caching, and dynamic module loading.
*   **Feasibility Sample Size:** $N = 100$ consecutive automated trials executing full end-to-end issuance, proof extraction, and 5-stage verification using NIST FIPS 204 ML-DSA-65.
*   **Comparative Sample Size:** $N = 50$ consecutive trials per scheme comparing pure-Python ML-DSA-65 against OpenSSL C-assembly ECDSA (secp256k1).
*   **Memory Profiling Instrumentation:** Python `tracemalloc` module capturing peak Python heap memory allocations within the cryptographic execution boundaries (`tracemalloc.start()`, `tracemalloc.get_traced_memory()`, `tracemalloc.stop()`).

---

## 4. Test Persona & Synthetic Credential Fixture
To eliminate personal data exposure in compliance with Edinburgh Napier University Research Ethics policies, all evaluations used the fabricated synthetic identity fixture:
*   **Subject DID:** `did:xeno:subject:001`
*   **Issuing Authority DID:** `did:xeno:issuer:nimc-01`
*   **Credential Identifier:** `cred-mldsa-001`
*   **Schema Identifier:** `schema:xeno:national-id:v1`
*   **Attributes:**
    1. `fullName`: "Alex Carter"
    2. `nin`: "29837492812" (Synthetic National Identity Number)
    3. `date_of_birth`: "1992-06-14"
    4. `nationality`: "Nigerian"
    5. `clearance_level`: "Tier-3"
    6. `residence`: "Victoria Island, Lagos"
*   **Revealed Attributes in Presentation:** `fullName` and `nationality` (2 of 6 revealed; 4 hidden).

---

## 5. Exact Execution Commands
1. **To run the full 13-test cryptographic and security suite:**
   ```bash
   python manage.py test core_identity -v 2
   ```
2. **To reproduce the 100-trial feasibility milestone:**
   ```bash
   python scripts/run_feasibility_100.py
   ```
3. **To reproduce the 50-trial comparative benchmark (ML-DSA-65 vs ECDSA):**
   ```bash
   python scripts/run_comparative_benchmark.py
   ```
4. **To reproduce all statistical tables, percentiles, and confidence intervals:**
   ```bash
   python "docs/chapter 4 full work/reproduce_tables_and_statistics.py"
   ```
