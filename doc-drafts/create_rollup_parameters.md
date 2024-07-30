# zkEVM/CDK Configuration Parameters

This JSON object outlines various parameters required for deploying contracts in a zkEVM/CDK environment. Each field is crucial for ensuring the contracts are deployed correctly.

Field Descriptions

    adminZkEVM: The address of the Layer 2 (L2) administrator for zkEVM.
    chainID: The unique identifier for the blockchain network. Represents the chain ID of the zkEVM rollup.
    consensusContract: The address of the consensus contract for the rollup.
    dataAvailabilityProtocol: Specifies the protocol used for data availability. Here, it uses "PolygonDataCommittee."
    deployerPvtKey: Private key of the account responsible for deploying the contracts.
    description: A description of the network environment, such as "kurtosis-devnet."
    forkID: An identifier representing the specific fork in the rollup process.
    gasTokenAddress: Address of the gas token (currently left empty in this configuration).
    maxFeePerGas: Maximum fee per unit of gas (left empty).
    maxPriorityFeePerGas: Maximum priority fee per unit of gas (left empty).
    multiplierGas: Multiplier for gas usage (left empty).
    networkName: The name of the network environment, like "kurtosis-test."
    realVerifier: Boolean flag indicating if a real verifier is in use (currently set to "false").
    trustedSequencer: The address of the trusted sequencer for zkEVM.
    trustedSequencerURL: URL for the trusted sequencer, dynamically constructed using the deployment suffix and port number.
    trustedAggregator: The address of the trusted aggregator for zkEVM.

Notes:

    maxFeePerGas and maxPriorityFeePerGas are currently empty. Ensure sensitive fields such as private keys are handled securely and not exposed in public repositories.
