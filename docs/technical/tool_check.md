# Version Check Script for Kurtosis CDK Requirements

## Overview

This script verifies that all required and optional software tools are installed with correct versions to support Kurtosis CDK packages. It ensures the development environment is set up correctly before deployment processes begin.

## Prerequisites

- Bash shell environment.
- Tools such as `kurtosis`, `docker`, `jq`, `yq`, `cast`, and `polycli` should be installed.

## Features

- **Version Verification**: Checks that the installed versions of required tools like Kurtosis and Docker meet or exceed the minimum supported versions.
- **Optional Tools Check**: Verifies the presence and versions of optional tools, providing installation guidance if not present.

## Script Breakdown

### Required Tools

1. **Kurtosis**: Ensures Kurtosis is installed and checks if the installed version meets the required version.
2. **Docker**: Validates that Docker is installed with a version at or above the supported level.

### Optional Tools

- **jq and yq**: Checks for the installation of jq and yq, tools used for handling JSON and YAML files.
- **cast**: Verifies the installation of the Cast tool, used for Ethereum contract interactions.
- **polycli**: Ensures the Polygon CLI tool is installed for managing Polygon-specific operations.

### Helper Functions

- `ensure_required_tool_is_installed`: Checks for the presence of essential tools and exits if any are missing.
- `ensure_optional_tool_is_installed`: Checks for optional tools and advises on installation if absent.

## Usage

To run the script, simply execute it in your terminal:

```bash
./version_check.sh

