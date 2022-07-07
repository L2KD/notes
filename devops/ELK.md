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

Có các plugin.

Fleet