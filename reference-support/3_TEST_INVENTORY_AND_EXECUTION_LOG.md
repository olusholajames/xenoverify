# TEST INVENTORY & NEGATIVE SECURITY EXECUTION LOG
**Candidate:** James Olushola Atolagbe (James Atolagbe)  
**Degree:** Master of Research (MRes) in Cybersecurity  
**Institution:** Edinburgh Napier University  
**Module:** `core_identity/tests.py`  
**Execution Command:** `python manage.py test core_identity -v 2`  
**Timestamp of Execution:** 2026-11-09T04:53:42Z  
**Result:** 13/13 Tests Passing (5.451 seconds)  

---

## 1. Test Inventory: Attack Vectors, Injections, Target Stages & Observed Behaviors

| Test Identifier & Method | Injected Mutation / Adversarial Anomaly | Target Pipeline Stage | Expected First Failing Assertion | Observed Empirical Result | Classification |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **test_01_nominal_pqc_selective_disclosure_verification** | Nominal valid ML-DSA-65 credential; valid selective presentation (2/6 attributes revealed). | Stage 0–4 (All Stages) | None (All stages pass; candidate root $R' == R$, signature $\sigma$ verifies against envelope $M$). | **PASSED (100% acceptance)** | Valid Control |
| **test_02_negative_tampered_disclosed_leaf_value (AV1)** | Altered 1 single ASCII byte in revealed claim plaintext (`fullName`: "Alex Carter" -> "Alex Barten"). | Stage 3 (Merkle Proof) | SHA-256 leaf divergence produces candidate root $R' \ne R$; `status: MERKLE_PROOF_INCLUSION_FAILED`. | **REJECTED (50/50 trials, 100%)** | Malformed Input |
| **test_03_negative_malformed_sibling_proof_path (AV2)** | Replaced 1 sibling hash in authentication path with random 256-bit hexadecimal string. | Stage 3 (Merkle Proof) | Branch reconciliation failure during tree climb yields $R' \ne R$; halts pipeline. | **REJECTED (50/50 trials, 100%)** | Malformed Input |
| **test_04_negative_corrupted_post_quantum_signature (AV3)** | Flipped 1 byte in the 3,309-byte ML-DSA-65 lattice signature string. | Stage 4 (PQC Signature) | Lattice polynomial vector verification fails on candidate envelope $M$; signature rejected. | **REJECTED (50/50 trials, 100%)** | Malformed Input |
| **test_05_negative_substituted_signed_root (AV4)** | Substituted authentic Merkle root from another valid credential into presentation envelope. | Stage 4 (PQC Signature) | Traversal derives presentation root $R'$, but envelope reconstruction $M'$ fails signature assertion. | **REJECTED (50/50 trials, 100%)** | Malformed Input |
| **test_06_negative_attribute_splicing_across_credentials (AV5)** | Mixed authentic attribute leaf from Credential B into presentation of Credential A. | Stage 3 (Merkle Proof) | Independent 128-bit CSPRNG salts and leaf positioning prevent composite branch reconciliation ($R' \ne R$). | **REJECTED (50/50 trials, 100%)** | Malformed Input |
| **test_07_negative_revoked_credential_circumvention (AV6)** | Presented valid credential marked `is_revoked = True` in simulated revocation database model. | Stage 1 (Revocation Check) | Query to revocation registry returns `is_revoked == True`; pipeline halts immediately. | **REJECTED (50/50 trials, 100%)** | Malformed Input |
| **test_08_presentation_unlinkability_entropy_audit (AV7)** | Evaluated Shannon entropy ($H$) and collision resistance across independently issued credentials. | Cross-Presentation Audit | Measured Shannon entropy $H \ge 7.99$ b/B; collision probability $\approx 2^{-128}$. | **EMPIRICALLY CONFIRMED** | Empirical Observation (Not Malformed Rejection) |
| **test_09_verifier_nonce_replay_protection_defense (AV8)** | Replayed a valid presentation token $T$ with an already consumed challenge nonce. | Stage 0 (Replay Defense) | Verifier checks database state of nonce; status `CONSUMED` triggers immediate token rejection. | **REJECTED (50/50 trials, 100%)** | Malformed Input (Consumed Nonce) |
| **test_10_negative_unregistered_or_substituted_issuer_key (AV9)** | Presenter supplies a self-generated public key or rogue authority DID (`did:xeno:issuer:rogue`). | Stage 2 (Registry Resolution) | Independent `DIDRegistry` lookup rejects mismatched key or unanchored authority DID. | **REJECTED (50/50 trials, 100%)** | Malformed Input |
| **test_11_authenticated_credential_envelope_verification (AV10)** | Injected tampered `schema_id`, `credential_id`, or `subject_did` in presentation metadata. | Stage 4 (PQC Signature) | Candidate envelope digest $M' \ne M$; ML-DSA-65 signature check fails. | **REJECTED (50/50 trials, 100%)** | Malformed Input |
| **test_12_worked_synthetic_credential_specification** | Verified exact leaf hashes, canonical parent hashes, and root derivation for synthetic fixture Alex Carter. | Derivation Verification | Mathematical parity confirmed across all 6 leaves and 3 tree levels. | **PASSED (100% parity)** | Worked Trace Fixture |
| **test_13_classical_ecdsa_baseline_verification** | Executed classical ECDSA secp256k1 baseline issuance, selective disclosure, and verification. | Classical Baseline | Validates baseline functional parity with post-quantum pipeline. | **PASSED (100% parity)** | Comparative Baseline |

---

## 2. Rejection Rate Reconciliation
*   **Total Malformed Input Vectors Evaluated:** 9 distinct attack vectors (AV1, AV2, AV3, AV4, AV5, AV6, AV8, AV9, AV10).
*   **Independent Automated Trials per Vector:** 50 trials.
*   **Total Negative Trials Executed:** $9 \times 50 = 450$ trials.
*   **Total Malformed Inputs Rejected:** 450 / 450 (**100.0% Empirical Rejection Rate**).
*   **Separate Classification of AV7:** Attack Vector 7 is an empirical entropy observation ($H \ge 7.99$ b/B), demonstrating that separately issued credentials diverge completely. However, as documented in Chapter 4, repeated presentations of the *same* credential reveal the identical root $R$, which enables verifier linkability.

---

## 3. Raw Execution Log Output
```text
Creating test database for alias 'default' ('file:memorydb_default?mode=memory&cache=shared')...
Found 13 test(s).
Operations to perform:
  Synchronize unmigrated apps: corsheaders, drf_yasg, messages, rest_framework, staticfiles
  Apply all migrations: admin, auth, contenttypes, core_identity, sessions
Synchronizing apps without migrations:
  Creating tables...
    Running deferred SQL...
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying core_identity.0001_initial... OK
  Applying core_identity.0002_verifiernonce_issuedcredential_envelope_message_and_more... OK
  Applying sessions.0001_initial... OK
System check identified no issues (0 silenced).

test_01_nominal_pqc_selective_disclosure_verification (core_identity.tests.XenoverifyCryptographicTestSuite.test_01_nominal_pqc_selective_disclosure_verification)
Confirms that valid selective disclosure with ML-DSA-65 verifies with 100% success. ... ok
test_02_negative_tampered_disclosed_leaf_value (core_identity.tests.XenoverifyCryptographicTestSuite.test_02_negative_tampered_disclosed_leaf_value)
Negative Test: Altering 1 byte in a revealed attribute value must be rejected at Stage 3. ... ok
test_03_negative_malformed_sibling_proof_path (core_identity.tests.XenoverifyCryptographicTestSuite.test_03_negative_malformed_sibling_proof_path)
Negative Test: Injected corrupted sibling hash in authentication path must fail root climb. ... ok
test_04_negative_corrupted_post_quantum_signature (core_identity.tests.XenoverifyCryptographicTestSuite.test_04_negative_corrupted_post_quantum_signature)
Negative Test: An altered or invalid ML-DSA-65 signature must fail at Stage 4. ... ok
test_05_negative_substituted_signed_root (core_identity.tests.XenoverifyCryptographicTestSuite.test_05_negative_substituted_signed_root)
Negative Test: Providing a substituted Merkle root that does not match the signature. ... ok
test_06_negative_attribute_splicing_across_credentials (core_identity.tests.XenoverifyCryptographicTestSuite.test_06_negative_attribute_splicing_across_credentials)
Negative Test: Mixing an attribute claim from Credential 2 into Credential 1's proof. ... ok
test_07_negative_revoked_credential_circumvention (core_identity.tests.XenoverifyCryptographicTestSuite.test_07_negative_revoked_credential_circumvention)
Negative Test: A revoked credential must be halted at Stage 1 before cryptographic evaluation. ... ok
test_08_presentation_unlinkability_entropy_audit (core_identity.tests.XenoverifyCryptographicTestSuite.test_08_presentation_unlinkability_entropy_audit)
Evaluates presentation unlinkability and privacy: ... ok
test_09_verifier_nonce_replay_protection_defense (core_identity.tests.XenoverifyCryptographicTestSuite.test_09_verifier_nonce_replay_protection_defense)
Validates verifier challenge nonce generation, presentation token binding, ... ok
test_10_negative_unregistered_or_substituted_issuer_key (core_identity.tests.XenoverifyCryptographicTestSuite.test_10_negative_unregistered_or_substituted_issuer_key)
Negative Test: Verifier must independently check the Issuer Registry (Stage 2). ... ok
test_11_authenticated_credential_envelope_verification (core_identity.tests.XenoverifyCryptographicTestSuite.test_11_authenticated_credential_envelope_verification)
Validates issuance and selective disclosure using domain-separated Authenticated ... ok
test_12_worked_synthetic_credential_specification (core_identity.tests.XenoverifyCryptographicTestSuite.test_12_worked_synthetic_credential_specification)
Validates worked synthetic credential construction, exact leaf hashes, ... ok
test_13_classical_ecdsa_baseline_verification (core_identity.tests.XenoverifyCryptographicTestSuite.test_13_classical_ecdsa_baseline_verification)
Validates classical ECDSA secp256k1 baseline issuance and selective disclosure. ... ok

----------------------------------------------------------------------
Ran 13 tests in 5.451s

OK
Destroying test database for alias 'default' ('file:memorydb_default?mode=memory&cache=shared')...
```
