# fluent-plugin-filter-docker_metadata


##Usage
To use this plugin the following config can be used

```
<source>
  @type tail
  read_from_head true
  path /docker/containers/*/*-json.log
  pos_file /tmp/fluentd-docker.pos
  time_format %Y-%m-%dT%H:%M:%S
  tag docker.var.lib.docker.containers.*.*.log
  format json
</source>

<filter docker.**>
  @type docker_metadata
  container_id_regexp  (\w+)(-json.log.*)
</filter>

<filter docker.**>
  @type record_transformer
  <record>
    hostname ${record["container_name"]}
    message ${record["container_name"]}: ${record["log"]}
  </record>
</filter>
```

If you are running this in a container you will need to add the below filter before your output plugin  (assuming your container hostname is fluent

```
<filter docker.**>
  @type grep
  <exclude>
    key hostname
    pattern /fluentd/
  </exclude>
</filter>
```
