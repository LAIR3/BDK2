# Regression Tests - Polygon CDK

## Overview
This documentation describes the process for deploying a local Kurtosis CDK devnet and conducting a series of regression tests. This test suite is crucial for ensuring the stability and functionality of new releases and updates to the CDK components.

**Author:** Dan Moore

### Inputs
- **zkevm_agglayer:** Version of the 0xPolygon/agglayer (default: 0.1.4)
- **zkevm_bridge_service:** Version of the 0xPolygonHermez/zkevm-bridge-service (default: v0.4.2)
- **zkevm_bridge_ui:** Version of the 0xPolygonHermez/zkevm-bridge-ui (default: '0006445')
- **zkevm_dac:** Version of the 0xPolygon/cdk-data-availability (default: 0.0.7)
- **zkevm_node:** Version of the 0xPolygon/cdk-validium-node (default: 0.6.5-cdk)
- **kurtosis_cli:** Version of kurtosis-tech/kurtosis (default: 0.89.3)
- **kurtosis_cdk:** Version of 0xPolygon/kurtosis-cdk (default: v0.2.0)
- **bake_time:** Duration for which the test network should run (default: 30 minutes)

## Test Execution Steps

### Step 1: Environment Setup
Clone and build the necessary components from their respective repositories. This includes the agglayer, bridge service, bridge UI, data availability component, and the validium node. If a specific commit hash or release tag is provided, the corresponding version of the component is built; otherwise, the default version is used.

```bash
# Example for cloning and building the agglayer
if [[ ${{ inputs.zkevm_agglayer }} =~ ^[0-9a-fA-F]{7}$ ]]; then
  git clone https://github.com/0xPolygon/agglayer.git
  cd agglayer
  git checkout "${{ inputs.zkevm_agglayer }}"
  docker compose -f docker/docker-compose.yaml build --no-cache agglayer
else
  echo "Skipping building agglayer as release tag provided: ${{ inputs.zkevm_agglayer }}"
fi
```

Deploy CDK Devnet

Using Kurtosis, deploy the local CDK devnet environment. Update the params.yml file with the versions of the components built or specified, and execute the deployment.

```bash
kurtosis run --enclave cdk-v1 --args-file params.yml --image-download always .
```

Conduct Regression Tests

Monitor and report any potential regressions. Use Foundry tools to interact with the smart contracts and simulate various transactions and interactions within the CDK environment.

// test this
```bash
# Example of monitoring script
bake_time=30
end_minute=$(( $(date +'%M') + bake_time))
export ETH_RPC_URL="$(kurtosis port print cdk-v1 zkevm-node-rpc-001 http-rpc)"
INITIAL_STATUS=$($GITHUB_WORKSPACE/cast rpc zkevm_verifiedBatchNumber 2>/dev/null)
incremented=false
while [ $(date +'%M') -lt $end_minute ]; do
  if STATUS=$($GITHUB_WORKSPACE/cast rpc zkevm_verifiedBatchNumber 2>/dev/null); then
    echo "ZKEVM_VERIFIED_BATCH_NUMBER: $STATUS"
    if [ "$STATUS" != "$INITIAL_STATUS" ]; then
      incremented=true
      echo "ZKEVM_VERIFIED_BATCH_NUMBER successfully incremented to $STATUS. Exiting..."
      exit 0
    fi
  else
    echo "Failed to connect, waiting and retrying..."
    sleep 60
    continue
  fi
  sleep 60
done
if ! $incremented; then
  echo "ZKEVM_VERIFIED_BATCH_NUMBER did not increment. This may indicate chain experienced a regression. Please investigate."
  exit 1
fi
```
```bash
kurtosis exec --enclave cdk-v1 --script monitor_and_test.sh
```

After testing, clean up the environment to ensure no residual data or configurations could impact subsequent tests.

```bash
kurtosis clean --all
```

This regression test suite ensures that each update or change to the LAIR3-BDK components meets the required standards and functions as expected within the local devnet environment. It's crucial for maintaining the reliability and stability of the system.





