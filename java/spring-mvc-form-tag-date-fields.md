Đối với các form tag input, label, checkbox, checkboxes, radiobuttons... thì mọi việc đơn giản. 

Tuy vậy đến trường hợp của tag input nhưng data type bên trong là LocalDateTime (J8) thì mọi chuyện phức tạp hơn. Do spring mvc không biết cách cast về kiểu này qua lại giữa form và controller (model map).

Mong muốn:

Ví dụ có entity sau:

```
@Getter
@Setter
@NoArgsConstructor
@Entity
@Table(name = "HSBA_BENHAN_YHCT_BANT", schema = "HIS_MANAGER",
        uniqueConstraints = {
        @UniqueConstraint(columnNames = {"STT_BENHAN", "DVTT", "STT_DOTDIEUTRI"}),
        @UniqueConstraint(columnNames = {"SO_LUU_TRU", "DVTT"}),
        @UniqueConstraint(columnNames = {"SO_BENH_AN", "DVTT"}),
    }
)
public class BenhAnYhct implements Serializable {
    @EmbeddedId
    private BenhAnYhctId id = new BenhAnYhctId();
    @Column(name = "HO_TEN")
    private String hoTen;

    @DateTimeFormat(pattern = DATETIME_FORMAT)
    private LocalDateTime thoiGianNhapVien;

}
```

Controller sau:

```
@PostMapping(value = "/hsba-bant-yhct-luu", consumes = MediaType.APPLICATION_FORM_URLENCODED_VALUE, produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
    public @ResponseBody BenhAnYhct hsbaBantLuuYhct(
            @ModelAttribute("benhAnYhct") BenhAnYhct form,
            Model model,
            HttpServletRequest request,
            HttpServletResponse response
    ) {

        // Xử lý lưu dữ liệu tại đây
    }
```        

View:

```

```