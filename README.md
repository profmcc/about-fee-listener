# ShapeShift Fee Listener System

## 🎯 Overview

The ShapeShift Fee Listener System is a comprehensive multi-protocol, multi-chain infrastructure designed to track, monitor, and analyze affiliate fee transactions across the entire ShapeShift ecosystem. This system provides real-time visibility into revenue streams, enabling the ShapeShift DAO to make informed decisions about treasury management and rFOX token distribution.

## 🏗️ System Architecture

### Core Components

1. **Protocol Listeners**: Real-time blockchain event listeners for each supported protocol
2. **Fee Aggregators**: Collectors that consolidate fees across multiple chains
3. **Treasury Safes**: Multi-signature wallets that securely hold collected fees
4. **Analytics Engine**: Data processing and reporting system for business intelligence

## 💰 Fee Collection Structure

### Revenue Streams

- **Affiliate Fees**: Percentage-based fees from protocol integrations
- **Swap Fees**: Transaction fees from DEX operations
- **Bridge Fees**: Cross-chain transfer fees
- **Liquidity Fees**: LP position management fees

### Fee Distribution

```
Total Fees Collected
├── 75% → ShapeShift DAO Treasury
└── 25% → rFOX Stakers (Monthly Distribution)
```

## 🏦 Treasury Management

### Multi-Chain Treasury Safes

The system maintains dedicated treasury safes across multiple blockchain networks:

#### **Ethereum Mainnet**
- **Address**: `0x90A48D5CF7343B08dA12E067680B4C6dbfE551Be`
- **Purpose**: Primary treasury for Ethereum-based protocols
- **Protocols**: CoW Swap, Portals, Relay, 0x

#### **Base Chain**
- **Address**: `0x9c9aA90363630d4ab1D9dbF416cc3BBC8d3Ed502`
- **Purpose**: Treasury for Base-based protocols
- **Protocols**: Relay

#### **Butterswap Chain**
- **Address**: `0x35339070f178dC4119732982C23F5a8d88D3f8a3`
- **Protocols**: Butterswap

#### **Additional Chains**
- **Optimism**: `0x6268d07327f4fb7380732dc6d63d95F88c0E083b`
- **Avalanche**: `0x74d63F31C2335b5b3BA7ad2812357672b2624cEd`
- **Polygon**: `0xB5F944600785724e31Edb90F9DFa16dBF01Af000`
- **BSC**: `0x8b92b1698b57bEDF2142297e9397875ADBb2297E`
- **Arbitrum**: `0x38276553F8fbf2A027D901F8be45f00373d8Dd48`
- **Solana**: `Bh7R3MeJ98D7Ersxh7TgVQVQUSmDMqwrFVHH9DLfb4u3`

### Multi-Signature Security

**Base Chain Treasury**: 2-of-3 multisig configuration
- **Owner 1**: `0x35339070f178dC4119732982C23F5a8d88D3f8a3`
- **Owner 2**: `0x47f11EB6ab5B41857EA8616aDeAf205248b5C55c`
- **Owner 3**: `0xF5AA59151bE6515C4Ca68A0282CF68B3eA4846fC`
- **Threshold**: 2 signatures required for any transaction

## 🔍 Fee Detection & Monitoring

### Event Types Monitored

1. **Affiliate Fee Events**
   - `AffiliateFeeCollected` events from protocol contracts
   - Real-time fee amount tracking
   - Source protocol identification

2. **Treasury Balance Changes**
   - Safe balance monitoring
   - Incoming/outgoing transaction tracking
   - Multi-chain balance aggregation

3. **rFOX Distribution Events**
   - Monthly distribution triggers
   - Staking contract interactions
   - Distribution amount calculations

### Monitoring Frequency

- **Fee Collection**: Real-time (block-by-block)
- **Balance Updates**: Every block (ideally/ but daily is sufficient)
- **Aggregation**: Daily and monthly
- **Reporting**: Daily summaries + monthly analytics

## 📊 Data Collection & Storage

### Storage Strategy

- **Primary**: CSV files for historical data
- **Real-time**: In-memory processing for live monitoring
- **Backup**: Database snapshots for critical data
- **Analytics**: Aggregated metrics for business intelligence

### Data Points Collected

- Transaction hash and block number
- Fee amount and token type
- Source protocol and chain
- Affiliate address involved
- Treasury safe destination
- Timestamp and gas costs

## 🔧 Technical Implementation

### Technology Stack

- **Blockchain Interaction**: Web3.py, ethers.js
- **Event Processing**: Custom event listeners
- **Data Storage**: CSV, SQLite, JSON
- **Monitoring**: Custom alerting system
- **Analytics**: Pandas, NumPy for data processing

### Key Features

- **Multi-Provider RPC**: Fallback support for reliability
- **Rate Limiting**: Intelligent request throttling
- **Error Handling**: Graceful degradation and retry logic
- **Block Tracking**: Persistent checkpointing
- **Cross-Chain Support**: Unified interface for multiple chains

## 📈 Business Intelligence

### Key Metrics

1. **Revenue Performance**
   - Daily/monthly fee collection totals
   - Protocol-specific revenue breakdown
   - Chain-specific performance metrics

2. **Protocol Analytics**
   - Usage patterns and trends
   - Fee optimization opportunities
   - Integration performance metrics

### Reporting

- **Daily Reports**: Fee collection summaries
- **Monthly Reports**: Comprehensive treasury analysis
- **Quarterly Reviews**: Strategic insights and recommendations
- **Annual Planning**: Revenue projections and budget planning

## 📚 Development Journal: What Was Tried & Why It Didn't Always Work

### **Phase 1: Monolithic Approach (Initial Attempt)**
**What We Built:**
- Single Python service handling all protocols
- Shared event parsing logic across CoW Swap, Relay, Portals
- Unified database schema for all affiliate transactions

**Why It Failed:**
- **Brittle Architecture**: One protocol failure would crash the entire system
- **Debugging Nightmare**: Impossible to isolate issues between protocols
- **Memory Issues**: Large block scans would consume excessive RAM
- **Single Point of Failure**: RPC rate limits would block all protocols simultaneously

**Specific Problems:**
```
Error: 'bytes' object has no attribute 'encode'
Cause: Web3.py compatibility issues when parsing different event formats
Impact: Complete system failure, lost transactions
```

### **Phase 2: Modular Repository (Intermediate Solution)**
**What We Built:**
- Separate directories per protocol with shared utilities
- Common ABI parsing and event detection
- Shared CSV storage and block tracking

**Why It Still Had Issues:**
- **Shared Dependencies**: Version conflicts between protocol requirements
- **Coupled Deployments**: Couldn't update one protocol without affecting others
- **Resource Contention**: Multiple protocols competing for RPC connections
- **Complex Error Handling**: Shared error handling made debugging difficult

**Specific Problems:**
```
Error: Rate limit exceeded on Infura
Cause: Multiple protocols hitting RPC endpoints simultaneously
Impact: Missed blocks, incomplete transaction data
```

### **Phase 3: Standalone Services (Current Working Solution)**
**What We Built:**
- Independent services for each protocol
- Protocol-specific configurations and error handling
- Separate deployment and scaling capabilities

**Why This Works Better:**
- **Isolated Failures**: One protocol can fail without affecting others
- **Independent Scaling**: Each protocol can be scaled based on its needs
- **Easier Debugging**: Issues can be isolated to specific protocols
- **Flexible Deployment**: Can update protocols independently

### **Protocol-Specific Challenges & Solutions**

#### **CoW Swap - The Success Story**
**Initial Problems:**
- Complex event structure with nested affiliate data
- Multiple affiliate addresses across different chains
- High transaction volume requiring efficient filtering

**Solutions That Worked:**
- Custom ABI parsing for affiliate fee events
- Chain-specific affiliate address filtering
- Block chunking to handle high volume
- Persistent CSV-based block tracking

**Why It Succeeded:**
- Well-documented event structure
- Consistent affiliate fee emission patterns
- Active development community with clear examples

#### **Relay - The Persistent Challenge**
**Initial Problems:**
- `'bytes' object has no attribute 'encode'` errors
- Inconsistent event emission patterns
- Complex cross-chain fee routing

**What We Tried:**
1. **Standard Web3.py parsing** → Failed with bytes encoding errors
2. **Custom event decoder** → Worked but was fragile
3. **Multiple ABI versions** → Inconsistent results
4. **Event signature filtering** → Missed some transactions

**Why It's Still Challenging:**
- Web3.py compatibility issues with specific event formats
- Events emitted differently across chains (Ethereum vs Base)
- Complex fee routing logic that's hard to track

**Current Status:**
- ✅ **Working**: Basic event detection and parsing
- ⚠️ **Issues**: Occasional parsing errors, missed transactions
- 🔧 **Next Steps**: Implement more robust error handling

#### **Portals - The Lucky One**
**Why It Works Well:**
- Leverages CoW Swap's proven listener design
- Consistent event structure
- Lower transaction volume

**What We Learned:**
- Reusing working patterns is more effective than reinventing
- Protocol integration can simplify monitoring
- Shared infrastructure reduces maintenance overhead

#### **ButterSwap - The Quiet One**
**Initial Problems:**
- No affiliate activity detected
- clear that the protocol is actually generating fees--we just can't see them on-chain
- Base chain RPC limitations

**What We Tried:**
1. **Direct contract monitoring** → No events found
2. **Transaction scanning** → No affiliate patterns detected
3. **Community research** → Limited documentation available

**Current Understanding:**
- Protocol may not be flagging fees in any on-chain/public way.
- Listener is operational but monitoring for future activity
- May need to investigate how affiliate partners are tracked

### **Technical Challenges That Persisted**

#### **1. Event Log Parsing Issues**
**Problem:**
```
'bytes' object has no attribute 'encode'
TypeError: Object of type AttributeDict is not JSON serializable
```

**Root Causes:**
- Web3.py version compatibility issues
- Different blockchain clients emit events differently
- Protocol-specific event structures

**Solutions Tried:**
- **Explicit type conversion** → Worked for some cases
- **Custom JSON encoders** → Complex and error-prone
- **Event filtering** → Reduced errors but missed transactions
- **Multiple parsing strategies** → Increased complexity

#### **2. RPC Provider Limitations**
**Problem:**
- Free-tier rate limits (Infura: 100k requests/day, Alchemy: 300M compute units/month)
- Large block scans would exceed limits
- Provider outages would stop all monitoring

**Solutions Implemented:**
- **Provider rotation** → Reduced single point of failure
- **Block chunking** → Stayed within rate limits
- **Retry backoff** → Handled temporary failures
- **Fallback providers** → Maintained uptime

**Why It Still Causes Issues:**
- Rate limits are still hit during high-volume periods
- Provider rotation adds complexity
- Some chains have limited RPC options

#### **3. Cross-Chain Consistency**
**Problem:**
- Different EVM chains have different quirks
- Event indexing varies between chains
- Gas costs and block times differ significantly

**Solutions Implemented:**
- **Chain-specific configurations** → Handled differences
- **Schema normalization** → Unified data storage
- **Chain-aware error handling** → Better debugging

**Remaining Challenges:**
- Some chains have unreliable RPC endpoints
- Event ordering can differ between chains
- Gas estimation varies significantly

### **Data Storage Evolution**

#### **Phase 1: SQLite Database**
**What We Built:**
- Relational database with normalized tables
- Complex queries for analytics
- Transaction rollback capabilities

**Why It Failed:**
- **Performance Issues**: Slow queries on large datasets
- **Complexity**: Schema changes required migrations
- **File Corruption**: Database files would occasionally corrupt
- **Debugging Difficulty**: Hard to inspect raw data

#### **Phase 2: PostgreSQL**
**What We Built:**
- Production-grade database with proper indexing
- ACID compliance and transaction support
- Advanced querying capabilities

**Why It Failed:**
- **Infrastructure Overhead**: Required database server
- **Connection Management**: Complex connection pooling
- **Schema Evolution**: Migrations were time-consuming
- **Performance**: Still slow for large block scans

#### **Phase 3: CSV-First Approach (Current)**
**What We Built:**
- Simple CSV files for each protocol
- Human-readable data format
- Easy backup and version control
- Simple data inspection and debugging

**Why This Works Better:**
- **Simplicity**: Easy to understand and modify
- **Performance**: Fast read/write operations
- **Debugging**: Can inspect data with any text editor
- **Version Control**: Git tracks all changes
- **Portability**: Easy to move between systems

**Trade-offs:**
- No complex queries or joins
- Limited concurrent access
- No built-in data validation
- Manual backup management

### **Monitoring & Alerting Evolution**

#### **Phase 1: Basic Logging**
**What We Built:**
- Simple console output
- Basic file logging
- Error messages to stdout

**Why It Failed:**
- **No Persistence**: Lost information on restart
- **No Alerts**: Had to manually check logs
- **No Metrics**: Couldn't track performance
- **No Aggregation**: Hard to see patterns

#### **Phase 2: Structured Logging**
**What We Built:**
- JSON-formatted logs
- Log rotation and compression
- Error aggregation

**Why It Still Had Issues:**
- **No Real-time Monitoring**: Had to wait for log analysis
- **No Alerting**: Still manual intervention required
- **No Dashboard**: Hard to visualize system health

#### **Phase 3: Comprehensive Monitoring (Current)**
**What We Built:**
- Real-time block processing metrics
- Error rate tracking
- Performance monitoring
- Automated alerting

**Current Capabilities:**
- **Real-time Metrics**: Block processing rate, error rates
- **Error Tracking**: Categorized by protocol and error type
- **Performance Monitoring**: Response times, throughput
- **Alerting**: Slack notifications for critical issues

### **Lessons Learned & Best Practices**

#### **1. Start Simple, Evolve Gradually**
- **What Worked**: CSV files, simple event parsing, basic error handling
- **What Didn't**: Complex databases, over-engineered architectures, premature optimization

#### **2. Protocol-Specific Solutions Beat Generic Ones**
- **What Worked**: Custom ABIs, protocol-specific error handling, tailored configurations
- **What Didn't**: Universal event parsers, generic error handling, one-size-fits-all configs

#### **3. Monitoring Is Critical**
- **What Worked**: Real-time metrics, error tracking, performance monitoring
- **What Didn't**: Basic logging, manual checking, reactive debugging

#### **4. Data Storage Should Match Use Case**
- **What Worked**: CSV for simple data, human-readable formats, easy debugging
- **What Didn't**: Complex databases for simple data, binary formats, hard-to-inspect storage

#### **5. Error Handling Must Be Robust**
- **What Worked**: Graceful degradation, retry logic, fallback mechanisms
- **What Didn't**: Fail-fast approaches, no retry logic, single point of failure

### **Current Status & Next Steps**

#### **What's Working Well:**
- ✅ **CoW Swap**: 1000+ block scans, reliable fee detection
- ✅ **Portals**: Leveraging CoW Swap's proven approach
- ✅ **Data Storage**: CSV-based system with good performance
- ✅ **Monitoring**: Real-time metrics and alerting
- ✅ **Error Handling**: Robust retry and fallback mechanisms

#### **What Still Needs Work:**
- ⚠️ **Relay**: Parsing errors, missed transactions
- ⚠️ **ButterSwap**: No affiliate activity detected
- ⚠️ **Cross-Chain**: Some chains have reliability issues
- ⚠️ **Performance**: Large block scans can be slow

#### **Next Development Priorities:**
1. **Fix Relay Parsing**: Implement more robust event parsing
2. **Improve Performance**: Optimize block scanning algorithms
3. **Enhanced Monitoring**: Better visualization and alerting
4. **Protocol Expansion**: Add support for new protocols
5. **Data Analytics**: Better insights and reporting

This development journal shows the evolution from a simple, failing monolithic system to a robust, scalable microservices architecture. The key insight is that **simplicity and protocol-specific solutions consistently outperformed complex, generic approaches**.

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- Web3.py library
- Access to blockchain RPC endpoints
- Treasury safe addresses and configurations

### Quick Start

```bash
# Clone the repository
git clone https://github.com/profmcc/shapeshift-affiliate-tracker.git
cd shapeshift-affiliate-tracker

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp env.example .env
# Add your RPC endpoints and API keys

# Start fee listening
python run_fee_listener.py --protocols all --chains all
```

### Configuration

1. **Environment Variables**: Set RPC endpoints and API keys
2. **Treasury Addresses**: Configure safe addresses in `treasury_safes.yaml`
3. **Protocol Settings**: Adjust listener parameters per protocol
4. **Alert Configuration**: Set up notification preferences

## 📚 Documentation

### Additional Resources

- **API Documentation**: REST API endpoints and usage
- **Configuration Guide**: Detailed setup instructions
- **Troubleshooting**: Common issues and solutions
- **Development Guide**: Contributing to the system

### Support

- **Technical Issues**: GitHub Issues repository
- **Business Questions**: ShapeShift DAO team
- **Emergency Contacts**: Treasury team direct line

## 🔮 Future Enhancements

### Planned Features

1. **Advanced Analytics**
   - Machine learning for fee prediction
   - Automated optimization recommendations
   - Real-time market analysis

2. **Integration Expansion**
   - Additional DeFi protocols
   - Cross-chain bridge monitoring
   - NFT marketplace fee tracking

3. **Automation**
   - Automated treasury rebalancing
   - Smart contract-based fee distribution
   - AI-powered anomaly detection

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.

---

**Last Updated**: August 28, 2025  
**Version**: 1.0.0  
**Maintainer**: ShapeShift DAO Team  
**Status**: Production Ready ✅
