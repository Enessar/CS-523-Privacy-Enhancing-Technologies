# CS-523: Advanced Topics in Privacy Enhancing Technologies

This repository summarizes the projects I completed as part of the **EPFL course CS-523 – Advanced Topics in Privacy Enhancing Technologies (PETs)**.  
Each project explores a different dimension of privacy-preserving computation and secure system design.

---

## Table of Contents

- [SMCompiler: Secure Multiparty Computation Framework](#smcompiler-secure-multiparty-computation-framework)
- [SecretStroll: Privacy-Preserving Location Service](#secretstroll-privacy-preserving-location-service)

---

## SMCompiler: Secure Multiparty Computation Framework

SMCompiler is a Python framework implementing **Secure Multiparty Computation (SMC)** for generic arithmetic circuits.  
It enables several participants to jointly compute a function over their private inputs **without revealing any individual data**, using **additive secret sharing** and **Beaver Triplet-based secure multiplications** under a trusted third party (TTP).

**Core Design**
- Implemented additive secret sharing over a Mersenne prime for efficient modular arithmetic.  
- Designed a recursive arithmetic expression engine that represents computation as abstract syntax trees, allowing operations like `a * b + c` to be securely executed.  
- Developed a Beaver Triplet protocol, with the TTP generating and distributing random shares `[a], [b], [c]` such that `a·b = c`.  
- Implemented a distributed client system managing shares, communication, and synchronization among parties.  
- Added performance instrumentation to monitor computation and communication overheads across different configurations.

**Optimizations**
- Bypassed Beaver multiplication when an operand is scalar, reducing latency for mixed operations.  
- Refactored communication logic to support multiple simultaneous secrets and minimize synchronization steps.  
- Added local caching and partial evaluation to reduce round-trip communication during large circuits.

**Performance Evaluation**
- Conducted experiments varying the number of parties, operations, and circuit depth.  
- Addition and scalar operations achieved near-constant latency (~0.1 ms).  
- Secure multiplications scaled linearly with operation count and party number.  
- Communication overhead dominated only in Beaver-based operations, with predictable linear growth.

**Use Case: Privacy-Preserving Friendship Compatibility**
Developed an application that securely computes a *compatibility score* between participants based on private attributes (e.g., preferences, habits).  
Inputs remain private, and only the joint output is revealed, with optional randomization and rounding to mitigate inference risk.

**Security Model**
- Honest-but-curious parties following the protocol but attempting inference from outputs.  
- Trusted third party assumed for Beaver triplet generation.  
- Secure communication channels; not resistant to active (malicious) adversaries.

**Technologies:** Python 3, Additive Secret Sharing, Beaver Triples, Modular Arithmetic, Secure Computation Framework  
**Type:** Group research project (3 members)  
**Report:** [`report_SMC_compiler.pdf`](./SMCompiler/handout_project_smcompiler.pdf)  
**Handout:** [`handout_project_smcompiler.pdf`](./SMCompiler/handout_project_smcompiler.pdf)  

*Note: Implementation is private per EPFL course policy.*

---

## SecretStroll: Privacy-Preserving Location Service

SecretStroll is a privacy-preserving location-based service designed to allow users to query nearby Points of Interest (POIs) **without revealing their identity, location history, or behavioral patterns**.  
It combines **anonymous authentication**, **metadata privacy analysis**, and **network traffic fingerprinting evaluation** to demonstrate both the strengths and limitations of privacy-enhancing technologies in real-world systems.

**Core Components**
- **Anonymous Authentication:** Implemented **Pointcheval–Sanders attribute-based credentials (ABCs)** with the **Fiat–Shamir heuristic** to achieve non-interactive zero-knowledge proofs.  
  - User-controlled attribute: private secret key (never revealed).  
  - Issuer-controlled attributes: username and valid subscriptions.  
  - Enables selective disclosure of attributes while maintaining unlinkability between queries.
- **System Integration:** Integrated the ABC protocol into a simulated location-based service supporting subscription-based POI retrieval, enabling anonymous query authentication.
- **Testing & Validation:** Built comprehensive unit and integration tests for credential generation, issuance, proof verification, and request signing. Tested both success and failure modes to ensure robustness and protocol correctness.

**Performance Evaluation**
- Conducted 200-run benchmarks across all core operations: key generation, issuance, signing, and verification.  
- **Credential authority generation:** 3.03 ± 0.02 ms; **request signing:** 11.39 ± 0.05 ms.  
- Communication overhead remained <2 KB per interaction.  
- Demonstrated practical performance for real-world deployment of privacy-preserving credentials.

**Metadata Privacy Analysis**
- Evaluated the system’s privacy risks using a simulated dataset of 200 users and 20 days of queries.  
- Identified three critical metadata attacks feasible for an adversarial service provider:
  1. **Home/Work inference:** Deduce users’ primary locations based on query time and coordinates.  
  2. **Interest profiling:** Infer user preferences by frequency analysis of POI types.  
  3. **Behavioral pattern extraction:** Determine daily routines from temporal query patterns.  
- Each attack demonstrated severe privacy leakage, with potential for identity re-linkage or sensitive inference.

**Defenses**
- Implemented **Gaussian noise-based location obfuscation** (σ = 0.002°, ≈200 m) to randomize user coordinates before query submission.  
- Re-running the inference attacks showed that clustering confidence dropped drastically—users’ home and work locations could no longer be reliably identified.  
- Discussed the **privacy–utility trade-off**: higher noise improves privacy but degrades POI relevance.

**Traffic Fingerprinting Analysis**
- Collected 2,000 Tor traffic traces (20 traces × 100 cells) and extracted 15+ metadata features (packet counts, byte volumes, timing distributions).  
- Trained machine-learning classifiers to infer queried grid cells from encrypted Tor traffic.  
- Achieved a **mean classification accuracy of ~65%**, showing that **network metadata can still leak location information** despite encryption and Tor anonymization.

**Countermeasures**
- Proposed mitigations such as:
  - Normalizing response sizes by padding or fragmenting data to equalize traffic patterns.  
  - Batching or dummy queries to blur traffic correlations.  
- Highlighted that defending against traffic analysis requires **protocol-level padding and timing randomization** beyond cryptographic guarantees.

**Technologies:** Python, Pointcheval–Sanders Signatures, Fiat–Shamir Heuristic, Tor, tcpdump, PyShark, Scikit-learn  
**Type:** Group research project (3 members)  
**Report:** [`Atopet_SecretStrol.pdf`](SecretStroll/Atopet_SecretStrol.pdf)  
**Handout:** [`handout_project_secretstroll.pdf`](SecretStroll/handout_project_secretstroll.pdf)

*Note: Implementation is private per EPFL course policy.*

---

## Course Context

**Course:** CS-523 – Advanced Topics in Privacy Enhancing Technologies  
**Institution:** EPFL (École Polytechnique Fédérale de Lausanne)  
**Focus:** Applied cryptography, metadata privacy, secure computation, anonymity systems.  
**Semester:** Fall 2024  

---

### Summary

These projects explore both **cryptographic protocol design** and **privacy engineering**, balancing theoretical rigor with practical system design and evaluation.  
Together, they demonstrate applied work across **secure computation**, **privacy-preserving authentication**, and **metadata resistance**.
