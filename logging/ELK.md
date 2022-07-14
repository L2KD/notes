# ELK/EFK stack

(E)lastic, (L)ogstash, (K)ibana

Logstash để gom log, xào nấu nó theo các format mình muốn, đổ dữ liệu vào Elastic. Elastic sẽ index, phân tách và hỗ trợ tìm kiếm nhanh. Kibana để hiển thị nó dưới dạng trực quan hơn (trực quan hóa dữ liệu).

## Logstash

Logstash có các khái niệm: Input, Filter, Output.

Input là thành phần tiếp nhận dữ liệu.

Output là thành phần đẩy dữ liệu đi.

Filter là thành phần để xào nấu dữ liệu cho đúng format theo ý muốn.

Các input thường gặp: stdin (nhận trực tiếp data từ stdin của terminal), filebeat (đọc 1 file và gửi lên logstash).

Các output thường gặp: stdout, elastic, mongodb...

## Elastic

### Elastic agent

Khi cài elastic agent, nó sẽ cài các thứ như filebeat và metricbeat. Sau đó cấu hình như systemd service và enable lên.

### Beats

Beat là các shipper, để đẩy dữ liệu vào Elastic stack

Các beat tiêu biểu:

- Auditbeat 	Collect your Linux audit framework data and monitor the integrity of your files.
- Filebeat 	Tails and ships log files
- Functionbeat 	Read and ships events from serverless infrastructure.
- Heartbeat 	Ping remote services for availability
- Metricbeat 	Fetches sets of metrics from the operating system and services
- Packetbeat 	Monitors the network and applications by sniffing packets
- Winlogbeat 	Fetches and ships Windows Event logs
- Osquerybeat 	Runs Osquery and manages interraction with it.

## Filebeat

Filebeat là tool để nó tail và send file log lên logstash. Cài filebeat vào máy cần gom log:

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.3.1-linux-x86_64.tar.gz
tar xzvf filebeat-8.3.1-linux-x86_64.tar.gz
```

Để gom log, hoặc dùng module có sẵn, hoặc cấu hình bằng tay.

### Xem các modules có sẵn:

```
./filebeat modules list
```

```
./filebeat modules enable mongodb
Enabled mongodb
```

Sau khi enable mongo. Vào chỉnh sửa

    modules.d/mongo.yml

Chỉnh lại các thứ:

    - module: mongodb
    # All logs
    log:
        enabled: true

        # Set custom paths for the log files. If left empty,
        # Filebeat will choose the paths depending on your OS.
        #var.paths:
        var.paths: ["/var/log/mongodb/mongod.log"]

Setup

    ./filebeat setup -e

Trên kibana, thêm mới index

- Name filebeat-mongodb
- index pattern  filebeat-*
- Tạo data stream.
- Component template: none
- Index settings:
    ```
    {
    "index": {
        "lifecycle": {
        "name": "filebeat"
        },
        "mapping": {
        "total_fields": {
            "limit": "10000"
        }
        },
        "refresh_interval": "5s",
        "number_of_shards": "1",
        "max_docvalue_fields_search": "200",
        "query": {
        "default_field": [
            "message",
            "tags",
            "agent.ephemeral_id",
            "agent.id",
            "agent.name",
            "agent.type",
            "agent.version",
            "error.code",
            "error.id",
            "error.message",
            "error.stack_trace",
            "error.type",
            "event.action",
            "event.category",
            "event.code",
            "event.dataset",
            "event.hash",
            "event.id",
            "event.kind",
            "event.module",
            "event.outcome",
            "event.provider",
            "event.timezone",
            "event.type",
            "host.architecture",
            "host.geo.city_name",
            "host.geo.continent_name",
            "host.geo.country_iso_code",
            "host.geo.country_name",
            "host.geo.name",
            "host.geo.region_iso_code",
            "host.geo.region_name",
            "host.hostname",
            "host.id",
            "host.mac",
            "host.name",
            "host.os.family",
            "host.os.full",
            "host.os.kernel",
            "host.os.name",
            "host.os.platform",
            "host.os.version",
            "host.type",
            "http.request.body.content",
            "http.request.method",
            "http.request.referrer",
            "http.response.body.content",
            "http.version",
            "log.level",
            "log.logger",
            "log.origin.file.name",
            "log.origin.function",
            "organization.id",
            "organization.name",
            "os.family",
            "os.full",
            "os.kernel",
            "os.name",
            "os.platform",
            "os.version",
            "process.args",
            "process.executable",
            "process.hash.md5",
            "process.hash.sha1",
            "process.hash.sha256",
            "process.hash.sha512",
            "process.name",
            "process.thread.name",
            "process.title",
            "process.working_directory",
            "server.address",
            "server.as.organization.name",
            "server.domain",
            "server.geo.city_name",
            "server.geo.continent_name",
            "server.geo.country_iso_code",
            "server.geo.country_name",
            "server.geo.name",
            "server.geo.region_iso_code",
            "server.geo.region_name",
            "server.mac",
            "server.registered_domain",
            "server.top_level_domain",
            "server.user.domain",
            "server.user.email",
            "server.user.full_name",
            "server.user.group.domain",
            "server.user.group.id",
            "server.user.group.name",
            "server.user.hash",
            "server.user.id",
            "server.user.name",
            "service.ephemeral_id",
            "service.id",
            "service.name",
            "service.node.name",
            "service.state",
            "service.type",
            "service.version",
            "host.os.build",
            "host.os.codename",
            "process.owner.id",
            "process.owner.name.text",
            "process.owner.name",
            "object.key",
            "fields.*"
        ]
        }
    }
    }
    ```
- Mappings fiels
    - @timestamp Date
    - mongodb Object
        - log Object
            - component Keyword
            - context Keyword
            - id Long

Run

    sudo ./filebeat -e --strict.perms=false

### Manually

Sau khi tải và giải nén filebeat. Xem yml của nó.

Config output

    - type: filestream
    id: mongodb-dump
    enabled: true
    paths:
        - "/home/admin/dump-*.log"

Type filestream, trước đây là `log`. Nó sẽ upload file này lên ES hoặc Logstash.

Fleet