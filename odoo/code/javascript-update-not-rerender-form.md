Dev chức năng Barcode cho Odoo CE 11, đã bắt được barcode scanned, nhưng không reload lại được form.

Code JS:

    self.update({}, {reload: false});

Debug sâu vào AbstractController, hàm update(),

    var localState = self.renderer.getLocalState();

thấy bản EE giữ localState = 0 thay vì 1.

Check hàm [`getLocalState()`][2]

    var state = {};
    this.$('div.o_notebook').each(function () {
        var $notebook = $(this);
        var name = $notebook.data('name');
        var index = -1;
        $notebook.find('li').each(function (i) {
            if ($(this).hasClass('active')) {
                index = i;
            }
        });
        state[name] = index;
    });

Trả về state là số 0 nếu đang ở notebook đầu, nhưng tại sao lại có localState = 1 khi scan xong barcode (Odoo CE), và localState = 0 (Odoo EE)????

Xem lại [view declaration][1] của `stock`.

    <page string="Detailed Operations" attrs="{'invisible': [('show_operations', '=', False)]}">

Wow, cái này bị invisible nếu show_operations False.

--> bật nó lên trong `stock.picking.type`

---

Tab Detailed Operations hiện các sản phẩm đã được đặt, và sẽ cập nhật lại (thông qua `self.update()`) khi scanned barcode.


[1]: https://github.com/odoo/odoo/blob/11.0/addons/stock/views/stock_picking_views.xml#L259

[2]: https://github.com/odoo/odoo/blob/97fe1f05074a82715560cc825359663d65a39e41/addons/web/static/src/js/views/form/form_renderer.js#L142
