# Bridge Manual Claim Script

## Overview

This Bash script automates the process of claiming assets transferred via a zkEVM bridge on Layer 1 (Ethereum). It efficiently interacts with the blockchain to verify, claim, and finalize transfers from the zkEVM environment to Ethereum.

## Prerequisites

- **Kurtosis**: Ensure that your environment is set up with Kurtosis to access necessary service endpoints.
- **Cast Tool**: This script uses the `cast` tool for Ethereum interactions; ensure it is installed.
- **jq Tool**: Used for JSON data manipulation, ensure `jq` is installed in your environment.

## Script Features

- **Dynamic Bridge Address Retrieval**: Automatically fetches the zkEVM bridge address using Kurtosis.
- **Deposit Listing and Filtering**: Retrieves and filters claimable transactions destined for Ethereum (L1).
- **Transaction Claiming**: Uses provided private keys and RPC URLs to claim verified transactions.

## Configuration Variables

- `private_key`: The Ethereum private key used to authorize transactions.
- `destination_net`: Network ID for Ethereum (set to `0` for Layer 1).
- `destination_addr`: Ethereum address to which the assets will be claimed.
- `bridge_addr`: Address of the zkEVM bridge contract, fetched dynamically.

## Execution Flow

1. **Setup Variables**: Initialize necessary variables including network addresses and the private key.
2. **Retrieve and Filter Deposits**: Connect to the bridge service API, retrieve the list of deposits, and filter them to find those eligible for claims.
3. **Claim Transactions**: For each eligible deposit, retrieve necessary proofs and construct the transaction payload, then simulate and execute the claim on Ethereum.

bridge_manual_claim.sh is essential for managing cross-chain asset transfers in zkEVM settings, providing a robust tool for administrators and users to ensure their assets are correctly claimed on the Ethereum network. bridge_manual_claim.sh automates the complexities involved in interacting with blockchain technologies to facilitate claiming of the minted asset across the bridge.

## Usage

To run the script, ensure you have the necessary permissions set and dependencies installed, then execute:

```bash
./bridge_manual_claim.sh

