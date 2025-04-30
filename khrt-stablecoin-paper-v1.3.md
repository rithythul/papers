# KHRt: Extending the Cambodian Riel to Open Blockchain Networks

_Whitepaper v1.3 – April 2025_
_By Selendra Team_

## Abstract

We propose a blockchain-based digital token system that extends the Cambodian riel's utility to the open blockchain network ecosystem. While National Bank of Cambodia's Bakong successfully handles interbank transfers on its blockchain, KHRt enables permissionless, programmable riel-denominated value transfers across multiple public blockchains. Each KHRt token represents exactly one Cambodian riel, with all tokens backed by reserves held in regulated financial institutions. The system implements a cryptographically verified reserve proof mechanism to ensure the circulating supply never exceeds the reserve amount.

## 1. Introduction

The success of USD-pegged stablecoins has demonstrated how blockchain technology can extend a national currency's utility to the digital environment. These stablecoins have significantly enhanced USD's role in global digital transactions, with daily trading volume exceeding $100 billion (CoinMarketCap, 2024). This proven model shows how a national currency can maintain its prominence in the evolving digital economy while preserving its monetary sovereignty.

Cambodia's financial infrastructure has made significant progress with the National Bank of Cambodia leading the way with its Bakong Blockchain that enables domestic interbank transfers and payments. However, the permissioned nature of Bakong, while appropriate for the interbank settlement, limits the riel's participation in the broader blockchain ecosystem. Just as USD-pegged stablecoins have expanded the USD's utility, KHRt extends the Cambodian riel's utility to the blockchain ecosystem. KHRt addresses these limitations by enabling permissionless, programmable riel-denominated value transfers across multiple public blockchains.

## 2. Background

Cambodia's financial system faces several structural challenges:

### 2.1 Dollarized Economy

68% of transactions in Cambodia are conducted in USD despite government de-dollarization initiatives (IMF, 2023). This high level of dollarization:

- Reduces monetary policy effectiveness
- Creates currency mismatch risks for businesses and individuals
- Limits the National Bank of Cambodia's ability to manage the money supply

### 2.2 Remittance Inefficiencies

Cambodia receives approximately $1.2 billion in annual remittances (World Bank, 2024) with significant friction:

- Average fees of 5.4% of transaction value
- Processing times of 2-5 business days
- Limited operating hours for receipt
- Physical pickup requirements in many cases

### 2.3 Limited Programmability

Current payment systems in Cambodia lack programmable functionality:

- Conditional payments require manual verification
- Automated recurring payments are limited to enterprises
- Smart contract capabilities are unavailable in existing systems
- Time-locked transactions are not supported

### 2.4 Financial Inclusion Barriers

Despite high mobile penetration (124%) and smartphone adoption (84%), financial inclusion remains limited:

- Only 33% of Cambodians have access to banking
- 78% of the rural population lacks access to comprehensive financial services
- Minimum transaction thresholds (50,000-100,000 KHR) exclude smaller payments
- Limited banking hours restrict access for many workers

### 2.5 Cross-Border Payment Friction

International payments to and from Cambodia face significant challenges:

- High costs (3-7% of transaction value)
- Long settlement times (2-5 days)
- Limited transparency in exchange rates
- Restricted access to global financial networks

These challenges create an opportunity for a blockchain-based solution that addresses these specific pain points while supporting Cambodia's national currency.

The National Bank of Cambodia has addressed some of these challenges through Bakong, which provides:

- Domestic interbank transfers
- Real-time settlement
- Mobile payments
- KHQR-based transactions
- Now integrated with neighboring countries' QR systems such as THQR, VNQR, JPQR for cross-border payments

While Bakong excels in its designed role, the global success of USD stablecoin shows the additional value that can be created by extending a national currency's utility to permissionless blockchain networks and programmable applications. This opportunity remains open for the Cambodian riel (KHR).

## 3. KHRt Design

### 3.1 System Overview

KHRt is a digital token system that represents the Cambodian riel (KHR) on blockchain networks. Each KHRt token is backed 1:1 by KHR held in reserve accounts at regulated Cambodian financial institutions. The system enables:

1. Peer-to-peer transfers of riel value
2. Programmable payments through smart contracts
3. Cross-chain interoperability
4. Full regulatory compliance
5. 24/7 operation beyond banking hours
6. Developers and SMEs friendly

### 3.2 Key Design Principles

The KHRt system is designed and operates as a complementary system according to the following principles:

1. **Full Reserve Backing**: Every KHRt token in circulation is backed by at least one KHR in reserve
2. **Regulatory Compliance**: All operations adhere to Cambodian and international financial regulations
3. **Transparency**: Reserve balances and token supply are regularly verified and publicly reported
4. **Security**: Multi-layer security protections for all system components
5. **Interoperability**: Ability to function across multiple blockchain networks

### 3.3 Reserve Management

The reserve backing KHRt follows a mathematical formula that ensures the system maintains full collateralization plus a safety buffer:

R ≥ C × (1 + B)

Where:

- R = Total value of reserves in KHR
- C = Total circulating supply of KHRt
- B = Buffer percentage (minimum 5%)

For example, with 100 billion KHRt in circulation and a 5% buffer:

- Required reserve = 100 billion × (1 + 0.05) = 105 billion KHR

The system enforces this constraint through smart contract logic with the following rules:

1. **Minting Constraint**: New tokens can only be minted when sufficient reserves exist:

   - Minting is only allowed when: R - (C + M) × (1 + B) ≥ 0
   - Where M is the amount to be minted

2. **Redemption Priority**: During periods of high activity, redemptions are prioritized over new minting to maintain the reserve ratio

3. **Buffer Adjustment**: The buffer percentage B can be adjusted based on market conditions through governance, with a minimum floor of 5% and a maximum ceiling of 20%

4. **Reserve Verification**: The formula is verified through both:
   - On-chain cryptographic proofs (zkSNARK-based verification)
   - Off-chain audits by independent accounting firms

This mathematical model ensures that KHRt maintains its 1:1 peg with the Cambodian riel while providing a safety margin against market volatility.

### 3.4 Advisory Alliance

KHRTrust aims to build an alliance of trusted financial institutions, technology partners, and regulatory experts to provide oversight and guidance for KHRt's reserve backing. This advisory alliance will:

- Enhance transparency through multi-stakeholder oversight
- Review reserve verification processes and audit results
- Provide balanced expertise across financial, technical, and regulatory domains
- Contribute to public reporting standards and best practices

The alliance will serve in an advisory capacity to KHRTrust, helping to ensure that reserve management remains transparent, secure, and aligned with industry best practices while maintaining the trust of all stakeholders.

### 3.5 Operational Entity

KHRTrust is a financial technology company incorporated in Cambodia that manages the KHRt system. The company:

- Maintains the reserve assets backing KHRt tokens
- Manages the minting and burning processes
- Ensures ongoing compliance with regulatory requirements
- Implements security measures and auditing procedures
- Consults with the advisory alliance on reserve management practices

KHRTrust operates under appropriate licenses for payment service provision and digital asset operations, in consultation with the National Bank of Cambodia and relevant regulatory bodies. The company maintains ultimate operational responsibility while benefiting from the expertise and oversight provided by the advisory alliance.

## 4. Technical Implementation

### 4.1 System Architecture

The KHRt implementation comprises a multi-layered technical architecture:

#### 4.1.1 Key Technical Components

- **Smart Contract Layer**: ERC-20 compatible implementation on Selendra with cross-chain bridges, formal verification by established security firms (quarterly audits), role-based access controls (3-of-5 multi-sig), and emergency pause functionality
- **Custody Framework**: Shamir's Secret Sharing (5-of-8), FIPS 140-2 Level 4 HSMs, cold storage for 85% of reserves, and geographically distributed key fragments
- **Auditing System**: zkSNARK-based proof of reserve (weekly verification), on-chain verification through Merkle proofs, independent accounting firm validation (monthly), and real-time reconciliation
- **Compliance Module**: Real-time AML/CFT screening with major providers, risk scoring based on transaction patterns, FATF Travel Rule compliance for transactions >1M KHR, and automated regulatory reporting

#### 4.1.2 Technology Stack Flow

The KHRt technology stack is organized in layers that ensure security, scalability, and interoperability:

```
┌───────────────────────┐     ┌───────────────────────┐     ┌───────────────────────┐
│   User Applications   │     │   Partner Services    │     │   Financial Systems   │
│                       │     │                       │     │                       │
│ ┌─────────────────┐   │     │ ┌─────────────────┐   │     │ ┌─────────────────┐   │
│ │ Mobile Wallets  │   │     │ │ Banking APIs    │   │     │ │ Bakong          │   │
│ └─────────────────┘   │     │ └─────────────────┘   │     │ └─────────────────┘   │
│ ┌─────────────────┐   │     │ ┌─────────────────┐   │     │ ┌─────────────────┐   │
│ │ Web Interfaces  │   │     │ │ Payment Systems │   │     │ │ SWIFT           │   │
│ └─────────────────┘   │     │ └─────────────────┘   │     │ └─────────────────┘   │
│ ┌─────────────────┐   │     │ ┌─────────────────┐   │     │ ┌─────────────────┐   │
│ │ Merchant Tools  │   │     │ │ Exchange APIs   │   │     │ │ Banking Network │   │
│ └─────────────────┘   │     │ └─────────────────┘   │     │ └─────────────────┘   │
└──────────┬────────────┘     └──────────┬────────────┘     └──────────┬────────────┘
           │                             │                             │
           ▼                             ▼                             ▼
┌───────────────────────┐     ┌───────────────────────┐     ┌───────────────────────┐
│   KHRt API Layer      │◄───►│  KHRt Service Layer   │◄───►│  Reserve Management   │
│                       │     │                       │     │                       │
│ ┌─────────────────┐   │     │ ┌─────────────────┐   │     │ ┌─────────────────┐   │
│ │ REST APIs       │   │     │ │ Business Logic  │   │     │ │ Treasury System │   │
│ └─────────────────┘   │     │ └─────────────────┘   │     │ └─────────────────┘   │
│ ┌─────────────────┐   │     │ ┌─────────────────┐   │     │ ┌─────────────────┐   │
│ │ SDK             │   │     │ │ Compliance      │   │     │ │ Audit System    │   │
│ └─────────────────┘   │     │ └─────────────────┘   │     │ └─────────────────┘   │
│ ┌─────────────────┐   │     │ ┌─────────────────┐   │     │ ┌─────────────────┐   │
│ │ Authentication  │   │     │ │ KYC/AML         │   │     │ │ Reconciliation  │   │
│ └─────────────────┘   │     │ └─────────────────┘   │     │ └─────────────────┘   │
└──────────┬────────────┘     └──────────┬────────────┘     └──────────┬────────────┘
           │                             │                             │
           └─────────────────────────────┼─────────────────────────────┘
                                         ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              Blockchain Layer                                   │
│                                                                                 │
│  ┌───────────────────────┐                      ┌───────────────────────┐       │
│  │   Selendra Network    │                      │     Cross-Chain       │       │
│  │    (Primary Chain)    │◄────────────────────►│       Bridges         │       │
│  └───────────────────────┘                      └───────────────────────┘       │
│                                                                                 │
│  ┌───────────────────────┐                      ┌───────────────────────┐       │
│  │   Smart Contracts     │                      │    Oracle Services    │       │
│  │                       │                      │                       │       │
│  │ ┌─────────────────┐   │                      │ ┌─────────────────┐   │       │
│  │ │ Minting/Burning │   │                      │ │ Price Feeds     │   │       │
│  │ └─────────────────┘   │                      │ └─────────────────┘   │       │
│  │ ┌─────────────────┐   │                      │ ┌─────────────────┐   │       │
│  │ │ Governance      │   │                      │ │ External Data   │   │       │
│  │ └─────────────────┘   │                      │ └─────────────────┘   │       │
│  └───────────────────────┘                      └───────────────────────┘       │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Operational Flow

The minting and redemption processes follow specific operational flows:

```
┌──────────┐      ┌──────────────┐      ┌─────────────┐      ┌──────────────┐
│          │ KHR  │              │ API  │             │      │              │
│  User    ├─────►│  Bank Partner├─────►│ Mint Oracle ├─────►│ Custody      │
│          │      │              │      │             │      │ Module       │
└──────────┘      └──────────────┘      └─────────────┘      └──────┬───────┘
                                                                    │
                                                                    │ Multi-sig
                                                                    ▼
┌──────────┐      ┌──────────────┐      ┌─────────────┐      ┌──────────────┐
│          │ KHRt │              │      │             │      │              │
│  User    │◄─────┤  User Wallet │◄─────┤ KHRt Token  │◄─────┤ Minting      │
│          │      │              │      │ Contract    │      │ Module       │
└──────────┘      └──────────────┘      └─────────────┘      └──────────────┘
```

**Process Flow Metrics:**

- Average minting time: 15 minutes (from fiat deposit to token receipt)
- Average redemption time: 30 minutes (from token burn to fiat availability)
- Transaction finality: 2-3 minutes (network confirmation time)
- Gas costs: Optimized to remain below 0.1% of transaction value

### 4.3 Token Supply Mechanics

**Minting Process:**

1. User deposits KHR at authorized financial institution
2. Institution verifies deposit and notifies KHRTrust via API
3. Oracle validates confirmation using cryptographic signatures
4. Multi-signature approval process initiated (3-of-5 required)
5. Smart contract mints exact amount of KHRt tokens
6. Tokens transferred to user's designated wallet

**Burning Process:**

1. User initiates redemption through application or partner institution
2. KHRt tokens locked in redemption contract
3. Verification process confirms compliance requirements
4. Smart contract burns tokens upon confirmation
5. Banking partner notified to release equivalent KHR
6. User receives KHR funds in designated account

### 4.4 Flow of Funds

The circulation of KHRt follows a clear five-step process that maintains the integrity of the 1:1 peg with the Cambodian riel:

```
                                3
                 ┌─────────────────────────────┐
                 │         KHRt sent           │
                 │                             │
            ┌─────────┐    ┌─────────┐    ┌─────────┐
            │  KHRt   │◄───┤  KHRt   ├───►│  KHRt   │
            │  user   │    │  user   │    │  user   │
            └─────────┘    └─────────┘    └─────────┘
                 ▲                             │
                 │                             │
       2         │                             │         4
  ┌──────────────┘                             └─────────────┐
  │    KHRt issued                               KHRt redeemed
  │                                                          │
  │               ┌───────────────────────┐                  │
  └──────────────►│  Riel reserves held   │◄─────────────────┘
                  │     by KHRTrust       │
                  └───────────────────────┘
                    ▲                   │
                    │       1           │        5
                    │                   ▼
               ┌─────────┐        ┌─────────┐
               │  Riel   │        │  Riel   │
               │   In    │        │   Out   │
               └─────────┘        └─────────┘
```

1. **Deposit**: Users deposit KHR into designated accounts at KHRTrust's banking partners
2. **Issuance**: KHRTrust mints equivalent KHRt tokens and transfers them to the user's wallet
3. **Circulation**: Users transfer KHRt tokens between wallets on the blockchain
4. **Redemption**: Users send tokens to a redemption address to initiate burning
5. **Withdrawal**: Banking partners transfer physical riel to the user's account

Throughout this process, the total supply of KHRt in circulation always equals the amount of physical riel held in reserve, maintaining the 1:1 peg.

## 5. Security and Risk Management

### 5.1 Security Framework

KHRt's security architecture follows a defense-in-depth approach with multiple layers of protection:

1. **Smart Contract Security:** Formal verification, multiple audits, time-locked upgrades, and bug bounty program

2. **Cryptographic Security:** Industry-standard encryption, Shamir's Secret Sharing (5-of-8), HSMs, and zero-knowledge proofs

3. **Operational Security:** Role-based access controls, multi-factor authentication, and segregation of duties

4. **Network Security:** DDoS protection, intrusion detection, regular penetration testing, and isolated environments

5. **Incident Response:** 24/7 monitoring, predefined playbooks, and transparent disclosure policy

### 5.2 Risk Management Framework

KHRt implements comprehensive risk mitigation across four key domains:

1. **Reserve Protection**: Diversification across institutions (30% maximum per institution), 5% reserve buffer, insurance coverage (covering theft, fraud, and operational errors), and weekly cryptographic verification

2. **Technical Safeguards**: Multi-layer security architecture, quarterly penetration testing by independent security firms, bug bounty program with tiered rewards, and open-source code review

3. **Operational Continuity**: Geographically distributed teams, 24/7 monitoring with automated alerting, secondary partner institutions with standby agreements, and quarterly disaster recovery testing

4. **Financial Protections**: 24-month operating reserve maintained separately from token reserves, legal defense fund, structured wind-down procedures with regulatory oversight, and legally segregated accounts

### 5.3 Risk Analysis Matrix

| Risk Category                    | Probability | Impact | Key Mitigation Strategies                                |
| -------------------------------- | ----------- | ------ | -------------------------------------------------------- |
| **Reserve Bank Failure**         | Low         | High   | Multi-institution distribution, backup transfers         |
| **Smart Contract Vulnerability** | Medium      | High   | Formal verification, audits, emergency pause             |
| **Regulatory Changes**           | Medium      | Medium | Proactive engagement, adaptable compliance               |
| **Market Liquidity Crisis**      | Low         | Medium | Liquidity incentives, reserve buffer, redemption limits  |
| **Bridge Security Failure**      | Medium      | High   | Limited exposure, monitoring, automatic circuit breakers |
| **Key Management Failure**       | Very Low    | High   | Shamir's Secret Sharing, HSMs, recovery procedures       |

### 5.4 Circuit Breaker Mechanisms

The system implements automatic safety mechanisms to protect the network during unusual conditions:

1. **Reserve Protection:** Pauses minting if reserves fall below 101% of circulation
2. **Volatility Management:** Implements gradual redemption limits during market volatility
3. **Anomaly Detection:** Uses machine learning to detect and respond to abnormal patterns
4. **Emergency Controls:** Provides governance override with multi-signature requirements

### 5.5 Cross-Chain Security

KHRt implements specialized cross-chain security measures to address the significant risks associated with cross-chain bridges:

1. **Bridge Capacity Restrictions**: Maximum 5% of total KHRt supply per bridge to limit exposure
2. **Multi-Layer Security**:
   - Multi-signature validation requiring 5-of-8 approvers for bridge transactions
   - Timelock periods (24-72 hours) for large transfers exceeding 500M KHR
   - Independent security audits of all bridge code before deployment
3. **Monitoring and Response**:
   - Chain-specific monitoring with dedicated anomaly detection
   - Dynamic liquidity management to prevent bridge exploitation
   - Automatic circuit breakers that suspend operations if suspicious patterns are detected
4. **Recovery Procedures**:
   - Predefined incident response playbooks for bridge-specific scenarios
   - Reserve allocation for potential bridge compromise recovery
   - Regular bridge security drills and simulations

### 5.6 Implementation Weaknesses and Trust Considerations

While KHRt aims to leverage blockchain technology for transparency and security, we acknowledge that our implementation is not fully trustless. Users must trust KHRTrust and our banking partners as custodians of the reserve assets. We believe honesty about these limitations is essential, so we directly acknowledge the following risks:

- **We could go bankrupt** - KHRTrust as a company could face financial difficulties or insolvency
- **Our banks could go insolvent** - Financial institutions holding the reserves could fail
- **Our banks could freeze or confiscate the funds** - Banking partners could restrict access to reserves
- **We could abscond with the reserve funds** - Theoretical risk of internal misappropriation exists
- **Re-centralization of risk to a single point of failure** - Despite blockchain technology, certain operations remain centralized

These risks are inherent to most asset-backed stablecoins and traditional financial services. Users of exchanges and wallets already face many of these same risks when dealing with digital assets.

**How We Address These Risks:**

1. **If KHRTrust went bankrupt:**

   - Reserve assets are legally segregated from operational funds
   - Corporate structure ensures reserves remain available for redemption
   - Smart contracts continue to function independently of company operations

2. **If our banks went insolvent:**

   - Reserves are distributed across multiple institutions (maximum 30% per institution)
   - Banking partners are selected based on rigorous financial stability criteria
   - Contingency relationships with backup financial institutions are maintained

3. **If our banks froze or confiscated funds:**

   - Banking partners are fully informed about our business model
   - We maintain compliance with all regulatory requirements
   - Legal agreements protect against arbitrary freezes
   - Multiple banking relationships provide redundancy

4. **To prevent misappropriation of funds:**

   - Multi-signature requirements for all reserve transfers
   - Public corporate structure with known principals
   - Regular independent audits with published results
   - Advisory alliance providing additional oversight

5. **To reduce centralization risks:**
   - Decentralized token storage and transfer on blockchain
   - Open-source smart contracts with formal verification
   - Transparent reserve verification mechanisms
   - Progressive decentralization of operational components where possible

We believe that by acknowledging these limitations directly and implementing robust mitigation strategies, we can build a system that balances the benefits of blockchain technology with the practical realities of integrating with the existing financial system.

## 6. Applications and Use Cases

KHRt can be applied to several use cases in the Cambodian financial ecosystem:

### 6.1 Cross-Border Payments and Remittances

Market Context: Cambodia receives $1.2 billion in annual remittances with fees averaging 5.4% and 2-5 day processing times.

KHRt Application: Offers 0.5% transaction fees, 15-minute settlement, and 24/7 operation.

Example: A worker sending $200 from Thailand would pay $1 instead of $10.80, with funds received in minutes rather than days.

### 6.2 Supply Chain Finance

Market Context: Cambodia's exports total $6.2 billion, with payment terms of 60-90 days and documentary credit costs around 3%.

KHRt Application: Provides smart contract escrow, digital document verification, and programmable milestone payments.

Example: A rice exporter could reduce payment waiting time from 60 to 45 days on $500,000 shipments, lowering working capital requirements.

### 6.3 Agricultural Sector Applications

Market Context: Cambodia's agricultural sector has a $450 million financing gap, with only 22% of farmers accessing formal credit.

KHRt Application: Enables weather-triggered conditional payments, automated insurance claims, and supply chain verification.

Example: Rice cooperatives can implement weather-indexed insurance with faster payouts and lower administrative costs.

### 6.4 Small Business Payments

Market Context: Cambodia has 580,000 MSMEs, with 73% lacking access to formal financial services.

KHRt Application: Offers automated supplier payments, programmable recurring payments, and digital reconciliation.

Example: Small businesses can automate payment processes, reducing time spent on payment management.

### 6.5 Cryptocurrency Market Access

Market Context: Cambodian investors seeking to access cryptocurrency markets face high fees, limited transparency, and regulatory uncertainty.

KHRt Application: Provides a regulated on/off-ramp for cryptocurrencies with transparent pricing, verifiable reserves, and reduced counterparty risk.

Example: A Cambodian investor can convert KHR to KHRt through a regulated partner, then exchange for Bitcoin with lower fees (1.5% vs 3-5%), transparent pricing, and full regulatory compliance, creating a secure pathway for Cambodian participation in global cryptocurrency markets.

## 7. Regulatory Compliance

KHRt operates within a comprehensive regulatory framework designed for compliance with both Cambodian and international standards:

### 7.1 Regulatory Alignment

**Cambodian Compliance:** Formal consultation with NBC under Prakas No. B7-08-089, Payment System License under Law on Payment Systems (2019), KYC/AML implementation, and compliance with data protection regulations.

**International Standards:** Implementation of FATF guidance for Virtual Asset Service Providers, Travel Rule compliance for transactions >1M KHR, ISO 20022 message standardization, and GDPR-inspired data protection.

### 7.2 Tiered Access Framework

| User Tier             | Requirements                       | Transaction Limits                | Key Features                                       |
| --------------------- | ---------------------------------- | --------------------------------- | -------------------------------------------------- |
| **Tier 1 (Basic)**    | Phone verification                 | 4M KHR daily / 20M KHR monthly    | Basic transfers and payments                       |
| **Tier 2 (Standard)** | ID verification                    | 20M KHR daily / 100M KHR monthly  | + Programmable payments, cross-chain functionality |
| **Tier 3 (Enhanced)** | Enhanced due diligence             | 100M KHR daily / 500M KHR monthly | + Enterprise functions, API access                 |
| **Institutional**     | Full KYB, licenses, on-site review | Customized risk-based limits      | + Direct mint/burn privileges, dedicated support   |

## 8. Implementation Timeline

KHRt will be implemented in five strategic phases over a three-year period:

**Phase 1: Foundation (Months 1-3)**

- Legal structure establishment and regulatory approvals
- Core smart contract development and security audits
- Initial banking partnerships (minimum 3 institutions)
- Advisory alliance formation
- Success criteria: Legal entity established, contracts audited, banking agreements signed

**Phase 2: Limited Launch (Months 4-6)**

- Institutional partner pilot with 5 financial institutions
- 5 billion KHR initial reserve establishment
- Bakong integration for fiat on/off ramps
- Selendra network deployment and testing
- Success criteria: Successful test transactions, 5 billion KHR in reserves, functional Bakong integration

**Phase 3: Public Expansion (Months 7-12)**

- Public access through partner institutions
- Mobile wallet integration and user interface development
- Cross-chain bridge deployment (starting with Ethereum)
- Enhanced security systems and monitoring implementation
- Success criteria: 1,000+ end users, 20+ billion KHR in circulation, operational cross-chain bridge

**Phase 4: Ecosystem Growth (Months 13-24)**

- Developer tools and SDK release
- International corridor expansion (Thailand, Vietnam, Singapore)
- Enterprise API offering for business integration
- Treasury management systems enhancement
- Success criteria: 10+ developer projects, 3+ international corridors, 50+ billion KHR in circulation

**Phase 5: Advanced Features (Months 25-36)**

- Expanded interoperability with additional blockchains
- Advanced payment capabilities (programmable escrow, conditional transfers)
- Specialized sector solutions (agriculture, supply chain)
- Enhanced governance and transparency mechanisms
- Success criteria: Support for 5+ blockchains, 100+ billion KHR in circulation, 10+ specialized applications

## 9. Future Considerations and Business Model

As KHRt matures, KHRTrust may explore opportunities to enhance the system while maintaining its core principles of security, compliance, and stability. Any future developments would be implemented only after thorough regulatory consultation and risk assessment.

### 9.1 Future Development Areas

Potential areas for future exploration include:

1. **Reserve Optimization**: KHRTrust may evaluate options to diversify a small portion of reserves into secure instruments like Cambodian government bonds while maintaining a substantial cash reserve (minimum 80%) for redemptions.

2. **Enhanced Utility**: Development of additional financial services and tools that leverage the KHRt infrastructure to address specific needs in the Cambodian market.

3. **Interoperability Enhancements**: Developing additional integrations with existing financial infrastructure to improve accessibility and utility.

All future developments will adhere to KHRTrust's foundational commitments to full reserve backing, regulatory compliance, and transparent operations.

### 9.2 Business Model and Sustainability

KHRTrust's business model is designed to ensure long-term sustainability while maintaining the security and stability of the KHRt system:

1. **Revenue Sources**:

   - Transaction fees (0.1-0.5% depending on volume and tier)
   - Interest income from a portion of reserves (subject to regulatory approval)
   - Enterprise API and integration services
   - Premium service fees for institutional clients

2. **Cost Structure**:

   - Banking and custody fees
   - Security and compliance infrastructure
   - Technical development and maintenance
   - Operational expenses and team

3. **Financial Sustainability**:
   - 24-month operational runway maintained at all times
   - Gradual fee reduction as volume increases
   - Reinvestment of profits into security and feature development
   - Transparent financial reporting to stakeholders

This business model ensures that KHRTrust can maintain the KHRt system while continuously improving security, compliance, and utility for users.

## 10. Conclusion

KHRt is a blockchain-based digital token system that represents the Cambodian riel on public blockchain networks. The system maintains a 1:1 peg through full reserve backing while adding programmable functionality through smart contracts, combining the stability of the national currency with the technical capabilities of blockchain networks.

The KHRt system provides regulatory compliance, programmable payment capabilities, lower transaction costs, faster settlement times, broader financial access, enhanced security, and transparent governance.

The system supports Cambodia's economic priorities by providing a digital alternative to foreign currencies, reducing barriers to financial services, enabling programmable money applications, and facilitating cross-border payments.

KHRt complements the National Bank of Cambodia's Bakong system by extending the riel's utility to permissionless blockchain networks. While Bakong serves interbank settlement within a permissioned environment, KHRt enables the riel to participate in the broader blockchain ecosystem.

Future development priorities include expanding blockchain compatibility, developing financial services, exploring CBDC integration, and creating ASEAN payment corridors.

By providing infrastructure for using the Cambodian riel in digital transactions and blockchain applications, KHRt contributes to the modernization of Cambodia's financial system while preserving monetary sovereignty.

## References

- National Bank of Cambodia. (2023). Prakas No. B7-023-003 on Digital Currency Operations.
- Financial Action Task Force. (2023). Updated Guidance for a Risk-Based Approach to Virtual Assets.
- Selendra. (2025). Selendra: A Substrate-Based Blockchain for Cambodia's Digital Infrastructure.
- World Bank. (2024). Remittance Prices Worldwide.
- International Monetary Fund. (2023). Cambodia: Financial System Stability Assessment.
- Asian Development Bank. (2023). MSME Finance Gap Assessment: Cambodia.
- Ministry of Commerce, Cambodia. (2024). Annual Trade Statistics Report.
- National Bank of Cambodia. (2020). Bakong: The Next-Generation Payment System.
- CoinMarketCap. (2024). Stablecoin Market Capitalization and Trading Volume.
- Nakamoto, S. (2008). Bitcoin: A Peer-to-Peer Electronic Cash System.
