```
[mysqld]
basedir=/opt/mysql
datadir=/opt/mysql/data
socket=/tmp/mysql.sock
log-error=/opt/mysql/error.log
pid-file=/opt/mysql/mysql.pid

sql_mode = "NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
server-id=1
log-bin=log
binlog-do-db=xxx
binlog-ignore-db=mysql
binlog-ignore-db=sys
binlog-ignore-db=information_schema
binlog-ignore-db=performance_schema
replicate-ignore-table = mysql.user

sync_binlog = 2

character-set-server = utf8mb4
skip_name_resolve = 1
open_files_limit    = 65535
back_log = 1024
max_connections = 5000
max_connect_errors = 1000000
table_open_cache = 600
table_definition_cache = 600
table_open_cache_instances = 64
thread_stack = 512K
external-locking = FALSE
max_allowed_packet = 32M
sort_buffer_size = 4M
join_buffer_size = 4M
thread_cache_size = 7500
interactive_timeout = 600
wait_timeout = 600

tmp_table_size = 256M
max_heap_table_size = 256M

query_cache_size=256M
query_cache_type=1

slow_query_log = 1
slow_query_log_file = /opt/mysql/slow.log
long_query_time = 0.1

key_buffer_size = 32M
read_buffer_size = 8M
read_rnd_buffer_size = 4M
bulk_insert_buffer_size = 64M

innodb_buffer_pool_size = 5734M
innodb_buffer_pool_instances = 8
innodb_buffer_pool_load_at_startup = 1
innodb_buffer_pool_dump_at_shutdown = 1
innodb_data_file_path = ibdata1:1G:autoextend
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 32M
innodb_log_file_size = 2G
innodb_log_files_in_group = 2
innodb_max_undo_log_size = 4G
innodb_undo_directory = undolog
innodb_undo_tablespaces = 95

innodb_thread_concurrency = 0
innodb_sync_spin_loops = 100
innodb_spin_wait_delay = 30

[mysqldump]
quick
max_allowed_packet = 32M
```
