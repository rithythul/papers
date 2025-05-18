# Selendra Neural Network: NEURON Tokenomics and SEL Migration Plan

**Version 1.0**
**Date: May 18, 2025**

## 1. Introduction

This document defines the tokenomics for NEURON, the native utility token of the Selendra Neural Network, and the migration plan for SEL token holders. The NEURON token and the Selendra Neural Network aim to provide a scalable and secure blockchain architecture.

The primary considerations for this tokenomic design and migration are fairness to SEL holders and the creation of a sustainable economic model for the Selendra Neural Network.

## 2. Background: Selendra Network (SEL) Token

The current Selendra Network operates with its native token, SEL. The total minted supply of SEL is ~227 million (Q2 2025), with an annual inflation rate of ~21 million SEL, primarily for validator rewards. SEL is integral to the current network for transaction fees, staking, and governance.

## 3. Introducing the NEURON Token

Selendra Neural Network will introduce a new native utility token, NEURON.

### 3.1 Rationale for a New Token

Architectural changes in the Selendra Neural Network, including the Synaptic Ledger, Dynamic Synaptic Clusters, and native Account Abstraction, require a new economic model. The NEURON token aligns with these capabilities.

### 3.2 Token Supply

NEURON has a target total supply of ~86 billion units. This supply allows for fine-grained transactions.

### 3.3 Core Utilities of NEURON

NEURON is essential for the operation and governance of the Selendra Neural Network. Primary utilities (detailed in Selendra Neural Network Whitepaper, Section 7.1):

1.  **Network Security (Staking):** Validators stake NEURON to participate in consensus.
2.  **Transaction Fees:** NEURON is used for network transactions and smart contract operations. A portion of fees may be burned, creating deflationary pressure.
3.  **Governance Participation:** Staked NEURON grants holders rights to propose and vote on protocol changes.
4.  **Access to Network Services:** NEURON will be required for specialized services (e.g., DSC registration, premium name services).

### 3.4 Ecosystem Stablecoins

The Selendra Neural Network will support native stablecoins (e.g., pegged to USD and KHR) to facilitate commerce and DeFi. These stablecoins will have their own backing mechanisms and economic models, detailed in a separate whitepaper. They are distinct from NEURON, the network's primary utility and governance token.

## 4. SEL to NEURON Migration Plan

A fair and transparent migration for SEL holders is a priority. This plan details the re-denomination of SEL into NEURON, forming the initial community-held circulating supply.

### 4.1 Objective

Convert SEL to NEURON at a defined ratio, ensuring SEL holders receive a proportional share. This forms the initial community-held NEURON supply.

### 4.2 Snapshot and Eligibility

- A **snapshot** of all SEL token balances on the Selendra Network will be taken at a specific, pre-announced block height. This snapshot will serve as the definitive record for determining NEURON allocation for SEL holders.
- All SEL holders at the time of the snapshot will be eligible for the migration.

### 4.3 Re-denomination of SEL Supply into NEURON

- The ~227 million SEL minted supply is the basis for the initial NEURON tokens distributed to existing holders.
- A **Re-denomination Factor** will be established.
  `NEURON amount for holder = SEL amount held at snapshot * Re-denomination Factor`
- The Selendra core team will determine and communicate the Re-denomination Factor, considering NEURON's granularity and the ~86 billion target supply.
- The total NEURON from this re-denomination will represent tokens claimable by existing SEL holders.

### 4.4 Claiming Mechanism

- A secure **claiming portal** or smart contract will be deployed on the Selendra Neural Network.
- SEL holders will prove ownership of their SEL (e.g., by signing a message with their SEL private key) to claim NEURON.

### 4.5 Migration Window

- A **migration window** (e.g., 12-24 months) will allow sufficient time for claims.

### 4.6 Unclaimed NEURON Tokens

- Network governance will determine the policy for unclaimed NEURON from the migration allocation after the window closes. Options include burning or transferring to the community treasury.

### 4.7 Non-Circulating and Vested SEL

- SEL tokens held by the team, foundation, or subject to existing vesting will also be converted to NEURON by the Re-denomination Factor. Converted NEURON from vested SEL will follow new or continued vesting schedules on the Selendra Neural Network, disclosed publicly. These tokens are not part of the initial free-circulating supply until vested.

## 5. NEURON Token Supply Management and Long-Term Economics

The Selendra Neural Network has a potential total NEURON supply of ~86 billion. This supply is not fully minted at launch but introduced methodically over time. This approach supports initial launch, long-term participation, security, development, and economic sustainability.

Initial circulating supply will primarily be NEURON from the SEL re-denomination (Section 4). Team and foundation allocations are vested. A large portion of the potential supply is for gradual release (e.g., network inflation, other controlled channels).

### 5.1 NEURON Supply Allocation and Distribution Mechanisms

The ~86 billion NEURON potential supply is allocated as follows, with release mechanisms supporting network growth and security:

1.  **Initial Community Supply (from SEL Re-denomination):**

    - **Illustrative Allocation:** Approx. **5%** of total supply (~4.3 billion NEURON).
      - _Note: This figure implies a Re-denomination Factor of approximately 1 SEL = 18.94 NEURON (4.3 billion NEURON / 227 million SEL). The final percentage and Re-denomination Factor will be confirmed by the Selendra core team._
    - **Source:** Conversion of the ~227 million SEL supply.
    - **Circulation:** Claimable by SEL holders at network launch, forming the primary initial circulating supply.

2.  **Network Security & Staking Rewards (Future Issuance):**

    - **Illustrative Allocation:** Approx. **41.4%** of total supply (~35.604 billion NEURON).
    - **Source:** From potential supply for future staking rewards.
    - **Mechanism:** Gradual release over years via inflation (Section 5.2) to incentivize validators and stakers.

3.  **Ecosystem Development Fund (Managed Release):**

    - **Illustrative Allocation:** Approx. **22%** of total supply (~18.92 billion NEURON).
    - **Source:** Dedicated allocation from potential supply, locked at genesis (distinct from network inflation).
    - **Mechanism:** Disbursed via grants for dApp/tool/infra development. Governed by Foundation or community.

4.  **Community Treasury (Managed Release / Future Issuance):**

    - **Illustrative Allocation:** Approx. **8%** of total supply (~6.88 billion NEURON).
    - **Source:** Potential funding from unclaimed tokens, inflation percentage, network revenue, or direct allocation.
    - **Mechanism:** Governed by token holders for strategic initiatives. Funds released via governance proposals.

5.  **Team & Advisors (Vested Allocation):**

    - **Illustrative Allocation:** Approx. **12%** of total supply (~10.32 billion NEURON).
    - **Source:** NEURON allocation (possibly supplemented from potential supply) beyond individual team SEL conversions. Subject to long-term vesting.
    - **Mechanism:** Gradual release (e.g., 1-year cliff, 3-4 year linear vesting) to align incentives. Not initial circulating supply.

6.  **Foundation / Protocol Development (Managed Release / Vested Allocation):**

    - **Illustrative Allocation:** Approx. **6.6%** of total supply (~5.676 billion NEURON).
    - **Source:** Dedicated allocation from potential supply for protocol R&D, audits, operations.
    - **Mechanism:** Released as needed, with transparency/governance, possibly vested.

7.  **Public Sale / Liquidity Provision (If Applicable, Managed Release):**
    - **Illustrative Allocation:** Approx. **5%** of total supply (~4.3 billion NEURON).
    - **Source:** From potential supply if a public sale/liquidity program occurs.
    - **Mechanism:** Per sale/program terms.

This ensures initial supply is based on SEL distribution, with the ~86 billion potential realized progressively. Allocations beyond initial community supply are vested or managed release, not immediate circulating supply.

### 5.2 NEURON Inflation Model

- **Purpose:** Ongoing incentives for validators/stakers.
- **Initial Rate:** Low annual percentage (e.g., 0.5%-3%) of current circulating supply, calibrated for competitive yields and supply management.
- **Inflation Schedule:** Potentially disinflationary (rate decreases over time) or dynamically adjusted by governance for security/staking targets.
- **Max Supply:** ~86 billion NEURON may be an initial target with ongoing low inflation for security, or a hard cap with rewards later from transaction fees. Subject to governance.

### 5.3 Fee Mechanism

- An EIP-1559-like fee mechanism will be implemented (details in main whitepaper). A portion of transaction base fees (NEURON) will be burned, creating deflationary pressure with network activity.

## 6. Ensuring Long-Term Value and Sustainability

NEURON's long-term value depends on the Selendra Neural Network's success, utility, and adoption. The model aims to:

- **Incentivize Security:** Staking rewards encourage validation participation.
- **Drive Demand:** Utilities (fees, governance, services) create NEURON demand.
- **Promote Decentralization:** Wide distribution and community governance.
- **Adaptability:** On-chain governance allows parameter adaptation.

## 7. Conclusion

The SEL to NEURON transition and new tokenomics are key for an advanced, scalable blockchain. This plan prioritizes fair migration for SEL holders. NEURON's design supports long-term security, ecosystem growth, and decentralized governance for the Selendra Neural Network.

The Selendra team will maintain communication and transparency during finalization and implementation. Community feedback is important.

---

**Disclaimer:** This document is for informational purposes only and is subject to change. The Selendra team will publish definitive details regarding the tokenomics and migration plan prior to implementation.
