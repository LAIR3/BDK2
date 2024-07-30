General Configuration for Blutgang
do_clear: Clears the cache database at startup.
address: Sets the address where Blutgang service will listen for incoming RPC requests.
ma_length: Specifies the length of the moving average for latency calculations.
sort_on_startup: Determines if RPCs should be sorted by latency when the service starts.
health_check: Enables health checking functionality to monitor the service status.
header_check: Activates checking for content type headers to ensure JSON-RPC compliance.
ttl: The time to wait for a response from an RPC call in milliseconds.
max_retries: The maximum number of retries for a request before it is considered failed.
expected_block_time and health_check_ttl: Used to define the expected block time and the frequency of health checks, respectively.
supress_rpc_check: Controls whether to suppress logging of health check messages.
Admin Specific Configuration
enabled: Turns on the admin namespace which contains more sensitive or powerful functions.
address: Sets the address specifically for admin RPC requests.
readonly: Restricts admin functions to read-only operations unless explicitly set otherwise.
jwt: Enables JWT (JSON Web Token) authentication for secured access.
Database Configuration Using Sled
db_path: Path to the Sled database used for caching.
mode: Operational mode of the database, which can be set to prioritize throughput or storage space.
cache_capacity: Sets the size of the cache in bytes.
compression: Toggles compression, which can reduce storage size at the cost of increased CPU usage and latency.
flush_every_ms: Sets the frequency at which the database writes changes to disk.
RPC Services Configuration
url and ws_url: Define URLs for JSON-RPC and WebSocket connections, respectively.
max_consecutive and max_per_second: Control the load handling capabilities by setting limits on consecutive uses and queries per second for the defined services.
This configuration is highly customizable, tailored to optimize the performance and security of the services provided by Blutgang. The use of variables like {{.blutgang_rpc_port}} suggests that the configuration might be managed through a templating system that injects these values at deployment time, allowing for environment-specific configurations without changing the base configuration file.
