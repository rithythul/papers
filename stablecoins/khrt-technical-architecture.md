# KHRt Technical Architecture Implementation

## Overview

This document provides detailed technical specifications for the KHRt blockchain architecture, implementing the independent multi-chain deployment model described in the KHRt whitepaper v1.4. The architecture eliminates cross-chain bridge vulnerabilities through separate contract deployments while maintaining unified reserve backing and complementing Cambodia's existing payment infrastructure.

**Current Payment Landscape Context:**

Cambodia has achieved remarkable success in digital payments:
- **Bakong System**: 30M wallets, $104.81B annual volume (330% of GDP), instant domestic transfers
- **International Integration**: Alipay, WeChat Pay, UnionPay connectivity with 200%+ growth
- **Merchant Adoption**: 4.5M merchants accepting KHQR payments
- **Cross-Border Corridors**: Direct connections with Thailand, Laos, Vietnam, and China

**KHRt's Complementary Technical Role:**

KHRt adds programmable blockchain capabilities that existing infrastructure doesn't provide:
- Smart contract automation for business processes
- 24/7 operations independent of banking infrastructure
- DeFi integration while maintaining KHR exposure
- Global blockchain ecosystem connectivity
- Programmable compliance and automated reporting

The technical architecture is designed to **enhance** rather than compete with existing payment systems, providing integration capabilities with Baray for fiat on/off ramps and compatibility with international payment solutions.

**About Baray:**
Baray is an Integrated Payment Environment. Baray simplifies banking connection and bridges finance to blockchain.

## 1. Core Smart Contract Architecture

### 1.1 Integration Context

KHRt operates within Cambodia's successful payment ecosystem:

**Existing Infrastructure Excellence:**
- Bakong: 30M wallets, $104.81B annual volume (330% of GDP)
- International integration: Alipay, WeChat Pay, UnionPay
- Instant domestic transfers and regional connectivity
- 4.5M merchants with KHQR acceptance

**KHRt's Complementary Role:**
- Programmable payment automation beyond simple transfers
- 24/7 blockchain operations independent of banking hours
- Smart contract capabilities for business process automation
- DeFi integration while maintaining KHR exposure
- Global blockchain ecosystem connectivity

### 1.2 Contract Interface Specifications

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";
import "@openzeppelin/contracts-upgradeable/security/PausableUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

interface IKHRtCore {
    // Events
    event Mint(address indexed to, uint256 amount, bytes32 indexed proofHash);
    event Burn(address indexed from, uint256 amount, bytes32 indexed requestId);
    event ReserveProofUpdated(bytes32 indexed merkleRoot, uint256 timestamp, uint256 totalReserves);
    event EmergencyPause(address indexed operator, string reason);
    event MultiChainSupplyUpdated(uint256 indexed chainId, uint256 supply, uint256 totalAcrossChains);
    
    // Core Functions
    function mint(address to, uint256 amount, bytes32 proofHash, uint256 deadline, bytes calldata signature) external;
    function burn(uint256 amount, bytes32 requestId) external;
    function emergencyPause(string calldata reason) external;
    
    // Reserve Management
    function updateReserveProof(bytes32 merkleRoot, uint256 totalReserves, bytes[] calldata signatures) external;
    function verifyReserveProof(uint256 amount, bytes32[] calldata proof) external view returns (bool);
    
    // Multi-Chain Coordination
    function updateGlobalSupply(uint256[] calldata chainIds, uint256[] calldata supplies, bytes[] calldata signatures) external;
    function getGlobalSupply() external view returns (uint256);
    function verifyGlobalReserveRatio() external view returns (bool);
    
    // Integration Functions for Baray Banking APIs
    function requestBarayMint(bytes32 requestId, uint256 amount, address recipient, uint256 chainId) external;
    function confirmBarayDeposit(bytes32 requestId, bytes calldata bankingProof) external;
    function requestBarayBurn(bytes32 requestId, uint256 amount, string calldata bankAccount) external;
}

contract KHRtToken is IKHRtCore, ERC20Upgradeable, PausableUpgradeable, AccessControlUpgradeable {
    bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
    bytes32 public constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
    bytes32 public constant ORACLE_ROLE = keccak256("ORACLE_ROLE");
    bytes32 public constant COORDINATOR_ROLE = keccak256("COORDINATOR_ROLE");
    bytes32 public constant BARAY_ROLE = keccak256("BARAY_ROLE");
    
    // Reserve Management
    bytes32 public reserveMerkleRoot;
    uint256 public totalReserves;
    uint256 public lastReserveUpdate;
    uint256 public constant RESERVE_UPDATE_FREQUENCY = 1 weeks;
    uint256 public constant MIN_RESERVE_RATIO = 105; // 105% - includes 5% buffer
    
    // Multi-Chain Coordination
    uint256 public chainId;
    mapping(uint256 => uint256) public chainSupply; // chainId => current supply
    uint256 public globalSupply; // Total supply across all chains
    uint256 public lastGlobalUpdate;
    
    // Integration with Baray Banking APIs
    mapping(bytes32 => BarayRequest) public barayRequests;
    bool public barayIntegrationEnabled;
    address public barayOracle;
    
    struct BarayRequest {
        address user;
        uint256 amount;
        uint256 targetChainId;
        string bankAccount; // For burn requests
        uint256 timestamp;
        RequestType requestType;
        bool confirmed;
        bool processed;
    }
    
    enum RequestType {
        MINT_KHR,
        MINT_USD,
        BURN_TO_KHR,
        BURN_TO_USD
    }
    
    // Circuit Breakers
    uint256 public maxDailyMint;
    uint256 public maxDailyBurn;
    uint256 public dailyMintedAmount;
    uint256 public dailyBurnedAmount;
    uint256 public lastResetDay;
    
    modifier onlyValidReserves(uint256 amount) {
        require(
            (globalSupply + amount) * MIN_RESERVE_RATIO <= totalReserves * 100,
            "Insufficient global reserves"
        );
        _;
    }
    
    modifier dailyLimitCheck(uint256 amount, bool isMint) {
        if (block.timestamp / 1 days > lastResetDay) {
            dailyMintedAmount = 0;
            dailyBurnedAmount = 0;
            lastResetDay = block.timestamp / 1 days;
        }
        
        if (isMint) {
            require(dailyMintedAmount + amount <= maxDailyMint, "Daily mint limit exceeded");
            dailyMintedAmount += amount;
        } else {
            require(dailyBurnedAmount + amount <= maxDailyBurn, "Daily burn limit exceeded");
            dailyBurnedAmount += amount;
        }
        _;
    }
    
    // Integration with Baray Banking APIs
    function requestBarayMint(
        bytes32 requestId,
        uint256 amount,
        address recipient,
        uint256 targetChainId
    ) external override onlyRole(BARAY_ROLE) {
        require(barayIntegrationEnabled, "Baray integration not enabled");
        require(barayRequests[requestId].user == address(0), "Request already exists");
        
        barayRequests[requestId] = BarayRequest({
            user: recipient,
            amount: amount,
            targetChainId: targetChainId,
            bankAccount: "",
            timestamp: block.timestamp,
            requestType: RequestType.MINT_KHR, // Default, can be updated
            confirmed: false,
            processed: false
        });
        
        emit BarayMintRequested(requestId, recipient, amount, targetChainId);
    }
    
    function confirmBarayDeposit(
        bytes32 requestId,
        bytes calldata bankingProof
    ) external override {
        require(msg.sender == barayOracle, "Only Baray oracle");
        BarayRequest storage request = barayRequests[requestId];
        require(request.user != address(0), "Request not found");
        require(!request.confirmed, "Already confirmed");
        
        // Verify proof of banking deposit through Baray's aggregated APIs
        require(verifyBarayBankingProof(requestId, request.amount, bankingProof), "Invalid banking proof");
        
        request.confirmed = true;
        
        // Mint KHRt tokens on the requested chain
        if (request.targetChainId == chainId) {
            _mint(request.user, request.amount);
            globalSupply += request.amount;
            chainSupply[chainId] = totalSupply();
        } else {
            // Signal cross-chain coordinator for minting on target chain
            emit CrossChainMintSignal(request.targetChainId, request.user, request.amount, requestId);
        }
        
        request.processed = true;
        
        emit BarayDepositConfirmed(requestId, request.user, request.amount, request.targetChainId);
    }
    
    function requestBarayBurn(
        bytes32 requestId,
        uint256 amount,
        string calldata bankAccount
    ) external override {
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");
        require(barayRequests[requestId].user == address(0), "Request already exists");
        
        barayRequests[requestId] = BarayRequest({
            user: msg.sender,
            amount: amount,
            targetChainId: chainId,
            bankAccount: bankAccount,
            timestamp: block.timestamp,
            requestType: RequestType.BURN_TO_KHR,
            confirmed: false,
            processed: false
        });
        
        // Burn tokens immediately
        _burn(msg.sender, amount);
        globalSupply -= amount;
        chainSupply[chainId] = totalSupply();
        
        emit BarayBurnRequested(requestId, msg.sender, amount, bankAccount);
    }
    
    function mint(
        address to,
        uint256 amount,
        bytes32 proofHash,
        uint256 deadline,
        bytes calldata signature
    ) external override onlyRole(MINTER_ROLE) onlyValidReserves(amount) dailyLimitCheck(amount, true) {
        require(block.timestamp <= deadline, "Mint deadline expired");
        require(verifyMintSignature(to, amount, proofHash, deadline, signature), "Invalid signature");
        
        _mint(to, amount);
        
        // Update global supply tracking
        globalSupply += amount;
        chainSupply[chainId] = totalSupply();
        
        emit Mint(to, amount, proofHash);
        emit MultiChainSupplyUpdated(chainId, totalSupply(), globalSupply);
    }
    
    function burn(uint256 amount, bytes32 requestId) external override dailyLimitCheck(amount, false) {
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");
        
        _burn(msg.sender, amount);
        
        // Update global supply tracking
        globalSupply -= amount;
        chainSupply[chainId] = totalSupply();
        
        emit Burn(msg.sender, amount, requestId);
        emit MultiChainSupplyUpdated(chainId, totalSupply(), globalSupply);
    }
    
    // Events for Baray integration
    event BarayMintRequested(bytes32 indexed requestId, address indexed user, uint256 amount, uint256 targetChainId);
    event BarayDepositConfirmed(bytes32 indexed requestId, address indexed user, uint256 amount, uint256 targetChainId);
    event BarayBurnRequested(bytes32 indexed requestId, address indexed user, uint256 amount, string bankAccount);
    event CrossChainMintSignal(uint256 indexed targetChainId, address indexed user, uint256 amount, bytes32 requestId);
}
```

## 2. Multi-Chain Deployment Architecture

### 2.1 Independent Contract Deployment Manager

```solidity
// Contract factory for deploying KHRt on new networks
contract KHRtDeploymentFactory {
    bytes32 public constant DEPLOYER_ROLE = keccak256("DEPLOYER_ROLE");
    
    struct DeploymentConfig {
        uint256 chainId;
        string networkName;
        uint256 maxDailyMint;
        uint256 maxDailyBurn;
        address coordinator;
        address[] oracles;
        bool barayIntegrationEnabled;
        address barayOracle;
    }
    
    mapping(uint256 => address) public deployments;
    mapping(uint256 => DeploymentConfig) public configs;
    
    event ContractDeployed(uint256 indexed chainId, address contractAddress, string networkName);
    
    function deployToNetwork(
        DeploymentConfig calldata config,
        bytes calldata initData
    ) external onlyRole(DEPLOYER_ROLE) returns (address) {
        require(deployments[config.chainId] == address(0), "Already deployed to this chain");
        
        // Deploy new KHRt contract
        address newContract = Clones.cloneDeterministic(
            implementation,
            keccak256(abi.encode(config.chainId))
        );
        
        // Initialize the contract with integration capabilities
        IKHRtToken(newContract).initialize(
            string(abi.encodePacked("KHRt-", config.networkName)),
            "KHRt",
            config.maxDailyMint,
            config.maxDailyBurn,
            config.coordinator,
            config.oracles,
            config.barayIntegrationEnabled,
            config.barayOracle
        );
        
        deployments[config.chainId] = newContract;
        configs[config.chainId] = config;
        
        emit ContractDeployed(config.chainId, newContract, config.networkName);
        return newContract;
    }
}
```

### 2.2 Integration Bridge to Existing Infrastructure

```solidity
// Bridge contract for integration with existing payment systems
contract KHRtIntegrationBridge {
    bytes32 public constant BRIDGE_OPERATOR_ROLE = keccak256("BRIDGE_OPERATOR_ROLE");
    bytes32 public constant BARAY_ORACLE_ROLE = keccak256("BARAY_ORACLE_ROLE");
    
    struct IntegrationRequest {
        address user;
        uint256 amount;
        string externalTxId; // Baray transaction ID
        uint256 timestamp;
        IntegrationType integrationType;
        bool processed;
    }
    
    enum IntegrationType {
        BARAY_DEPOSIT,
        BARAY_WITHDRAWAL,
        INTERNATIONAL_WALLET,
        BANK_TRANSFER
    }
    
    mapping(bytes32 => IntegrationRequest) public integrationRequests;
    mapping(string => bool) public processedExternalTxs;
    
    // Integration with Baray system
    function processBarayDeposit(
        string calldata barayTxId,
        address recipient,
        uint256 amount,
        bytes calldata barayProof
    ) external onlyRole(BARAY_ORACLE_ROLE) {
        require(!processedExternalTxs[barayTxId], "Transaction already processed");
        require(verifyBarayTransaction(barayTxId, amount, barayProof), "Invalid Baray proof");
        
        bytes32 requestId = keccak256(abi.encode(barayTxId, recipient, amount));
        
        integrationRequests[requestId] = IntegrationRequest({
            user: recipient,
            amount: amount,
            externalTxId: barayTxId,
            timestamp: block.timestamp,
            integrationType: IntegrationType.BARAY_DEPOSIT,
            processed: false
        });
        
        processedExternalTxs[barayTxId] = true;
        
        // Mint KHRt tokens
        IKHRtToken(khrtContract).mint(recipient, amount, keccak256(abi.encode(barayTxId)), block.timestamp + 1 hours, "");
        
        integrationRequests[requestId].processed = true;
        
        emit BarayDepositProcessed(requestId, recipient, amount, barayTxId);
    }
    
    function processBarayWithdrawal(
        bytes32 requestId,
        string calldata barayTxId
    ) external onlyRole(BARAY_ORACLE_ROLE) {
        IntegrationRequest storage request = integrationRequests[requestId];
        require(request.user != address(0), "Request not found");
        require(request.integrationType == IntegrationType.BARAY_WITHDRAWAL, "Invalid request type");
        require(!request.processed, "Already processed");
        
        request.externalTxId = barayTxId;
        request.processed = true;
        
        emit BarayWithdrawalProcessed(requestId, request.user, request.amount, barayTxId);
    }
    
    // Events
    event BarayDepositProcessed(bytes32 indexed requestId, address indexed user, uint256 amount, string barayTxId);
    event BarayWithdrawalProcessed(bytes32 indexed requestId, address indexed user, uint256 amount, string barayTxId);
}
```

## 3. Enhanced API Layer for Ecosystem Integration

### 3.1 Enhanced API Layer for Ecosystem Integration

```typescript
// Enhanced API for integration with existing payment systems and Bitriel wallet
interface EcosystemIntegratedKHRtAPI {
  // Baray integration for banking APIs
  getBarayIntegrationStatus(): Promise<BarayIntegrationStatus>;
  requestBarayOnRamp(amount: string, currency: 'KHR' | 'USD', userWallet: string): Promise<BarayOnRampResponse>;
  requestBarayOffRamp(amount: string, currency: 'KHR' | 'USD', bankAccount: string): Promise<BarayOffRampResponse>;
  
  // Wallet integration (optimized for Bitriel)
  getBitrielCompatibility(): Promise<BitrielIntegrationStatus>;
  initiateBitrielMinting(request: MintRequest): Promise<MintResponse>;
  
  // Multi-chain operations with clear fee structure
  mint(request: MintRequest, chainId?: number): Promise<MintResponse>;
  burn(request: BurnRequest, chainId?: number): Promise<BurnResponse>;
  estimateFees(operation: 'mint' | 'burn' | 'transfer', chainId: number): Promise<FeeEstimate>;
  
  // Cross-ecosystem balance queries
  getTotalBalance(address: string): Promise<EcosystemBalanceResponse>;
  getOptimalPaymentMethod(amount: string, recipient: string): Promise<PaymentRecommendation[]>;
}

interface BarayIntegrationStatus {
  enabled: boolean;
  supportedCurrencies: ['KHR', 'USD'];
  avgProcessingTime: string; // "15 minutes"
  bankingPartners: string[];
  dailyLimits: {
    individual: string;
    business: string;
  };
}

interface BitrielIntegrationStatus {
  available: boolean;
  features: {
    nativeMinting: boolean;
    crossChainSupport: boolean;
    gasFreeSelendra: boolean;
  };
  supportedNetworks: string[];
}

interface FeeEstimate {
  networkFee: string;
  programmableFee?: string; // 0.1-0.5% for smart contract features
  bankingFee: string; // "Included"
  total: string;
  currency: 'KHR' | 'USD';
}
```

### 3.2 Smart Payment Router

```typescript
// Intelligent payment routing considering all available options
class SmartPaymentRouter {
  private barayAPI: BarayAPI;
  private khrtAPI: KHRtAPI;
  private internationalWallets: InternationalWalletAPI[];
  
  constructor(config: PaymentRouterConfig) {
    this.barayAPI = new BarayAPI(config.baray);
    this.khrtAPI = new KHRtAPI(config.khrt);
    this.internationalWallets = config.international.map(cfg => new InternationalWalletAPI(cfg));
  }
  
  // Recommend best payment method based on context
  async recommendPaymentMethod(request: PaymentRequest): Promise<PaymentRecommendation[]> {
    const amount = parseFloat(request.amount);
    const isInternational = request.isInternational;
    const urgency = request.urgency; // immediate, standard, scheduled
    const features = request.requiredFeatures; // ['programmable', 'automation', 'simple-transfer']
    
    const recommendations: PaymentRecommendation[] = [];
    
    // Domestic instant transfers - Baray is optimal
    if (!isInternational && urgency === 'immediate' && !features.includes('programmable')) {
      recommendations.push({
        method: 'baray',
        reason: 'Instant domestic transfer with zero fees',
        estimatedFee: '0 KHR',
        estimatedTime: 'Instant',
        suitability: 'best'
      });
    }
    
    // International tourist/e-commerce - existing solutions are optimal
    if (isInternational && features.includes('tourism')) {
      recommendations.push({
        method: 'international-wallet',
        reason: 'Tourist-optimized with merchant acceptance',
        estimatedFee: '1-3% depending on provider',
        estimatedTime: 'Instant',
        suitability: 'best'
      });
    }
    
    // Programmable features required - KHRt is optimal
    if (features.includes('programmable') || features.includes('automation')) {
      recommendations.push({
        method: 'khrt',
        reason: 'Smart contracts and automation capabilities',
        estimatedFee: '0.1-0.5% + gas fees',
        estimatedTime: '15 minutes for minting, instant for transfers',
        suitability: 'best'
      });
    }
    
    // 24/7 operations beyond banking hours
    if (features.includes('24/7') && urgency === 'immediate') {
      const currentTime = new Date();
      const isBankingHours = this.isBankingHours(currentTime);
      
      if (!isBankingHours) {
        recommendations.push({
          method: 'khrt',
          reason: '24/7 blockchain operations independent of banking hours',
          estimatedFee: '0.1-0.5% + gas fees',
          estimatedTime: 'Immediate for existing tokens',
          suitability: 'best'
        });
      }
    }
    
    // DeFi and yield generation
    if (features.includes('defi')) {
      recommendations.push({
        method: 'khrt',
        reason: 'Only solution enabling DeFi participation while maintaining KHR exposure',
        estimatedFee: '0.1-0.5% + DeFi protocol fees',
        estimatedTime: 'Varies by protocol',
        suitability: 'best'
      });
    }
    
    return recommendations.sort((a, b) => a.suitability === 'best' ? -1 : 1);
  }
  
  // Execute payment using optimal method
  async executeOptimalPayment(request: PaymentRequest): Promise<PaymentResult> {
    const recommendations = await this.recommendPaymentMethod(request);
    const bestOption = recommendations[0];
    
    switch (bestOption.method) {
      case 'baray':
        return await this.executeBarayPayment(request);
      case 'khrt':
        return await this.executeKHRtPayment(request);
      case 'international-wallet':
        return await this.executeInternationalPayment(request);
      default:
        throw new Error('No suitable payment method found');
    }
  }
  
  private async executeBarayPayment(request: PaymentRequest): Promise<PaymentResult> {
    // Direct integration with Baray for simple transfers
    return await this.barayAPI.transfer({
      amount: request.amount,
      recipient: request.recipient,
      memo: request.memo
    });
  }
  
  private async executeKHRtPayment(request: PaymentRequest): Promise<PaymentResult> {
    // Use KHRt for programmable features
    if (request.requiredFeatures.includes('programmable')) {
      return await this.khrtAPI.executeSmartContract({
        contract: request.smartContract,
        amount: request.amount,
        parameters: request.parameters
      });
    } else {
      return await this.khrtAPI.transfer({
        amount: request.amount,
        recipient: request.recipient,
        chainId: await this.khrtAPI.getOptimalChain(request.amount)
      });
    }
  }
  
  private isBankingHours(time: Date): boolean {
    const hour = time.getHours();
    const day = time.getDay();
    
    // Assuming banking hours: 8 AM - 4 PM, Monday-Friday
    return day >= 1 && day <= 5 && hour >= 8 && hour < 16;
  }
}
```

## 4. Ecosystem Integration Monitoring

### 4.1 Comprehensive Ecosystem Metrics

```solidity
// Monitoring contract for ecosystem integration health
contract EcosystemIntegrationMonitor {
    struct EcosystemMetrics {
        // KHRt specific metrics
        uint256 totalKHRtSupply;
        uint256 dailyKHRtVolume;
        uint256 uniqueKHRtUsers;
        
        // Integration metrics
        uint256 barayIntegrationVolume;
        uint256 internationalWalletVolume;
        uint256 crossPlatformTransactions;
        
        // Comparative metrics
        uint256 programmablePaymentRatio; // % of payments using smart contracts
        uint256 ecosystemComplementarity; // Measure of how well systems work together
        uint256 userSatisfactionIndex;
    }
    
    mapping(uint256 => EcosystemMetrics) public dailyMetrics; // timestamp => metrics
    
    // Integration health monitoring
    function updateEcosystemMetrics(
        uint256 date,
        EcosystemMetrics calldata metrics
    ) external onlyRole(METRICS_ORACLE_ROLE) {
        dailyMetrics[date] = metrics;
        
        // Analyze ecosystem health
        analyzeEcosystemHealth(metrics);
        
        emit EcosystemMetricsUpdated(date, metrics);
    }
    
    function analyzeEcosystemHealth(EcosystemMetrics memory metrics) internal {
        // Check for healthy complementarity
        uint256 totalVolume = metrics.dailyKHRtVolume + metrics.barayIntegrationVolume + metrics.internationalWalletVolume;
        
        if (totalVolume > 0) {
            uint256 khrtRatio = (metrics.dailyKHRtVolume * 100) / totalVolume;
            
            // KHRt should complement, not dominate simple transfer use cases
            if (khrtRatio > 30) {
                emit EcosystemAlert("KHRt usage ratio high - check if cannibalizing simple transfers", khrtRatio);
            }
            
            // Programmable features should be primary KHRt use case
            if (metrics.programmablePaymentRatio < 60) {
                emit EcosystemAlert("Low programmable payment ratio - need better feature adoption", metrics.programmablePaymentRatio);
            }
        }
    }
    
    event EcosystemMetricsUpdated(uint256 indexed date, EcosystemMetrics metrics);
    event EcosystemAlert(string message, uint256 value);
}
```

### 4.2 Integration Success Metrics

```typescript
// Analytics for measuring ecosystem integration success
interface EcosystemAnalytics {
  // User behavior analysis
  getUserPaymentPreferences(userId: string): Promise<PaymentPreferences>;
  getEcosystemUsagePatterns(): Promise<UsagePattern[]>;
  
  // Integration effectiveness
  getComplementarityIndex(): Promise<number>; // 0-100 scale
  getFeatureAdoptionRates(): Promise<FeatureAdoption[]>;
  
  // Ecosystem health
  getEcosystemHealth(): Promise<EcosystemHealth>;
  getIntegrationSuccessMetrics(): Promise<IntegrationMetrics>;
}

interface PaymentPreferences {
  preferredMethods: {
    domesticTransfers: 'baray' | 'khrt' | 'mixed';
    internationalPayments: 'international-wallets' | 'khrt' | 'mixed';
    automatedPayments: 'khrt' | 'traditional';
    dfiParticipation: 'khrt' | 'none';
  };
  reasons: string[];
  satisfactionScores: {
    [method: string]: number; // 1-10 scale
  };
}

interface UsagePattern {
  timeOfDay: string;
  paymentMethod: string;
  averageAmount: string;
  useCase: string;
  frequency: 'daily' | 'weekly' | 'monthly' | 'occasional';
}

interface EcosystemHealth {
  overallScore: number; // 0-100
  complementarityIndex: number; // How well systems work together
  competitionIndex: number; // How much systems compete vs complement
  userSatisfaction: number;
  technicalReliability: number;
  recommendations: string[];
}

interface IntegrationMetrics {
  barayIntegration: {
    enabled: boolean;
    volume24h: string;
    averageProcessingTime: string;
    successRate: number;
  };
  internationalWalletIntegration: {
    supportedWallets: number;
    volume24h: string;
    userSatisfaction: number;
  };
  crossPlatformTransactions: {
    count24h: number;
    averageValue: string;
    popularFlows: string[];
  };
}
```

This enhanced technical architecture provides:

1. **Ecosystem Integration** - Direct integration capabilities with Baray and international wallets
2. **Smart Payment Routing** - Intelligent recommendation of optimal payment methods based on use case
3. **Complementary Positioning** - Technical framework designed to enhance rather than compete with existing solutions
4. **Integration Monitoring** - Comprehensive analytics to ensure healthy ecosystem development
5. **Future-Ready Architecture** - Extensible design for additional payment system integrations

The architecture recognizes Cambodia's payment ecosystem success and positions KHRt as an additive layer for programmable money capabilities while maintaining seamless integration with existing infrastructure. 