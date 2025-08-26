# Telegraf

## Configuration

```toml
[global_tags]
    dc = "us-east-1"
    user= "$USER" # Environment variable
[agent]
    interval = "60s"
    round_interval = true
    metric_batch_size = 1000
    metric_buffer_limit = 10000
    collection_jittser = "0s"
    collection_offset = "6m"
[[inputs.file]]
    files = ["/data/temp.out"]
    data_format = "influx"
    [inputs.file.tags]
        sensor_type = "thermostat"
[[inputs.file]]
     files = ["data/wheather.csv", "/data/location.csv"]
     data_format = "csv"
[[inputs.file]]
    files = ["data/sample.json"]
    data_format = "json"
    tag_keys = ["Factory", "Machine"]
[[inputs.cpu]]
    percpu = true
    totalcpu = true
    collect_cpu_time = false
    report_active = false
    fielddrop = ["time_*"]
[[inputs.disk]]
    mount_points = ["/"]
    ignore_fs = ["tmpfs", "devtmpfs", "devfs", "iso9660", "overlay", "aufs", "squashfs"] # ignore mount points by filesystem type
[[outputs.influxdb_v2]]
    urls = ["http://localhost:8086"]
    token = "secret"
    organization = "influxData"
    bucket = "demo"
    precision = "1s"
```

### Commands

```shell
# Generate Sample Config
telegraf --sample-config > .telegraf.conf

# Generate new configuration with specific entries
telegraf --sample-config --input-filter cpu:mem:net:swap --processor-filter : --aggregator-filter : --output-filter influxdb:kafka > ./telegraf.conf

# Use Local configuration file
telegraf --config /etc/telegraf/telegraf.conf

# Load Configuration from remote URL
telegraf --config "http://remote-URL-endpoint"

# Specify directory containing configuration file(s)
telegraf --config-directory /etc/telegraf/telegraf.d
```

### Metric Filtering

* Selectors
  * namepass / namedrop
  * tagpass / tagdrop
* Modifiers
  * fieldpass / fielddrop
  * taginclude / tagexclude