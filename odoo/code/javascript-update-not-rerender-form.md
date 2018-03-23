Dev chức năng Barcode cho Odoo CE 11, đã bắt được barcode scanned, nhưng không reload lại được form.

Code JS:

    self.update({}, {reload: false});

Debug sâu vào AbstractController, hàm update(),

    var localState = self.renderer.getLocalState();

thấy bản EE giữ localState = 0 thay vì 1.

Check hàm `getLocalState()`

Xem lại [view declaration][1] của `stock`.

    <page string="Detailed Operations" attrs="{'invisible': [('show_operations', '=', False)]}">


[1]: https://github.com/odoo/odoo/blob/11.0/addons/stock/views/stock_picking_views.xml#L259
