# DATED FORMATIVE HEURISTIC USABILITY INSPECTION RECORD
**Candidate & Inspector:** James Olushola Atolagbe (James Atolagbe)  
**Degree:** Master of Research (MRes) in Cybersecurity  
**Institution:** School of Computing, Engineering & the Built Environment, Edinburgh Napier University  
**Date of Inspection:** October 25, 2026  
**Prototype Inspected:** Xenoverify Version 1.0 (Git Commit: `2f8ac68a2234c5467ba59ca470158c6bf05453b9`)  
**Applicable Framework:** Edinburgh Napier University Research Degrees Framework, Section 1.3 (Research Ethics and Integrity)  

---

## 1. Institutional Ethics Determination & Reviewer Position
*   **Evaluation Mode:** Formative Developer Self-Inspection (Technical walkthrough conducted by the author).
*   **Institutional Position:** Under Edinburgh Napier University Research Degrees Framework Section 1.3, an institutional determination confirmed that developer self-inspection of a technical prototype without the recruitment of external human participants or collection of external personal data does not require formal Research Ethics Committee (REC) approval.
*   **Methodological Purpose:** To identify interface friction, interaction bottlenecks, and user feedback consequences resulting from post-quantum cryptographic latency (~1.6-second ML-DSA-65 signing) and selective disclosure controls prior to any future participant trials.

---

## 2. Severity Rating Criteria (Nielsen, 1994)
The inspection applied the standard Nielsen Norman Group severity scale:
*   **0 = Not a usability problem at all:** No interaction or visual friction observed.
*   **1 = Cosmetic problem only:** Need not be fixed unless extra time is available on project.
*   **2 = Minor usability problem:** Fixing this should be given low priority.
*   **3 = Major usability problem:** Important to fix, so should be given high priority.
*   **4 = Usability catastrophe:** Imperative to fix before product can be released.

---

## 3. Heuristic Inspection Record across Nielsen's Ten Usability Heuristics

### H1: Visibility of System Status
*   **Evaluated Feature:** Authority credential issuance and cryptographic signing.
*   **Observed Interaction Issue:** Pure-Python bytecode signing with ML-DSA-65 requires a mean of 1,659.51 ms. Without visual feedback, the user could perceive an interface freeze or crash.
*   **Assigned Severity:** **1 (Cosmetic)**
*   **Design Remedy Implemented:** Added an asynchronous animated spinner alongside real-time dynamic text banners (e.g., *"Signing Authenticated Credential Envelope with NIST FIPS 204 ML-DSA-65..."*), providing clear operational visibility.

### H2: Match between System and the Real World
*   **Evaluated Feature:** Identity lifecycle workflow organization.
*   **Observed Interaction Issue:** Lattice mathematics and polynomial sampling are unintuitive for non-specialist operators.
*   **Assigned Severity:** **0 (None)**
*   **Design Remedy Implemented:** Decoupled the interface into three clear role-oriented tabs mirroring physical identity verification: **Issuer** (Genesis & Issuance), **Holder** (Wallet & Selective Toggling), and **Verifier** (Verification & Telemetry).

### H3: User Control and Freedom
*   **Evaluated Feature:** Attribute selective disclosure selection in the wallet.
*   **Observed Interaction Issue:** Risk of unintentional disclosure or rigid all-or-nothing presentation.
*   **Assigned Severity:** **0 (None)**
*   **Design Remedy Implemented:** Implemented granular single-click checkbox toggles for each attribute, allowing the holder to freely reveal or conceal claims before clicking 'Compile Presentation'.

### H4: Consistency and Standards
*   **Evaluated Feature:** Cryptographic inspectors and claim formatting.
*   **Observed Interaction Issue:** Raw 3,309-byte signatures and 1,952-byte keys overflow standard input fields. Attribute names (`fullName`, `nin`, `dob`) are custom and do not yet fully implement W3C JSON-LD context URLs.
*   **Assigned Severity:** **0 (None)**
*   **Design Remedy Implemented:** Formatted cryptographic telemetry inside scrollable, monospace text areas. Documented claim schemas clearly in OpenAPI/Swagger UI.

### H5: Error Prevention
*   **Evaluated Feature:** Form submission during asynchronous cryptographic calls.
*   **Observed Interaction Issue:** Rapid double-clicking by an operator during the 1.6-second signing delay could spawn concurrent HTTP requests, risking race conditions in database transactions.
*   **Assigned Severity:** **2 (Minor)**
*   **Design Remedy Implemented:** Implemented button disabling during in-flight asynchronous requests. Buttons automatically enter a disabled state upon click and re-enable upon API response resolution.

### H6: Recognition rather than Recall
*   **Evaluated Feature:** Verification outcome presentation.
*   **Observed Interaction Issue:** Interpreting whether a multi-stage verification succeeded required parsing raw JSON telemetry.
*   **Assigned Severity:** **0 (None)**
*   **Design Remedy Implemented:** High-contrast emerald badge for valid verifications (*"CRYPTOGRAPHICALLY VALID & VERIFIED"*); crimson alert banner for invalid presentations.

### H7: Flexibility and Efficiency of Use
*   **Evaluated Feature:** Security demonstration and adversarial testing.
*   **Observed Interaction Issue:** Manually tampering with JSON payloads to test verification stages is error-prone and slow.
*   **Assigned Severity:** **0 (None)**
*   **Design Remedy Implemented:** Added a one-click *"Simulate Tamper Attack"* button that programmatically flips a byte in revealed claims, demonstrating immediate Stage 3 rejection.

### H8: Aesthetic and Minimalist Design
*   **Evaluated Feature:** Overall interface layout and information density.
*   **Observed Interaction Issue:** Dense cryptographic parameters can visually overwhelm user claim data.
*   **Assigned Severity:** **0 (None)**
*   **Design Remedy Implemented:** Clean Slate/Zinc dark-mode palette with glassmorphism elevation, separating essential identity claims from advanced technical telemetry.

### H9: Help Users Recognize, Diagnose, and Recover from Errors
*   **Evaluated Feature:** Relying party verification failure alerts.
*   **Observed Interaction Issue:** Raw cryptographic exception codes (e.g., `MERKLE_PROOF_INCLUSION failed`) provide diagnostic precision for developers, but fail to explain the cause or remedy to end users.
*   **Assigned Severity:** **2 (Minor)**
*   **Design Remedy Implemented:** Added contextual explanatory alerts alongside technical codes (e.g., *"Attribute value was modified after issuance: SHA-256 leaf hash divergence detected"*).

### H10: Help and Documentation
*   **Evaluated Feature:** In-app architecture guidance and API documentation.
*   **Observed Interaction Issue:** Operators may not understand the boundary between implemented cryptographic components and simulated ledger services.
*   **Assigned Severity:** **0 (None)**
*   **Design Remedy Implemented:** Integrated a dedicated ScopeTab explaining the prototype boundary matrix (Table 4.1), supplemented by dynamic Swagger UI documentation at `/swagger/`.

---

## 4. Inspection Synthesis & Version Evolution
*   **Identified Usability Issues:** 3 issues identified (H1: Severity 1 Cosmetic; H5: Severity 2 Minor; H9: Severity 2 Minor).
*   **Remediation Status:** All three issues were mitigated through UI enhancements in React (commit `2f8ac68a`).
*   **Evaluation Limitation:** While these remedies improve interaction clarity, they represent developer-centric heuristics. Comprehensive multi-participant empirical testing remains a documented recommendation for future work in Chapter 5.
