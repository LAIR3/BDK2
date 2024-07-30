# databases.star: Initializing zkEVM Databases

## Overview

This script facilitates the setup and launch of database services for a zkEVM node environment. It specifically manages the initialization of databases for events and provers, along with additional peripheral databases necessary for the zkEVM operation.

## Key Modules and Packages

- `zkevm_databases_package`: Handles the creation of database service configurations, ensuring each database is configured correctly according to the provided specifications and initial scripts.

## Function: `run(plan, args)`

### Description

This function orchestrates the deployment of database services, uploading initialization scripts and creating service configurations.

### Parameters

- `plan`: An object that manages the deployment environment, capable of handling file uploads and service additions.
- `args`: A dictionary containing deployment-specific parameters such as suffixes for identifying unique service instances.

### Workflow

1. **Upload Initialization Scripts**:
   - Uploads SQL scripts for initializing the event and prover databases. These scripts are stored with a unique suffix to differentiate between deployments.

2. **Create Database Service Configurations**:
   - Calls the `zkevm_databases_package` to generate service configurations for node databases using the uploaded SQL scripts.
   - Generates configurations for peripheral databases that are crucial for the full operation of the zkEVM infrastructure.

3. **Deploy Services**:
   - Adds the configured database services to the deployment plan, effectively starting the databases with their respective configurations.

### Helper Functions

- No additional helper functions are detailed in the script, but the `zkevm_databases_package` likely contains methods such as:
  - `create_node_db_service_configs()`: Generates configurations for node databases.
  - `create_peripheral_databases_service_configs()`: Handles configurations for peripheral databases.

## Configuration and Template Management

Utilizes SQL templates for the initial setup of databases, ensuring that each database is prepared with the necessary schema and settings for operation. These templates are parameterized with deployment-specific data to adapt to varying environments and requirements.

## Service Deployment and Management

Database services are carefully managed through the deployment plan, which orchestrates their configuration, launch, and integration within the larger zkEVM infrastructure.

## Security and Data Handling

The script ensures secure handling of database initialization scripts and maintains strict management of configuration data to prevent unauthorized access or misconfigurations.

## Usage

This script is designed to be executed in a controlled deployment environment where the deployment plan and necessary arguments are predefined. It is typically triggered automatically as part of a larger deployment workflow.


databases.star is essential for the robust and efficient setup of databases in a zkEVM environment, demonstrating an advanced use of Python scripting to automate and streamline the deployment of critical infrastructure components.


databases.star script is designed to support both local and remote Postgres databases within the Kurtosis-CDK package. It configures and initializes databases essential for the zkEVM node environment. This script allows users to choose between creating databases locally within the service or using preconfigured remote Postgres instances.

    Local and Remote Postgres Support:
        USE_REMOTE_POSTGRES: A boolean flag that determines whether to use local or remote Postgres databases.
        When False, the script creates and manages databases locally.
        When True, it uses remote Postgres instances, and the service is used for parameter injection across pods.

    Database Configuration Parameters:
        Static and dynamic parameters are defined for database service configuration.
        Users can customize Postgres hostnames, ports, and credentials according to their deployment needs.

    Initialization Scripts:
        SQL scripts are used to initialize databases, ensuring they are set up with the necessary schema and settings.

# Configuration Details

    Constants:

```python
USE_REMOTE_POSTGRES = False
POSTGRES_HOSTNAME = "127.0.0.1"
POSTGRES_IMAGE = "postgres:16.2"
POSTGRES_SERVICE_NAME = "postgres"
POSTGRES_PORT = 5432
POSTGRES_MASTER_DB = "master"
POSTGRES_MASTER_USER = "master_user"
POSTGRES_MASTER_PASSWORD = "master_password"
```
Databases:

# Trusted Databases: Critical databases with initial SQL scripts.

```python
TRUSTED_DATABASES = {
    "event_db": {
        "name": "event_db",
        "user": "event_user",
        "password": "redacted",
        "init": read_file(src="./templates/databases/event-db-init.sql"),
    },
    "pool_db": {
        "name": "pool_db",
        "user": "pool_user",
        "password": "redacted",
    },
    "prover_db": {
        "name": "prover_db",
        "user": "prover_user",
        "password": "redacted",
        "init": read_file(src="./templates/databases/prover-db-init.sql"),
    },
    "state_db": {
        "name": "state_db",
        "user": "state_user",
        "password": "redacted",
    },
}
```
# Permissionless Databases: Additional databases without initial SQL scripts.

```python
PERMISSIONLESS_DATABASES = {
    "agglayer_db": {
        "name": "agglayer_db",
        "user": "agglayer_user",
        "password": "redacted",
    },
    "bridge_db": {
        "name": "bridge_db",
        "user": "bridge_user",
        "password": "redacted",
    },
    "dac_db": {
        "name": "dac_db",
        "user": "dac_user",
        "password": "redacted",
    },
}
```

# Combined Database Configurations:

```python
        DATABASES = TRUSTED_DATABASES | PERMISSIONLESS_DATABASES
```

Helper Functions

    Service Name Generation:
        Generates service names based on suffix.

```python
def _service_name(suffix):
    return POSTGRES_SERVICE_NAME + suffix

def _pless_suffix(suffix):
    return "-pless" + suffix
```

# Database Configuration Retrieval:

    Retrieves configurations for all databases, adjusting for local or remote setups.

```python
def get_db_configs(suffix):
    configs = {
        k: v | {
            "hostname": POSTGRES_HOSTNAME if USE_REMOTE_POSTGRES else _service_name(suffix),
            "port": POSTGRES_PORT,
        }
        for k, v in DATABASES.items()
    }
    return configs

def get_pless_db_configs(suffix):
    configs = {
        k: v | {
            "hostname": POSTGRES_HOSTNAME if USE_REMOTE_POSTGRES else _service_name(_pless_suffix(suffix)),
            "port": POSTGRES_PORT,
        }
        for k, v in TRUSTED_DATABASES.items()
    }
    return configs
```

# Service Creation:

    Creates a Postgres service with the given configurations.

```python
    def create_postgres_service(plan, db_configs, suffix):
        init_script_tpl = read_file(src="./templates/databases/init.sql")
        init_script = plan.render_templates(
            name="init.sql" + suffix,
            config={
                "init.sql": struct(
                    template=init_script_tpl,
                    data={
                        "dbs": db_configs,
                        "master_db": POSTGRES_MASTER_DB,
                        "master_user": POSTGRES_MASTER_USER,
                    },
                )
            },
        )

        postgres_service_cfg = ServiceConfig(
            image=POSTGRES_IMAGE,
            ports={
                "postgres": PortSpec(POSTGRES_PORT, application_protocol="postgresql"),
            },
            env_vars={
                "POSTGRES_DB": POSTGRES_MASTER_DB,
                "POSTGRES_USER": POSTGRES_MASTER_USER,
                "POSTGRES_PASSWORD": POSTGRES_MASTER_PASSWORD,
            },
            files={"/docker-entrypoint-initdb.d/": init_script},
            cmd=["-N 1000"],
        )

        plan.add_service(
            name=_service_name(suffix),
            config=postgres_service_cfg,
            description="Starting Postgres Service",
        )
```

# Main Functions

    Run Function:
        Executes the deployment plan for creating and configuring database services.

```python
def run(plan, suffix):
    db_configs = get_db_configs(suffix)
    create_postgres_service(plan, db_configs, suffix)
```

# Run Permissionless Function:

    Executes the deployment plan specifically for permissionless database configurations.

```python
    def run_pless(plan, suffix):
        db_configs = get_pless_db_configs(suffix)
        create_postgres_service(plan, db_configs, _pless_suffix(suffix))
```

databases.star is a robust tool for managing both local and remote Postgres databases within the zkEVM environment. It ensures the correct initialization and configuration of databases necessary for zkEVM operations, offering flexibility and security through its structured approach and use of initialization scripts. By leveraging this script, users can automate and streamline the deployment of their database infrastructure, ensuring a smooth and reliable setup process.

