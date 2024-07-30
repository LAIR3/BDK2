# Bridge Configuration File (`bridge-config.toml`)

This configuration file is used to set up the bridge between Layer 1 (L1) and Layer 2 (L2) networks, specifically tailored for a zkEVM environment. Each section of the configuration file is crucial for the operation and management of the bridge services, ensuring efficient and secure data synchronization and transaction management across networks.

## Sections Explained

### Log Configuration

```toml
[Log]
Level = "info"
Environment = "production"
Outputs = ["stderr"]

Level: Controls the log output verbosity. Valid options are info, debug, warn, and error.
Environment: Indicates the operational environment, production for live deployments and development for testing.
Outputs: Defines the output destinations for log entries. Common outputs are stdout, stderr, and log file paths.
Synchronization Database Settings

[SyncDB]
Database = "postgres"
User = "{{.zkevm_db_bridge_user}}"
Name = "{{.zkevm_db_bridge_name}}"
Password = "{{.zkevm_db_bridge_password}}"
Host = "{{.zkevm_db_bridge_hostname}}{{.deployment_suffix}}"
Port = "{{.zkevm_db_postgres_port}}"
MaxConns = 20


Database: Type of database used, typically postgres.
User, Name, Password, Host, Port: Database connection credentials and network details.
MaxConns: Maximum number of concurrent connections to the database.


Ethereum example or L1 chain of choice connection

[Etherman]
l1URL = "{{.l1_rpc_url}}"
L2URLs = ["http://zkevm-node-rpc{{.deployment_suffix}}:{{.zkevm_rpc_http_port}}"]

l1URL: RPC URL for the Ethereum mainnet or any L1 network.
L2URLs: Array of URLs for L2 networks. Supports multiple entries for load balancing and redundancy.

Bridge Server Configuration

[BridgeServer]
GRPCPort = "{{.zkevm_bridge_grpc_port}}"
HTTPPort = "{{.zkevm_bridge_rpc_port}}"
DefaultPageLimit = 25
MaxPageLimit = 100
BridgeVersion = "v1"

GRPCPort, HTTPPort: Network ports for GRPC and HTTP services, respectively.
DefaultPageLimit, MaxPageLimit: Controls pagination limits for list responses.
BridgeVersion: API version, useful for managing backward compatibility.

Network Configuration
GenBlockNumber: Genesis block number for the rollup operation.
PolygonBridgeAddress, PolygonZkEVMGlobalExitRootAddress, PolygonRollupManagerAddress, PolygonZkEVMAddress: Contract addresses relevant to the bridge operation.
L2PolygonBridgeAddresses: Addresses for L2 bridge components.
Additional Tips
Ensure all placeholders (e.g., {{.zkevm_db_bridge_user}}) are replaced with actual operational values before deployment.
Keep sensitive information such as passwords secure and preferably use environment variables or secret management tools to inject them at runtime.



