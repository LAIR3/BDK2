# Workload Deployment Script for zkEVM Network

Starlark function run is designed to configure and deploy workload scripts that perform testing and stress simulations on a blockchain network, specifically focusing on the zkEVM (zero-knowledge Ethereum Virtual Machine) node. This script utilizes templating to create and manage workload scripts dynamically based on the provided network configurations.

## Overview

This script is part of a suite of tools used to deploy and manage workloads on a zkEVM network. It automates the deployment of workload scripts that test various aspects of the blockchain network, including load testing and RPC fuzzing.

## Functionality

### Script Deployment

- **Purpose**: The main function `run` orchestrates the setup and execution of workload scripts on the blockchain network.
- **Script Artifacts**:
  - Creates artifacts from templates for workload application, load testing, and RPC fuzz testing scripts.
  - Each script is configured dynamically with network-specific parameters such as RPC URLs and chain IDs.

### Service Configuration

- Configures a service that executes the workload scripts in an isolated environment.
- The service is set up to execute these scripts sequentially to simulate different network interactions and stress tests.

### Key Components

- **Workload Application**: Deploys scripts that apply predefined workload commands to the network.
- **Load Testing**: Utilizes `polycli_loadtest_on_l2.sh` to simulate transaction loads on Layer 2 of the blockchain.
- **RPC Fuzz Testing**: Deploys `polycli_rpcfuzz_on_l2.sh` to perform fuzz testing on the RPC interfaces of the zkEVM node.

### Template Data Mapping

- Maps dynamic data into scripts such as RPC URLs and specific blockchain parameters to ensure the scripts interact correctly with the live network environment.
- Uses network configuration parameters to adjust the behavior of the scripts dynamically, adapting to network-specific needs and configurations.

## Usage

This script is crucial for blockchain developers and network administrators who need to ensure the robustness and efficiency of their blockchain solutions. It allows for automated, dynamic testing of network components, providing essential feedback on performance and stability.

### Steps to Run the Script

1. Ensure all prerequisites are met, including necessary permissions and network accessibility.
2. Adjust the `args` dictionary to match your specific network configurations.
3. Run the script within your deployment environment to start the testing processes.

## Conclusion

This deployment script is an essential tool for conducting thorough and effective testing of blockchain networks, specifically designed to handle the complexities of zkEVM environments. It provides a robust framework for testing the scalability and stability of blockchain applications under various conditions.


