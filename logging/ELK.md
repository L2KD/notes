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

### Manually

Sau khi tải và giải nén filebeat. Xem yml của nó.

Config output



Fleet