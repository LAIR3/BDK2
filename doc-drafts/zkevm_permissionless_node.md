# Permissionless zkEVM Node Deployment Script

## Overview

This script automates the deployment of a permissionless zkEVM node, enabling it to operate independently within a blockchain network. It configures and deploys several critical services including node databases, executors, and synchronization services using a Kurtosis environment. 

## Script Functionality

### Initialize Node Databases

- **Purpose**: Establish databases essential for the zkEVM node's operation, particularly for event logging and transaction execution.
- **Implementation**:
  - Uploads and configures SQL scripts for initializing the event and executor databases.
  - Deploys these databases as services within the network.

### Configure and Start the Executor

- **Purpose**: Set up and launch the executor component that processes transactions for the zkEVM node.
- **Implementation**:
  - Reads and applies a configuration template to set up the executor.
  - Starts the executor service with the specified configuration.

### Manage Genesis File

- **Purpose**: Provision the genesis file necessary for initializing the blockchain network parameters for the node.
- **Implementation**:
  - Checks for an existing genesis artifact; if not available, generates a new one from a template.

### Launch Network Services

- **Purpose**: Activate the synchronizer and RPC services to enable blockchain operations and external API interactions.
- **Implementation**:
  - Configures and initiates the synchronizer service using the node and genesis configurations.
  - Sets up the RPC service to facilitate external communications and interactions.

## Deployment Strategy

The deployment process is structured to ensure sequential setup of each component, maintaining the operational dependencies crucial for a permissionless environment. This strategy ensures that the node is fully functional and capable of operating independently within the network.

## Usage

This script is designed for blockchain developers and system administrators aiming to deploy permissionless zkEVM nodes. It provides a robust framework for setting up and managing independent blockchain nodes.

## Running the Script

Ensure that Kurtosis is configured correctly in your environment. Customize the `args` dictionary with appropriate network parameters and file paths. Execute the script to start the deployment of the permissionless zkEVM node.

## Conclusion

By automating the deployment of a permissionless zkEVM node, this script facilitates the expansion and decentralization of blockchain networks. It provides essential tools for developers looking to enhance network robustness and autonomy.


