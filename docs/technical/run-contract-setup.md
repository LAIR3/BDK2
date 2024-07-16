# zkEVM/CDK Contract Deployment Script

This Bash script is used to deploy contracts in the zkEVM/CDK environment. It ensures proper sequencing, funds accounts, and deploys contracts.

## Table of Contents

1. [Functions](#functions)
    - [echo_ts](#echo_ts)
    - [wait_for_rpc_to_be_available](#wait_for_rpc_to_be_available)
    - [fund_account_on_l1](#fund_account_on_l1)
2. [Execution Logic](#execution-logic)
    - [RPC Availability Check](#rpc-availability-check)
    - [Account Funding](#account-funding)
    - [Deployment Configuration](#deployment-configuration)
    - [Contract Deployment](#contract-deployment)
    - [Post-Deployment Configuration](#post-deployment-configuration)

## Functions

### `echo_ts`
A simple function that prefixes log messages with a timestamp.

```bash
echo_ts() {
    timestamp=$(date +"[%Y-%m-%d %H:%M:%S]")
    echo "$timestamp $1"
}
```
wait_for_rpc_to_be_available

Waits for the specified RPC URL to become available.

    Arguments:
        rpc_url: The URL of the RPC endpoint to check.
    Behavior:
        Retries up to 20 times before exiting if unsuccessful.
```bash
wait_for_rpc_to_be_available() {
    rpc_url="$1"
    counter=0
    max_retries=20
    until cast send --rpc-url "{{.l1_rpc_url}}" --mnemonic "{{.l1_preallocated_mnemonic}}" --value 0 "{{.zkevm_l2_sequencer_address}}"; do
        ((counter++))
        echo_ts "L1 RPC might not be ready... Retrying ($counter)..."
        if [ $counter -ge $max_retries ]; then
            echo_ts "Exceeded maximum retry attempts. Exiting."
            exit 1
        fi
        sleep 5
    done
}
```

fund_account_on_l1

Funds an account on Layer 1 (L1) by sending Ether.

    Arguments:
        name: The name of the account.
        address: The address of the account to fund.

```bash
fund_account_on_l1() {
    name="$1"
    address="$2"
    echo_ts "Funding $name account"
    cast send \
        --rpc-url "{{.l1_rpc_url}}" \
        --mnemonic "{{.l1_preallocated_mnemonic}}" \
        --value "{{.l1_funding_amount}}" \
        "$address"
}
```

# Execution Logic
RPC Availability Check

Ensures the L1 RPC endpoint is ready before proceeding.

if [[ -e "/opt/zkevm/.init-complete.lock" ]]; then
    echo "This script has already been executed"
    exit
fi

echo_ts "Waiting for the L1 RPC to be available"
wait_for_rpc_to_be_available "{{.l1_rpc_url}}"
echo_ts "L1 RPC is now available"

# Account Funding

Funds the necessary accounts on L1
```bash
echo_ts "Funding important accounts on l1"
fund_account_on_l1 "admin" "{{.zkevm_l2_admin_address}}"
fund_account_on_l1 "sequencer" "{{.zkevm_l2_sequencer_address}}"
fund_account_on_l1 "aggregator" "{{.zkevm_l2_aggregator_address}}"
fund_account_on_l1 "agglayer" "{{.zkevm_l2_agglayer_address}}"
fund_account_on_l1 "claimtxmanager" "{{.zkevm_l2_claimtxmanager_address}}"
```
# zkEVM/CDK Contract Deployment Script

This Bash script is used to deploy contracts in the zkEVM/CDK environment. It ensures proper sequencing, funds accounts, and deploys contracts.

## Table of Contents

1. [Functions](#functions)
    - [echo_ts](#echo_ts)
    - [wait_for_rpc_to_be_available](#wait_for_rpc_to_be_available)
    - [fund_account_on_l1](#fund_account_on_l1)
2. [Execution Logic](#execution-logic)
    - [RPC Availability Check](#rpc-availability-check)
    - [Account Funding](#account-funding)
    - [Deployment Configuration](#deployment-configuration)
    - [Contract Deployment](#contract-deployment)
    - [Post-Deployment Configuration](#post-deployment-configuration)

## Functions

### `echo_ts`
A simple function that prefixes log messages with a timestamp.

```bash
echo_ts() {
    timestamp=$(date +"[%Y-%m-%d %H:%M:%S]")
    echo "$timestamp $1"
}
```
wait_for_rpc_to_be_available

Waits for the specified RPC URL to become available.

    Arguments:
        rpc_url: The URL of the RPC endpoint to check.
    Behavior:
        Retries up to 20 times before exiting if unsuccessful.

```bash
wait_for_rpc_to_be_available() {
    rpc_url="$1"
    counter=0
    max_retries=20
    until cast send --rpc-url "{{.l1_rpc_url}}" --mnemonic "{{.l1_preallocated_mnemonic}}" --value 0 "{{.zkevm_l2_sequencer_address}}"; do
        ((counter++))
        echo_ts "L1 RPC might not be ready... Retrying ($counter)..."
        if [ $counter -ge $max_retries ]; then
            echo_ts "Exceeded maximum retry attempts. Exiting."
            exit 1
        fi
        sleep 5
    done
}
```

fund_account_on_l1

Funds an account on Layer 1 (L1) by sending Ether.

    Arguments:
        name: The name of the account.
        address: The address of the account to fund.

```bash
fund_account_on_l1() {
    name="$1"
    address="$2"
    echo_ts "Funding $name account"
    cast send \
        --rpc-url "{{.l1_rpc_url}}" \
        --mnemonic "{{.l1_preallocated_mnemonic}}" \
        --value "{{.l1_funding_amount}}" \
        "$address"
}
```

Execution Logic
RPC Availability Check

Ensures the L1 RPC endpoint is ready before proceeding.

```bash
if [[ -e "/opt/zkevm/.init-complete.lock" ]]; then
    echo "This script has already been executed"
    exit
fi

echo_ts "Waiting for the L1 RPC to be available"
wait_for_rpc_to_be_available "{{.l1_rpc_url}}"
echo_ts "L1 RPC is now available"
```

# Account Funding

Funds the necessary accounts on L1

```bash
echo_ts "Funding important accounts on l1"
fund_account_on_l1 "admin" "{{.zkevm_l2_admin_address}}"
fund_account_on_l1 "sequencer" "{{.zkevm_l2_sequencer_address}}"
fund_account_on_l1 "aggregator" "{{.zkevm_l2_aggregator_address}}"
fund_account_on_l1 "agglayer" "{{.zkevm_l2_agglayer_address}}"
fund_account_on_l1 "claimtxmanager" "{{.zkevm_l2_claimtxmanager_address}}"
```

# Deployment Configuration

Copies deployment parameters and updates the Hardhat configuration
```bash
pushd /opt/zkevm-contracts || exit 1
cp /opt/contract-deploy/deploy_parameters.json /opt/zkevm-contracts/deployment/v2/deploy_parameters.json
cp /opt/contract-deploy/create_rollup_parameters.json /opt/zkevm-contracts/deployment/v2/create_rollup_parameters.json
sed -i 's#http://127.0.0.1:8545#{{.l1_rpc_url}}#' hardhat.config.ts
```

## Contract Deployment

Deploys contracts using Hardhat, including optional gas token contracts
```bash
{{if .zkevm_use_gas_token_contract}}
echo_ts "Deploying gas token to L1"
printf "[profile.default]\nsrc = 'contracts'\nout = 'out'\nlibs = ['node_modules']\n" > foundry.toml
forge create \
    --json \
    --rpc-url "{{.l1_rpc_url}}" \
    --mnemonic "{{.l1_preallocated_mnemonic}}" \
    contracts/mocks/ERC20PermitMock.sol:ERC20PermitMock \
    --constructor-args  "CDK Gas Token" "CDK" "{{.zkevm_l2_admin_address}}" "1000000000000000000000000" > gasToken-erc20.json
```
# Post-Deployment Configuration

Combines deployment outputs, modifies them, and ensures proper roles are granted.

    Combine Files
        Merges multiple JSON files into a combined.json.

    Address Mapping
        Maps critical addresses like the global exit root address.

    Set Data Committee
        Sets up the data availability committee

# Step 1: Configure and Approve
```bash
echo_ts "Combining contract deploy files"
# ...
```
