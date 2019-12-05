# Chúng ta lướt web ở độ cao 10 000 mét như thế nào?

Lướt internet ở độ cao 10 km thực tế là điều mà chúng ta mong đợi khi bay. Theo nghiên cứu Du khách Toàn cầu 2018 (Nguyên văn: [2018 Global Traveler](https://www.gogoair.com/learning-center/2018-global-traveler-research-study/)), 94% du khách trên quả địa cầu này cảm thấy rằng trên chuyến bay của họ có dịch vụ wifi sẽ tăng trải nghiệm bay của họ và 30% trong số đó rõ ràng là quan tâm đến dịch vụ này khi đặt vé máy bay.

![10 quan tâm khi đặt vé máy bay](https://miro.medium.com/max/1306/0*OiNjMo2GsbQWQEoq)

> Nguồn: 2018 Global Traveler 

Hiện tại, các hãng hàng không thu $17 mỗi hành khách chi phí các dịch vụ như thức ăn hay bán lẻ trên chuyến bay. Nếu có cả wifi trên chuyến bay, hành khách sẽ phải trả thêm $4 và điều này sẽ mang về thêm khoảng 30 tỉ đô vào doanh thu cho các hãng hàng không vào năm 2035.

Vì thế, sẽ không bất ngờ khi các hãng hàng không trên thế giới đang chạy đua nhau để trang bị wifi trên các chuyến bay của họ. Nhưng làm thế nào một hãng hàng không có thể đáp ứng được cái thứ xa xỉ hiện đại đó cho khách hàng khi họ đang di chuyển gần 1000 km/h và ở độ cao xấp xỉ 10 000 mét trên mặt nước biển?

Có hai cách để chúng ta đem internet về với các phi cơ.

## Hệ thống Air to ground (ATG)

Hệ thống này, vào thủa ban sơ lúc nó được phát triển, hoạt động như thể mạng di động mà chúng ta đang sử dụng như trên điện thoại. Nhưng không giống các trạm phát sóng di động chủ yếu phát tín hiệu xuống, các tháp này cung cấp internet cho các phi cơ bằng cách [phát tín hiệu lên phía trên](https://fortune.com/2017/08/10/how-does-wifi-work-on-planes/). Các antenne được bố trí ngay bụng máy bay sẽ thu tín hiệu này và truyền đến máy chủ cũng được đặt trên máy bay. Server này có modem để có thể chuyển đổi tín hiệu ở tần số radio thành tín hiệu máy tính và ngược lại, cung cấp internet cho khách bay thông qua các access points đặt bên trong phi cơ. Thông tin được trao đổi giữa antenne của máy bay và các tháp thu phát sóng trong suốt hành trình bay. Tiếp đên, các tháp phát sóng được kết nối vào các trung tâm vận hành được quản bởi các nhà cung cấp dịch vụ y như các ISP di động.

![Ground-based towers projecting signal upward.](https://miro.medium.com/max/530/0*z8wD2ixUwoZwqZdV)

> Các tháp phát sóng chiếu tín hiệu lên phía trên. Nguồn: [ThePointsGuy](https://thepointsguy.com/2015/11/how-in-flight-wi-fi-works/).

![ATG-4 của hãng cung cấp GoGo bao gồm 2 antenne chính ngay dưới bụng máy bay, 2 antenne bên hông, một server và một dàn các router wifi bên trong máy bay.](https://miro.medium.com/max/1000/0*r-18bsHXs3IJhpae)

> ATG-4 của hãng cung cấp GoGo bao gồm 2 antenne chính ngay dưới bụng máy bay, 2 antenne bên hông, một server và một dàn các router wifi bên trong máy bay.

Hình trên mô tả hệ thống ATG-4 của GoGo, hiện được sử dụng ở các hãng hàng không của Hoa Kỳ. Tầm phủ sóng của GoGo kéo dài xuyên suốt Bắc Mỹ với hệ thống tầm 200 tháp.

Hệ thống ATG có 2 khuyết điểm lớn như sau:

1. Do hoạt động trên băng tần thấp (800MHz), nên chỉ có thể cung cấp được tốc độ tối đa cho mỗi chuyến bay vào khoảng 10 Mbps. Để so sánh thì [tốc độ internet bình quân](https://www.speedtest.net/reports/united-states/2018/#fixed) của Hoa Kỳ gần 100 Mbps. Khi có nhiều người dùng cùng lúc, tốc độ cho mỗi người dùng chỉ đủ để check mail, và thậm chí là để check mail thì có khi cũng phải chờ trong vô vọng.

2. Tầm phủ sóng khá lẻ tẻ, vì sẽ có ít tháp phát sóng hơn như trong hoang mạc hoặc là không có tháp nào như ở ngoài đại dương mênh mông. Việc này khiến cho hệ thống ATG không phải là sự lựa chọn phổ biến trong các chuyến bay quốc tế.

## Hệ thống vệ tinh

Hệ thống vệ tinh phức tạp hơn ATG, nhưng đem lại tốc độ nhanh hơn và đáng tin cậy hơn.

Thay vì nằm dưới bụng, các antenne được trang bị trên đỉnh máy bay. Các antennes này thu tín hiệu từ các vệ tinh bay quanh trái đất. Nhưng vì cả đôi chúng nó (cái phi cơ của chúng ta và cái vệ tinh)