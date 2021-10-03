
1. Setup primary database.

primary postgres.conf

```
synchronous_standby_names = '*'	# standby servers that provide sync rep
listen_addresses = '*'
wal_level = replica			# minimal, replica, or logical
```

primary pg_hba.conf

```
host    replication     postgres        all                     trust
```

2. Generate postgres.conf on standby.

```
pg_basebackup -R -D ${PGDATA} -h 192.168.56.10
```

standby postgres.conf

```
primary_conninfo = 'user=postgres passfile=''/root/.pgpass'' channel_binding=prefer host=primary port=5432 sslmode=prefer sslcompression=0 ssl_min_protocol_version=TLSv1.2 gssencmode=prefer krbsrvname=postgres target_session_attrs=any'
hot_standby = on			# "off" disallows queries during recovery
```
