# Odoo 11 - Create stock.picking Programmatically

confirm 1 sale order thì stock picking (delivery order) sẽ được tự tạo ra.

    sale_order.action_confirm()

Nếu không tạo ra, có thể sản phẩm trong line

- không phải là dạng stockable
- cursor chưa được commit mặc dù trước đó đã có tạo sản phẩm (trong trường hợp sale_order được confirm ở 1 thread riêng biệt)
