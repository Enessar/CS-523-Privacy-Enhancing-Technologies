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
**Report:** [`report_STOPET_SMC_compiler.pdf`](./report_STOPET_SMC_compiler.pdf)  
**Handout:** [`handout_project_smcompiler.pdf`](./handout_project_smcompiler.pdf)  

*Note: Implementation is private per EPFL course policy.*

---

## SecretStroll: Privacy-Preserving Location Service

A complete privacy-preserving location service enabling **anonymous queries for Points of Interest (POIs)** while defending against identity, location, and behavioral tracking.  
Combines cryptographic authentication, metadata privacy analysis, and machine-learning-based defenses.

**Key Components**
- **Anonymous Authentication:** Implemented Pointcheval–Sanders signatures with the Fiat–Shamir heuristic for non-interactive zero-knowledge proofs, allowing selective disclosure of attributes while preserving anonymity.  
- **Privacy Attack Analysis:** Developed three metadata inference attacks:
  - Home/work inference via temporal clustering.  
  - Interest profiling from POI type frequency.  
  - Behavioral pattern extraction from query timing.  
- **Defenses:** Designed Gaussian noise-based obfuscation (σ ≈ 200 m) to reduce clustering accuracy while maintaining service utility.  
- **Traffic Fingerprinting:** Collected and analyzed 2,000 Tor traces, training ML classifiers on 15+ network features.

**Results**
- Anonymous credential operations: 1.5–11 ms latency with < 2 KB communication.  
- Inference attacks effectively revealed behavioral patterns pre-defense.  
- Location obfuscation destroyed clustering confidence.  
- Traffic fingerprinting achieved ~65 % accuracy despite Tor encryption.

**Insights**
- Cryptographic privacy alone cannot prevent metadata leakage.  
- Noise-based and behavioral defenses can significantly reduce exposure.  
- Even encrypted traffic can leak patterns through timing and packet size.

**Technologies:** Python, Pointcheval–Sanders Signatures, Tor, Scikit-learn, tcpdump, PyShark  
**Type:** Group research project (3 members)

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
