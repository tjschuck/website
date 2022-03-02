---
title: vtctld
description: The Vitess Admin GUI
---

`vtctld` is a webserver interface to administer a Vitess cluster. It is usually the first Vitess component to be started after a valid global topology service has been created.

## Example Usage

The following example launches the `vtctld` daemon on port 15000:

```bash
export TOPOLOGY_FLAGS="-topo_implementation etcd2 -topo_global_server_address localhost:2379 -topo_global_root /vitess/global"
export VTDATAROOT="/tmp"

vtctld \
 $TOPOLOGY_FLAGS \
 -workflow_manager_init \
 -workflow_manager_use_election \
 -service_map 'grpc-vtctl' \
 -backup_storage_implementation file \
 -file_backup_storage_root $VTDATAROOT/backups \
 -log_dir $VTDATAROOT/tmp \
 -port 15000 \
 -grpc_port 15999
```

## Options

| Name | Type     | Definition |
| :------------------------------------ |:---------| :----------------------------------------------------------------------------------------- |
| -action_timeout | duration | Time to wait for an action before resorting to force (default 2m0s) |
| -alsologtostderr | boolean  | Log to standard error as well as files |
| -app_idle_timeout | duration | Idle timeout for app connections (default 1m0s) |
| -app_pool_size | int      | Size of the connection pool for app connections (default 40) |
| -azblob_backup_account_key_file | string   | Path to a file containing the Azure Storage account key; if this flag is unset, the environment variable VT_AZBLOB_ACCOUNT_KEY will be used as the key itself (NOT a file path) |
| -azblob_backup_account_name | string   | Azure Storage Account name for backups; if this flag is unset, the environment variable VT_AZBLOB_ACCOUNT_NAME will be used |
| -azblob_backup_container_name | string   | Azure Blob Container Name |
| -azblob_backup_parallelism | int      | Azure Blob operation parallelism (requires extra memory when increased) (default 1) |
| -azblob_backup_storage_root | string   | Root prefix for all backup-related Azure Blobs; this should exclude both initial and trailing '/' (e.g. just 'a/b' not '/a/b/') |
| -backup_engine_implementation | string   | Specifies which implementation to use for creating new backups (builtin or xtrabackup). Restores will always be done with whichever engine created a given backup (default "builtin") |
| -backup_storage_block_size | int      | If backup_storage_compress is true, backup_storage_block_size sets the byte size for each block while compressing (default is 250000) |
| -backup_storage_compress | boolean | If set, the backup files will be compressed. Set to false for instance if a backup_storage_hook is specified and it compresses the data (default true) |
| -backup_storage_hook | string   | If set, we send the contents of the backup files through this hook |
| -backup_storage_implementation | string   | Which implementation to use for the backup storage feature |
| -backup_storage_number_blocks | int      | If backup_storage_compress is true, backup_storage_number_blocks sets the number of blocks that can be processed at once, before the writer blocks, during compression. It should be equal to the number of CPUs available for compression (default 2) |
| -binlog_player_protocol | string | The protocol to download binlogs from a vttablet (default "grpc") |
| -binlog_use_v3_resharding_mode | boolean | True if the binlog streamer should use V3-style sharding, which doesn't require a preset sharding key column (default true) |
| -catch-sigpipe | boolean | Catch and ignore SIGPIPE on stdout and stderr if specified |
| -cell | string   | Cell to use |
| -ceph_backup_storage_config | string   | Path to JSON config file for ceph backup storage (default "ceph_backup_config.json") |
| -consul_auth_static_file | string   | JSON File to read the topos/tokens from |
| -cpu_profile | string   | Deprecated: use '-pprof=cpu' instead |
| -datadog-agent-host | string   | Host to send spans to; if empty, no tracing will be done |
| -datadog-agent-port | string   | Port to send spans to; if empty, no tracing will be done |
| -durability_policy | string   | Type of durability to enforce. Other values are dictated by registered plugins (default "none") |
| -db-credentials-file | string | db credentials file; send SIGHUP to reload this file |
| -db-credentials-server | string | db credentials server type (use 'file' for the file implementation) (default "file") |
| -dba_idle_timeout | duration | Idle timeout for dba connections (default 1m0s) |
| -dba_pool_size | int | Size of the connection pool for dba connections (default 20) |
| -discovery_high_replication_lag_minimum_serving | duration | The replication lag that is considered too high when selecting the minimum num vttablets for serving (default 2h0m0s) |
| -discovery_low_replication_lag | duration | The replication lag that is considered low enough to be healthy (default 30s) |
| -emit_stats | boolean | True if we should emit stats to push-based monitoring/stats backends |
| -enable-consolidator | boolean | This option enables the query consolidator (default true) |
| -enable-consolidator-replicas | boolean | This option enables the query consolidator only on replicas |
| -enable-query-plan-field-caching | boollean | This option fetches & caches fields (columns) when storing query plans (default true) |
| -enable-tx-throttler | boolean | If true, replication-lag-based throttling on transactions will be enabled |
| -enable_hot_row_protection | boolean | If true, incoming transactions for the same row (range) will be queued and cannot consume all txpool slots |
| -enable_hot_row_protection_dry_run | boolean | If true, hot row protection is not enforced but logs if transactions would have been queued |
| -enable_queries | boolean | If set, allows vtgate and vttablet queries. May have security implications, as the queries will be run from this process |
| -enable_realtime_stats | boolean | Required for the Realtime Stats view. If set, vtctld will maintain a streaming RPC to each tablet (in all cells) to gather the realtime health stats |
| -enable_transaction_limit | boolean | If true, limit on number of transactions open at the same time will be enforced for all users. User trying to open a new transaction after exhausting their limit will receive an error immediately, regardless of whether there are available slots or not |
| -enable_transaction_limit_dry_run | boolean | If true, limit on number of transactions open at the same time will be tracked for all users, but not enforced |
| -enforce_strict_trans_tables | boolean | If true, vttablet requires MySQL to run with STRICT_TRANS_TABLES or STRICT_ALL_TABLES on. It is recommended to not turn this flag off. Otherwise MySQL may alter your supplied values before saving them to the database (default true) |
| -file_backup_storage_root | string   | Root directory for the file backup storage |
| -gcs_backup_storage_bucket | string   | Google Cloud Storage bucket to use for backups |
| -gcs_backup_storage_root | string   | Root prefix for all backup-related object names |
| -grpc_auth_mode | string   | Which auth plugin implementation to use (eg: static) |
| -grpc_auth_mtls_allowed_substrings | string   | List of substrings of at least one of the client certificate names (separated by colon) |
| -grpc_auth_static_client_creds | string   | When using grpc_static_auth in the server, this file provides the credentials to use to authenticate with the server |
| -grpc_auth_static_password_file | string   | JSON File to read the users/passwords from |
| -grpc_ca | string   | Server CA to use for gRPC connections, requires TLS, and enforces client certificate check |
| -grpc_cert | string   | Server certificate to use for gRPC connections, requires grpc_key, enables TLS |
| -grpc_compression | string   | Which protocol to use for compressing gRPC. Supported: snappy (default: nothing) |
| -grpc_crl | string   | Path to a certificate revocation list in PEM format, client certificates will be further verified against this file during TLS handshake |
| -grpc_enable_optional_tls | boolean | Enable optional TLS mode when a server accepts both TLS and plain-text connections on the same port |
| -grpc_enable_tracing | boolean | Enable gRPC tracing |
| -grpc_initial_conn_window_size | int      | gRPC initial connection window size |
| -grpc_initial_window_size | int      | gRPC initial window size |
| -grpc_keepalive_time | duration | After a duration of this time, if the client doesn't see any activity, it pings the server to see if the transport is still alive (default 10s) |
| -grpc_keepalive_timeout | duration | After having pinged for keepalive check, the client waits for a duration of Timeout and if no activity is seen even after that, the connection is closed (default 10s) |
| -grpc_key | string   | Server private key to use for gRPC connections, requires grpc_cert, enables TLS |
| -grpc_max_connection_age | duration | Maximum age of a client connection before GoAway is sent (default 2562047h47m16.854775807s) |
| -grpc_max_connection_age_grace | duration | Additional grace period after grpc_max_connection_age, after which connections are forcibly closed (default 2562047h47m16.854775807s) |
| -grpc_max_message_size | int      | Maximum allowed RPC message size. Larger messages will be rejected by gRPC with the error 'exceeding the max size' (default 16777216) |
| -grpc_port | int      | Port to listen on for gRPC calls |
| -grpc_prometheus | boolean | Enable gRPC monitoring with Prometheus |
| -grpc_server_ca | string   | Path to server CA in PEM format, which will be combine with server cert, return full certificate chain to clients |
| -grpc_server_initial_conn_window_size | int      | gRPC server initial connection window size |
| -grpc_server_initial_window_size | int      | gRPC server initial window size |
| -grpc_server_keepalive_enforcement_policy_min_time | duration | gRPC server minimum keepalive time (default 10s) |
| -grpc_server_keepalive_enforcement_policy_permit_without_stream | boolean | gRPC server permit client keepalive pings even when there are no active streams (RPCs) |
| -heartbeat_enable | boolean | If true, vttablet records (if primary) or checks (if replica) the current time of a replication heartbeat in the table _vt.heartbeat. The result is used to inform the serving state of the vttablet via healthchecks |
| -heartbeat_interval | duration | How frequently to read and write replication heartbeat (default 1s) |
| -hot_row_protection_concurrent_transactions | int | Number of concurrent transactions let through to the txpool/MySQL for the same hot row. Should be > 1 to have enough 'ready' transactions in MySQL and benefit from a pipelining effect (default 5) |
| -hot_row_protection_max_global_queue_size | int | Global queue limit across all row (ranges). Useful to prevent that the queue can grow unbounded (default 1000) |
| -hot_row_protection_max_queue_size | int | Maximum number of BeginExecute RPCs which will be queued for the same row (range) (default 20) |
| -jaeger-agent-host | string   | Host and port to send spans to; if empty, no tracing will be done |
| -keep_logs | duration | Keep logs for this long (using ctime) (zero to keep forever) |
| -keep_logs_by_mtime | duration | Keep logs for this long (using mtime) (zero to keep forever) |
| -lameduck-period | duration | Keep running at least this long after SIGTERM before stopping (default 50ms) |
| -legacy_replication_lag_algorithm | boolean | use the legacy algorithm when selecting the vttablets for serving (default true) |
| -log_backtrace_at | value    | When logging hits line file:N, emit a stack trace |
| -log_dir | string   | If non-empty, write log files in this directory |
| -log_err_stacks | boolean | Log stack traces for errors |
| -log_rotate_max_size | uint     | Size in bytes at which logs are rotated (glog.MaxSize) (default 1887436800) |
| -logtostderr | boolean | Log to standard error instead of files |
| -mem-profile-rate | int      | Deprecated: use '-pprof=mem' instead (default 524288) |
| -min_number_serving_vttablets | int | The minimum number of vttablets that will be continue to be used even with low replication lag (default 2) |
| -mutex-profile-fraction | int      | Deprecated: use '-pprof=mutex' instead |
| -mysql_auth_server_static_file | string | JSON File to read the users/passwords from. |
| -mysql_auth_server_static_string | string | JSON representation of the users/passwords config. |
| -mysql_auth_static_reload_interval | duration | Ticker to reload credentials |
| -onclose_timeout | duration | Wait no more than this for OnClose handlers before stopping (default 1ns) |
| -online_ddl_check_interval | duration | Interval polling for new online DDL requests (default 1m0s) |
| -onterm_timeout | duration | Wait no more than this for OnTermSync handlers before stopping (default 10s) |
| -opentsdb_uri | string   | URI of opentsdb /api/put method |
| -pid_file | string   | If set, the process will write its pid to the named file, and delete it on graceful shutdown |
| -pool_hostname_resolve_interval | duration | If set force an update to all hostnames and reconnect if changed, defaults to 0 (disabled) |
| -port | int      | Port for the server |
| -pprof | string   | Enable profiling |
| -proxy_tablets | boolean | Setting this true will make vtctld proxy the tablet status instead of redirecting to them |
| -purge_logs_interval | duration | How often to try to remove old logs (default 1h0m0s) |
| -query-log-stream-handler | string | URL handler for streaming queries log (default "/debug/querylog") |
| -querylog-filter-tag | string | String that must be present in the query as a comment for the query to be logged, works for both vtgate and vttablet |
| -querylog-format | string | Gormat for query logs ("text" or "json") (default "text") |
| -redact-debug-ui-queries | boolean | Redact full queries and bind variables from debug UI |
| -remote_operation_timeout | duration | Time to wait for a remote operation (default 30s) |
| -s3_backup_aws_endpoint | string   | Endpoint of the S3 backend (region must be provided) |
| -s3_backup_aws_region | string   | AWS region to use (default "us-east-1") |
| -s3_backup_aws_retries | int      | AWS request retries (default -1) |
| -s3_backup_force_path_style | boolean | Force the S3 path style |
| -s3_backup_log_level | string   | Determine the S3 loglevel to use from LogOff, LogDebug, LogDebugWithSigning, LogDebugWithHTTPBody, LogDebugWithRequestRetries, LogDebugWithRequestErrors (default "LogOff") |
| -s3_backup_server_side_encryption | string   | Server-side encryption algorithm (e.g., AES256, aws:kms, sse_c:/path/to/key/file) |
| -s3_backup_storage_bucket | string   | S3 bucket to use for backups |
| -s3_backup_storage_root | string   | Root prefix for all backup-related object names |
| -s3_backup_tls_skip_verify_cert | boolean | Skip the 'certificate is valid' check for SSL connections |
| -schema_change_check_interval | int | This value decides how often we check schema change dir, in seconds (default 60) |
| -schema_change_controller | string | Schema change controller is responsible for finding schema changes and responding to schema change events |
| -schema_change_dir | string | Directory contains schema changes for all keyspaces. Each keyspace has its own directory and schema changes are expected to live in '$KEYSPACE/input' dir. e.g. test_keyspace/input/*sql, each SQL file represents a schema change |
| -schema_change_replicas_timeout | duration | How long to wait for replicas to receive the schema change (default 10s) |
| -schema_change_user | string | The user who submits this schema change. |
| -schema_swap_admin_query_timeout | duration | Timeout for SQL queries used to save and retrieve meta information for schema swap process (default 30s) |
| -schema_swap_backup_concurrency | int | Number of simultaneous compression/checksum jobs to run for seed backup during schema swap (default 4) |
| -schema_swap_delay_between_errors | duration | Time to wait after a retryable error happened in the schema swap process (default 1m0s) |
| -schema_swap_reparent_timeout | duration | Timeout to wait for replicas when doing reparent during schema swap (default 30s) |
| -security_policy | string   | The name of a registered security policy to use for controlling access to URLs - empty means allow all for anyone (built-in policies: deny-all, read-only) |
| -service_map | value    | Comma separated list of services to enable (or disable if prefixed with '-') Example: grpc-vtworker |
| -sql-max-length-errors | int | Truncate queries in error logs to the given length (default unlimited) |
| -sql-max-length-ui | int | Truncate queries in debug UIs to the given length (default 512) (default 512) |
| -srv_topo_cache_refresh | duration | How frequently to refresh the topology for cached entries (default 1s) |
| -srv_topo_cache_ttl | duration | How long to use cached entries for topology (default 1s) |
| -stats_backend | string   | The name of the registered push-based monitoring/stats backend to use |
| -stats_combine_dimensions | string   | List of dimensions to be combined into a single "all" value in exported stats vars |
| -stats_common_tags | string   | Comma-separated list of common tags for the stats backend. It provides both label and values. Example: label1:value1,label2:value2 |
| -stats_drop_variables | string   | Variables to be dropped from the list of exported variables |
| -stats_emit_period | duration | Interval between emitting stats to all registered backends (default 1m0s) |
| -stderrthreshold | value    | Logs at or above this threshold go to stderr (default 1) |
| -tablet_dir | string | The directory within the vtdataroot to store vttablet/mysql files. Defaults to being generated by the tablet uid. |
| -tablet_grpc_ca | string | The server ca to use to validate servers when connecting |
| -tablet_grpc_cert | string | The cert to use to connect |
| -tablet_grpc_key | string | The key to use to connect |
| -tablet_grpc_server_name | string | The server name to use to validate server certificate |
| -tablet_health_keep_alive | duration | Close streaming tablet health connection if there are no requests for this long (default 5m0s) |
| -tablet_manager_grpc_ca | string | The server ca to use to validate servers when connecting |
| -tablet_manager_grpc_cert | string | The cert to use to connect |
| -tablet_manager_grpc_concurrency | int | Concurrency to use to talk to a vttablet server for performance-sensitive RPCs (like ExecuteFetchAs{Dba,AllPrivs,App}) (default 8) |
| -tablet_manager_grpc_key | string | The key to use to connect |
| -tablet_manager_grpc_server_name | string | The server name to use to validate server certificate |
| -tablet_manager_protocol | string | The protocol to use to talk to vttablet (default "grpc") |
| -tablet_protocol | string | How to talk to the vttablets (default "grpc") |
| -tablet_url_template | string | Format string describing debug tablet url formatting. See the Go code for getTabletDebugURL() how to customize this. (default "http://{{.GetTabletHostPort}}") |
| -throttler_client_grpc_ca | string | The server ca to use to validate servers when connecting |
| -throttler_client_grpc_cert | string | The cert to use to connect |
| -throttler_client_grpc_key | string | The key to use to connect |
| -throttler_client_grpc_server_name | string | The server name to use to validate server certificate |
| -throttler_client_protocol | string | The protocol to use to talk to the integrated throttler service (default "grpc") |
| -topo_consul_watch_poll_duration | duration | Time of the long poll for watch queries (default 30s) |
| -topo_etcd_lease_ttl | int | Lease TTL for locks and leader election. The client will use KeepAlive to keep the lease going (default 30) |
| -topo_etcd_tls_ca | string | Path to the CA to use to validate the server cert when connecting to the etcd topo server |
| -topo_etcd_tls_cert | string | Path to the client cert to use to connect to the etcd topo server, requires topo_etcd_tls_key, enables TLS |
| -topo_etcd_tls_key | string | Path to the client key to use to connect to the etcd topo server, enables TLS |
| -topo_global_root | string | The path of the global topology data in the global topology server |
| -topo_global_server_address | string | The address of the global topology server |
| -topo_implementation | string | The topology implementation to use |
| -topo_k8s_context | string | The kubeconfig context to use, overrides the 'current-context' from the config |
| -topo_k8s_kubeconfig | string | Path to a valid kubeconfig file. |
| -topo_k8s_namespace | string | The kubernetes namespace to use for all objects. Default comes from the context or in-cluster config |
| -topo_zk_auth_file | string | Auth to use when connecting to the zk topo server, file contents should be <scheme>:<auth>, e.g., digest:user:pass |
| -topo_zk_base_timeout | duration | zk base timeout (see zk.Connect) (default 30s) |
| -topo_zk_max_concurrency | int | Maximum number of pending requests to send to a Zookeeper server (default 64) |
| -topo_zk_tls_ca | string | The server CA to use to validate servers when connecting to the zk topo server |
| -topo_zk_tls_cert | string | The cert to use to connect to the zk topo server, requires topo_zk_tls_key, enables TLS |
| -topo_zk_tls_key | string | The key to use to connect to the zk topo server, enables TLS |
| -tracer | string   | Tracing service to use (default "noop") |
| -tracing-enable-logging | boolean | Whether to enable logging in the tracing service |
| -tracing-sampling-rate | value    | Sampling rate for the probabilistic jaeger sampler (default 0.1) |
| -tracing-sampling-type | value    | Sampling strategy to use for jaeger. Possible values are 'const', 'probabilistic', 'rateLimiting', or 'remote' (default const) |
| -transaction-log-stream-handler | string | URL handler for streaming transactions log (default "/debug/txlog") |
| -transaction_limit_by_component | boolean | Include CallerID.component when considering who the user is for the purpose of transaction limit |
| -transaction_limit_by_principal | boolean | Include CallerID.principal when considering who the user is for the purpose of transaction limit (default true) |
| -transaction_limit_by_subcomponent | boolean | Include CallerID.subcomponent when considering who the user is for the purpose of transaction limit |
| -transaction_limit_by_username | boolean | Include VTGateCallerID.username when considering who the user is for the purpose of transaction limit (default true) |
| -transaction_limit_per_user | float | Maximum number of transactions a single user is allowed to use at any time, represented as fraction of -transaction_cap (default 0.4) |
| -transaction_shutdown_grace_period | int | How long to wait (in seconds) for transactions to complete during graceful shutdown |
| -twopc_abandon_age | float | Time in seconds. Any unresolved transaction older than this time will be sent to the coordinator to be resolved |
| -twopc_coordinator_address | string | Address of the (VTGate) process(es) that will be used to notify of abandoned transactions |
| -twopc_enable | boolean | If the flag is on, 2pc is enabled. Other 2pc flags must be supplied |
| -tx-throttler-config | string | The configuration of the transaction throttler as a text formatted throttlerdata.Configuration protocol buffer message (default "target_replication_lag_sec: 2 max_replication_lag_sec: 10 initial_rate: 100 max_increase: 1 emergency_decrease: 0.5 min_duration_between_increases_sec: 40 max_duration_between_increases_sec: 62 min_duration_between_decreases_sec: 20 spread_backlog_across_sec: 20 age_bad_rate_after_sec: 180 bad_rate_increase: 0.1 max_rate_approach_threshold: 0.9 ") |
| -tx-throttler-healthcheck-cells | value | A comma-separated list of cells. Only tabletservers running in these cells will be monitored for replication lag by the transaction throttler |
| -v | value    | Log level for V logs |
| -version | boolean | Print binary version |
| -vmodule | value    | Comma-separated list of pattern=N settings for file-filtered logging |
| -vstream_dynamic_packet_size | boolean | Enable dynamic packet sizing for VReplication. This will adjust the packet size during replication to improve performance (default true) |
| -vstream_packet_size | int      | Suggested packet size for VReplication streamer. This is used only as a recommendation. The actual packet size may be more or less than this amount (default 250000) |
| -vtctl_client_protocol | string   | The protocol to use to talk to the vtctl server (default "grpc") |
| -vtctl_healthcheck_retry_delay | duration | Delay before retrying a failed healthcheck (default 5s) |
| -vtctl_healthcheck_timeout | duration | The health check timeout period (default 1m0s) |
| -vtctl_healthcheck_topology_refresh | duration | Refresh interval for re-reading the topology (default 30s) |
| -vtctld_show_topology_crud | boolean | Controls the display of the CRUD topology actions in the vtctld UI (default true) |
| -vtgate_grpc_ca | string | The server CA to use to validate servers when connecting |
| -vtgate_grpc_cert | string | The cert to use to connect |
| -vtgate_grpc_key | string | The key to use to connect |
| -vtgate_grpc_server_name | string | The server name to use to validate server certificate |
| -vtgate_protocol | string | how to talk to vtgate (default "grpc") |
| -vtworker_client_grpc_ca | string   | The server CA to use to validate servers when connecting |
| -vtworker_client_grpc_cert | string   | The cert to use to connect |
| -vtworker_client_grpc_crl | string   | The server crl to use to validate server certificates when connecting |
| -vtworker_client_grpc_key | string   | The key to use to connect |
| -vtworker_client_grpc_server_name | string   | The server name to use to validate server certificate |
| -vtworker_client_protocol | string   | The protocol to use to talk to the vtworker server (default "grpc") |
| -wait_for_drain_sleep_rdonly | duration | Time to wait before shutting the query service on old RDONLY tablets during MigrateServedTypes (default 5s) |
| -wait_for_drain_sleep_replica | duration | Time to wait before shutting the query service on old REPLICA tablets during MigrateServedTypes (default 15s) |
| -watch_replication_stream | boolean | When enabled, vttablet will stream the MySQL replication stream from the local server, and use it to update schema when it sees a DDL |
| -web_dir | string   | NOT USED, here for backward compatibility |
| -web_dir2 | string   | NOT USED, here for backward compatibility |
| -workflow_manager_disable | value    | Comma separated list of workflow types to disable |
| -workflow_manager_init | boolean | Initialize the workflow manager in this vtctld instance |
| -workflow_manager_use_election | boolean | If specified, will use a topology server-based master election to ensure only one workflow manager is active at a time |
| -xbstream_restore_flags | string   | Flags to pass to xbstream command during restore. These should be space separated and will be added to the end of the command. These need to match the ones used for backup e.g. --compress / --decompress, --encrypt / --decrypt |