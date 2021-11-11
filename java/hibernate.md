# Hibernate

Sử dụng enum trong hibernate

Ví dụ có enum sau:

    public enum Tuyen {
        TUYEN1,
        TUYEN2,
        TUYEN3,
        TUYEN4
    }

Và class

    @Entity
    public class DonVi {
        private Tuyen tuyen;
        ...
    }

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

Class DonVi:

    @Entity
    public class DonVi {
        @Enumerated(EnumType.ORDINAL)
        @Convert(converter = TuyenAttributeConverter.class)
        private Tuyen tuyen;
    }
    
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
