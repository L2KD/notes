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

    public static final String DATE_FORMAT = "dd/MM/yyyy";
    public static final String DATETIME_FORMAT = "dd/MM/yyyy HH:mm";

    @EmbeddedId
    private BenhAnYhctId id = new BenhAnYhctId();
    @Column(name = "HO_TEN")
    private String hoTen;
    ...

    @DateTimeFormat(pattern = DATETIME_FORMAT)
    private LocalDateTime thoiGianNhapVien; // <-- Xem chỗ này 

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
        model.addAttribute("benhAnYhct", form);

        // Xử lý lưu dữ liệu tại đây
    }
```        

View:

```

<form:form action="hsba-bant-yhct-luu" method="post" modelAttribute="benhAnYhct">
  <form:hidden path="id.sttBenhAn"/>
  <form:hidden path="id.sttDotDieuTri"/>
  <form:input path="thoiGianRaVien" cssClass="classic" />
</form:form>
```

Ajax.js:

```
$.ajax({
      url: 'hsba-bant-yhct-luu',
      data: $('#benhAnYhct').serialize(),
      contentType: 'application/x-www-form-urlencoded',
      type: 'post'
    }).success(function (data) {

      });
```

Để view có để parse được field type `LocalDateTime` hoặc ngược lại, phải thêm method sau vào controller.

```
@InitBinder
public void initBinder(WebDataBinder binder) {
    SimpleDateFormat sdf = new SimpleDateFormat(BenhAnYhct.DATE_FORMAT);
    sdf.setLenient(true);
    binder.registerCustomEditor(Date.class, new CustomDateEditor(sdf, true));
    SimpleDateFormat dateTimeFormat = new SimpleDateFormat(BenhAnYhct.DATETIME_FORMAT);
    dateTimeFormat.setLenient(true);
    binder.registerCustomEditor(Date.class, "thoiGianKhoaNv", new CustomDateEditor(dateTimeFormat, true));
    binder.registerCustomEditor(Date.class, "thoiGianNhapVien", new CustomDateEditor(dateTimeFormat, true));
    binder.registerCustomEditor(Date.class, "thoiGianRaVien", new CustomDateEditor(dateTimeFormat, true));
}
```

Thế là xong. Có thể map thoải mái LocalDateTime trong entity lên view và ngược lại (view map về model cho controller).

Từ bước này có thể dùng jQuery UI datepicker để init cho input #thoiGianRaVien, format thì xem doc của jQ UI.
