# HAProxy Configuration

This configuration file details the setup of HAProxy to route and manage HTTP traffic based on URL paths. It is structured into several sections: global settings, default settings, frontend, and multiple backend configurations.

## Global Settings

```plaintext
global
    log stdout format raw local0

log: Configures HAProxy to log to the standard output using a raw format, suitable for environments like Docker

# Default Settings

defaults
    log global
    mode http
    option httplog
    option dontlognull
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms


Applies universally unless overridden in specific proxy configurations
timeout connect: Maximum time to establish a TCP connection
timeout client/server: Idle timeout settings for client and server connections
# Frontend Configuration

frontend http_in
    bind *:80
    http-response set-header Access-Control-Allow-Origin "*"
    http-response set-header Access-Control-Allow-Methods "GET, DELETE, OPTIONS, POST, PUT"

    acl url_l1rpc path_beg /l1rpc
    acl url_l2rpc path_beg /l2rpc
    acl url_bridgeservice path_beg /bridgeservice

    use_backend backend_l1rpc if url_l1rpc
    use_backend backend_l2rpc if url_l2rpc
    use_backend backend_bridgeservice if url_bridgeservice
    default_backend backend_default


bind: Listen on all network interfaces at port 80.
http-response: Configures cross-origin resource sharing (CORS).
acl: Defines Access Control Lists to match URL paths.
use_backend: Directs traffic to different backends based on ACL matches.
Backend Configurations

# L1 RPC Backend

backend backend_l1rpc
    http-request set-path /
    server server1 {{.l1rpc_ip}}:{{.l1rpc_port}}

Similar setup as the L1 RPC backend but for L2 RPC traffic.

# Bridge Service Backend

backend backend_bridgeservice
    http-request set-path "%[path,regsub(^/bridgeservice/,/)]"
    server server3 {{.bridgeservice_ip}}:{{.bridgeservice_port}}

Adjusts the request path by removing the /bridgeservice prefix.

# Default Backend

backend backend_default
    server server4 {{.bridgeui_ip}}:{{.bridgeui_port}}

This HAProxy configuration is designed to provide flexible and efficient routing of HTTP traffic based on URL paths. It includes robust logging and connection management features, making it suitable for production environments.



