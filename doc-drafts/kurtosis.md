# Polygon CDK Package

## Overview

The Polygon CDK package provides a robust framework for deploying a complete Polygon CDK Devnet environment using Kurtosis. This package simplifies the configuration and management of blockchain network components, enabling developers to efficiently set up, test, and interact with a Polygon-specific blockchain environment.

## Key Features

The package supports various deployment stages and configurations, which are customizable to meet specific network requirements. The primary functionalities include:

### Deployment Stages

- **Deploy Local L1**: Sets up a local Layer 1 (L1) environment for initial blockchain interactions.
- **Deploy zkEVM Contracts on L1**: Manages the deployment of zkEVM-specific smart contracts on the L1 network.
- **Deploy zkEVM Node and CDK Peripheral Databases**: Establishes necessary databases for the zkEVM nodes and peripheral components of the CDK.
- **Deploy CDK Central/Trusted Environment**: Configures and launches the central or trusted node environment within the CDK architecture.
- **Deploy CDK/Bridge Infrastructure**: Implements bridge services between different blockchain layers or separate blockchain networks.
- **Deploy Permissionless Node**: Adds a permissionless node to the network, enhancing decentralization and security.
- **Deploy Observability Stack**: Integrates monitoring and observability tools to track network performance and health.
- **Deploy ETH Load Balancer**: Balances Ethereum transaction loads to optimize network throughput and stability.
- **Apply Workload to CDK Stack Components**: Executes predefined workload scripts to test various network functions and performance under load.

### Configurability

Configurations for the deployment process are highly customizable, with parameters that can be set through form inputs or directly via the `args` object. Default values for these parameters ensure a smooth initial setup but can be adjusted to fit specific deployment needs.

#### Default Configuration Parameters

- **Service Names and Docker Images**: Defines the Docker images for various services like zkEVM provers, nodes, data availability layers, and bridge services.
- **Network and Port Configurations**: Specifies the network IDs, port numbers for various services, and detailed configurations for database connections.
- **Private Keys and Addresses**: Lists Ethereum addresses and private keys for essential components and roles within the network.
- **Additional Settings**: Includes configurations for explorers, load balancers, and additional tools or services required during deployment.

### Usage

The package is ideal for developers working on blockchain solutions that require a customizable and scalable environment. It supports various development, testing, and production scenarios, facilitating a seamless workflow from contract deployment to network monitoring.

## Conclusion

`github.com/0xPolygon/kurtosis-cdk` stands out as a pivotal tool for blockchain developers looking to leverage Polygon's CDK capabilities within a structured and extensible framework. It provides the necessary tools and configurations to deploy a fully-functional blockchain network environment tailored to the specific needs of Polygon's technology stack.

