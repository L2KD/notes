# Hibernate

Sử dụng enum trong hibernate

Ví dụ có enum sau:

```java
public enum Tuyen {
    TUYEN1,
    TUYEN2,
    TUYEN3,
    TUYEN4
}
```

Và class

```java
@Entity
public class DonVi {
    private Tuyen tuyen;
    ...
}
```

Khi này trong DB sẽ lưu dạng 

Ma_DV | Ten_DV | Tuyen
---|---|---
0001 | Don vi 1 | TUYEN1
0002 | Don vi 2 | TUYEN2

Thì các file data để init cũng phải có dạng

    ma_dv;ten_dv;tuyen
    0001;Don vi 1;TUYEN1
    0002;Don vi 2;TUYEN2
    
Đồng thời schema definition cũng phải có dạng

    <!-- Liquibase XML -->
    <Column name="tuyen" type="string" />
    
Để thay đổi lại dạng Tuyen (1,2,3,4,5...). Chúng ta cần 1 converter để convert từ data trong DB về kiểu enum tuỳ chọn trong java theo ý muốn mà không cần bó buộc theo chuyện tự đoonjg của hibernate.

Thêm converter

```java
@Converter
public class TuyenAttributeConverter implements AttributeConverter<Tuyen, Integer> {
    @Override
    public Integer convertToDatabaseColumn(Tuyen attribute) {
        if (attribute == null)
        return null;

    switch (attribute) {
    case TUYEN1:
        return 1;

    case TUYEN2:
        return 2;

    case TUYEN3:
        return 3;

    case TUYEN4:
        return 4;

    default:
        throw new IllegalArgumentException(attribute + " not supported.");
    }
    }

    @Override
    public Tuyen convertToEntityAttribute(Integer dbData) {
        if (dbData == null)
            return null;

        switch (dbData) {
        case 1:
            return Tuyen.TUYEN1;

        case 2:
            return Tuyen.TUYEN2;

        case 3:
            return Tuyen.TUYEN3;

        case 4:
            return Tuyen.TUYEN4;

        default:
            throw new IllegalArgumentException(dbData + " not supported.");
        }
    }
}
```

Class DonVi:

```java
@Entity
public class DonVi {
    @Enumerated(EnumType.ORDINAL)
    @Convert(converter = TuyenAttributeConverter.class)
    private Tuyen tuyen;
}
```
    
Khi này DB sẽ có dạng sau:

Ma_DV | Ten_DV | Tuyen
---|---|---
0001 | Don vi 1 | 1
0002 | Don vi 2 | 2

Mặc dù khi lấy dữ liệu lên java, object sẽ get được theo enum type: Tuyen.TUYEN1

Ở class DonVi, type ordinal để báo hiệu rằng DB dưới DB lưu kiểu numeric.

Liquibase:

    <!-- Liquibase XML -->
    <Column name="tuyen" type="numeric" />

Init data:

    ma_dv;ten_dv;tuyen
    0001;Don vi 1;1
    0002;Don vi 2;2
    
Đọc thêm: https://thorben-janssen.com/hibernate-enum-mappings/

## Cách viết câu select count, group by bằng hibernate, jpa

Đoạn này viết gấp, xem ý tưởng thôi nhé

Cách 1:

        CriteriaBuilder cb = em.getCriteriaBuilder();
        CriteriaQuery<Tuple> q = cb.createTupleQuery();
        Root<DoiTuong> r = q.from(DoiTuong.class);
        q.multiselect(r.get("phuongXaNlv.id"), cb.count(r));
        r.join("phuongXaNlv", JoinType.INNER);
        q.groupBy(r.get("phuongXaNlv"));

        TypedQuery<Tuple> t = em.createQuery(q);
        List<Tuple> resultList = t.getResultList();

        for(Tuple tuple : resultList){
            logger.info(tuple.get(0).toString() );
        }
        
Đoạn trên tương đương với

    select count(dt.id), px.ten
    from doi_tuong dt
    inner join phuong_xa px on px.id = dt.phuong_xa_noio_id
    where noi_dieu_tri_id in (307, 413)
    and phuong_xa_noio_id in (
        select p.id
        from phuong_xa p
        where p.quan_huyen_id = 815
    )
    group by px.te

Cách 2:

    @Repository
    public interface DoiTuongRepository extends JpaRepository<DoiTuong, Long>, JpaSpecificationExecutor<DoiTuong> {
        Integer countAllByNoiDieuTri_IdIn(List<Long> ids);
     }
     
 Cách 3:
 
    public class BaoCaoF0TheoPhuongXa {
        private String phuongXa;
        private Long soLuong;
    // Getter, setter, constructors
    }

 
     @Query("SELECT new vn.vnpt.domain.BaoCaoF0TheoPhuongXa(px.ten, COUNT(dt))" +
    " FROM DoiTuong dt " +
    " inner join PhuongXa px on px.id = dt.phuongXaNoiO.id" +
    " where dt.noiDieuTri.id in (:noiDtTaiNhaIds)" +
    " and dt.phuongXaNoiO.id in (:idCrit )" +
    " group by px.ten")
    List<BaoCaoF0TheoPhuongXa> baoCaoF0TheoPhuongXa(List<Long> noiDtTaiNhaIds, List<Long> idCrit);

Xem thêm tại: https://stackoverflow.com/a/36329166
