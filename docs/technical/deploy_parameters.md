# deploy_parameters.json
Field Descriptions explanation change configuration as necessary

    admin: The address of the administrator responsible for zkEVM operations.
    deployerPvtKey: Private key of the deployer account. Used for deploying and managing contracts.
    emergencyCouncilAddress: Address of the council responsible for emergency management.
    initialZkEVMDeployerOwner: Owner address for the zkEVM deployer.
    maxFeePerGas: Specifies the maximum gas fee (currently empty).
    maxPriorityFeePerGas: Specifies the maximum priority fee per gas unit (currently empty).
    minDelayTimelock: Minimum delay before executing timelock changes. The value is in seconds (3600 seconds = 1 hour).
    multiplierGas: Gas multiplier used to calculate the final gas fee (currently empty).
    pendingStateTimeout: Timeout value for pending states, set to 604799 seconds (approx. 7 days).
    polTokenAddress: Address of the POL token (currently empty).
    salt: Salt used for unique hashing during deployments, ensuring no collisions.
    timelockAdminAddress: Address for the admin managing the timelock mechanism.
    trustedSequencer: Address of the trusted sequencer in the zkEVM environment.
    trustedSequencerURL: URL pointing to the trusted sequencer, including a dynamic suffix and port number.
    trustedAggregator: Address of the trusted aggregator responsible for aggregation.
    trustedAggregatorTimeout: Timeout value for the trusted aggregator, set to 604799 seconds (approx. 7 days).
    forkID: Identifier for the specific fork of the zkEVM rollup.
    test: Boolean flag indicating whether the configuration is for a test environment (true) or production (false).

Notes:

    Ensure sensitive data such as private keys are handled securely.
    Adjust the timeout values according to specific security and operational requirements.
