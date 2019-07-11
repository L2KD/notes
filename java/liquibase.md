# Liquibase

Một dạng version control for DB.

Trong maven proj, các plugin:

**Diff**: Scan code các entity hiện tại (hibernate) và DB thực tế để generate ra các file gọi là changlog.xml (named theo kiểu `timestamp_changelog.xml`).

Các file này sau khi được tạo ra sẽ add vào master.xml để liquibase thực hiện update vào DB.

## Data

Trong `master.xml`, bên trong tag `<changeSet>`, thêm đoạn

```xml
<loadData
    file="config/liquibase/data/huong_dieu_tri.csv"
    separator=";"
    tableName="huong_dieu_tri">
    <column name="code" type="smallint"/>
    <column name="ten" type="varchar(500)"/>
    <column name="noi_tru" type="smallint"/>
</loadData>
```

Thêm file `huong_dieu_tri.csv`

```csv
1;Cấp toa cho về;0
2;Cấp toa và hẹn tái khám;0
3;Chuyển phòng;0
4;Chuyển tuyến;0
5;Nhập viện;0
6;Không toa;0
7;Hội chẩn;0
8;Điều trị bệnh án ngoại trú;0
10;Xuất viện;1
11;Tử vong;1
12;Chuyển tuyến;1
13;Lập phiếu chăm sóc tờ điều trị;1
14;Chuyển khoa;1
15;Hội chẩn;1
```

Sau đó chạy plugin liquibase:update
