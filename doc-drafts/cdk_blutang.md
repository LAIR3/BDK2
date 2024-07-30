# Function: `run`

## Overview
This function automates the deployment of services in a blockchain infrastructure, utilizing dynamically generated configuration files and service setups. It handles the creation of the `blutgang` service configuration, reads from templates, retrieves service details, and prepares URLs for various service endpoints.

## Parameters
- `plan`: The deployment plan object that manages the state and configuration of services.
- `args`: A dictionary containing arguments that influence the service configurations, such as deployment suffixes, service images, and port specifications.

## Process Description

### Configuration Name Generation
- **Blutgang Name**: Constructs the service name for Blutgang by appending a deployment suffix to "blutgang", allowing for unique identification of the service instance.
  
### Template Reading
- **Configuration Template**: Reads a TOML file template for the Blutgang configuration from a predefined path (`./templates/blutgang/blutgang-config.toml`).

### Service Detail Retrieval
- Retrieves network details for three specific services (sequencer, RPC, and permissionless RPC) which are part of the zkEVM node setup. This includes:
  - **Sequencer Service**: Fetches IP address and RPC port.
  - **RPC Service**: Fetches both HTTP and WebSocket URLs.
  - **Permissionless RPC Service**: Similarly fetches both HTTP and WebSocket URLs.

### URL Construction
- Constructs URLs for each service by combining the IP addresses and port numbers retrieved from the service details. This includes HTTP and WebSocket URLs for both the standard and permissionless RPC services.

### Configuration Rendering
- **Configuration Artifact**: Uses the `plan.render_templates` method to dynamically replace placeholders in the Blutgang configuration template with actual values derived from service details and additional arguments. This config artifact is then used in setting up the service.

### Service Configuration Setup
- **Service Configuration**: Defines the configuration for the Blutgang service, including:
  - Docker image to be used.
  - Port specifications for the service’s HTTP and administrative interfaces.
  - Configuration files to be mounted inside the service container.

### Service Deployment
- **Add Service to Plan**: Adds the Blutgang service to the deployment plan with the specified configuration and a description of the service start-up.

## Usage Example
To deploy the services with custom configurations as specified by user arguments and system states, this function is invoked with a plan and argument dictionary containing necessary deployment details such as service names, image references, and ports.

## Notes
- Ensure that all provided services and templates are correctly specified and accessible.
- The function assumes that the service infrastructure (like Docker, Kubernetes) and the plan management system are appropriately configured to handle these operations.

