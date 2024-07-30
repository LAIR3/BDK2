# Batch Verification Monitor Script

## Overview

This Bash script is specifically designed to monitor the verification progress of zkEVM batches within a blockchain network. It's an essential tool for administrators and developers to ensure that the zkEVM node is functioning properly by tracking the number of verified batches against a target within a specified timeframe.

## Features

- **Real-Time Monitoring**: Continuously checks the number of batches verified by the zkEVM node.
- **Timeout Feature**: Automatically exits if the monitoring exceeds the set timeout period, ensuring system resources are not overused.
- **Target Achievement Notification**: Notifies the user once the target number of verified batches is reached.

## Prerequisites

To use this script, you need:
- A Unix-like environment with Bash support.
- The `cast` command-line tool installed, which is used for interacting with Ethereum nodes.
- Network access to the zkEVM node you intend to monitor.

## Script Parameters

- `verified_batches_target`: The number of batches you expect to be verified.
- `timeout`: The maximum time (in seconds) the script should run if the target isn't reached.

## Usage Guide

### Setting Permissions

Ensure the script is executable by running:

```bash
chmod +x batch_verification_monitor.sh


./batch_verification_monitor.sh [verified_batches_target] [timeout]

./batch_verification_monitor.sh 50 1800

This command will monitor the zkEVM node until 50 batches are verified or 30 minutes have passed.

# Script Output
The script provides a timestamped log in the console:

It reports the current number of verified batches at regular intervals.
It logs a success message and exits if the target is reached within the timeout.
It logs a failure message and exits if the timeout period is exceeded without reaching the target.

# Exit Conditions
Success: The script will exit successfully when the target number of verified batches is reached before the timeout.
Timeout: If the timeout is reached without achieving the target, the script will exit with a failure status.

 batch_verification_monitor.sh is a valuable tool for maintaining the health and performance of zkEVM nodes. By ensuring that batch processing is occurring as expected, batch_verification_monitor.sh helps maintain the integrity and efficiency of blockchain operations.




