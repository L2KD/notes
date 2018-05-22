### Tạo 1 menu print trực tiếp & đơn giản

Trong module, tạo folder `report`

Tạo file `product_report.xml`.

    <odoo>
        <data>
            <report id="report_product_label"
                    string="Product Barcode Labels"
                    model="product.product"
                    report_type="qweb-html"
                    name="<module_name>.report_productlabel"
                    file="<module_name>.report_productlabel" <!-- this will look for that `report_productlabel` inside the module_name (1)-->
                    print_report_name="'Product Barcode Labels - %s' % (object.name)"
                    menu="True" <!-- True will bring this to the Print button, and vice versa --> />
        </data>
    </odoo>

File này sẽ tạo ra một option print nữa có text `Product Barcode Labels` vào các form của model `product.product`, có thể xem thêm tại Settings > Reports

Tạo file `product_product_template.xml`

    <odoo>
        <data>
            <template id="report_simple_label">
                <div class="col-xs-3"
                     style="padding:5px;text-align:center;border: 2px solid black;width: calc(100%/5);"
                     t-if="product.barcode">
                    <img t-att-src="'/report/barcode/?type=%s&amp;value=%s&amp;width=%s&amp;height=%s&amp;humanreadable=1' % ('EAN13', product.barcode, 600, 400)"
                         style="max-width:100%;"/>
                </div>
            </template>

            <!-- This is referenced by the above (1), then call (2) -->
            <template id="report_productlabel">
                <t t-call="web.basic_layout">
                    <div class="page">
                        <t t-foreach="docs" t-as="product">
                            <!-- (2), call this and the result is up there -->
                            <t t-call="<module_name>.report_simple_label">
                                <t t-set="product" t-value="product"/>
                            </t>
                        </t>
                    </div>
                </t>
            </template>
        </data>
    </odoo>

Add 2 files xml vào `__manifest__.py`

### Thay vì menu print trực tiếp, nhúng wizard vào (1 dạng act_window) để xác nhận hoặc thêm vài thông số trước khi in

Trong folder module, tạo python `models/print_qty.py`

    from odoo import models, fields, api, exceptions


    class ProductBarcodePrintQty(models.Model):
        _name = 'mekongoo_barcode.print_qty'

        barcode_print_qty = fields.Integer("Quantity to print", default=1, required=True)

        @api.multi
        def print_report(self):
            print_qty_ids = self.env[self._name].search([('id', '!=', self.id)])
            for print_qty_id in print_qty_ids:
                print_qty_id.unlink()

            res = self.read(['barcode_print_qty'])
            res = res and res[0] or {}
            res['barcode_print_qty'] = res['barcode_print_qty']

            if not res['barcode_print_qty'] or res['barcode_print_qty'] <= 0:
                raise exceptions.UserError('Please select a valid quantity')

            docids = self.env.context.get('active_ids', [])
            return self.env.ref('mekongoo_barcode.report_product_barcode_label').report_action([docids], config=False)

Trong file này khai báo 1 prop là barcode_print_qty kiểu Int, 1 method print_report. Methodo này đơn giản chỉ là nhận active_ids và truyền vào report chính.

tạo `wizard/product_barcode_labels_views.xml`

    <?xml version="1.0" encoding="UTF-8" ?>
    <odoo>
        <!--  Wizard for Number of Print Product Barcode Labels -->
        <record id="view_product_barcode_label" model="ir.ui.view">
            <field name="name">Quantity Labels to Print</field>
            <field name="model"><module_name>.print_qty</field>
            <field name="arch" type="xml">
                <form string="Quantity Labels to Print">
                    <label for="barcode_print_qty"/>
                    <field name="barcode_print_qty" required="1"/>
                    <footer>
                        <button name="print_report" string="Print" type="object" class="btn-primary"/>
                        <button string="Cancel" class="btn-default" special="cancel"/>
                    </footer>
                </form>
            </field>
        </record>

        <act_window id="action_product_label"
                    key2="client_print_multi"
                    name="Product Barcode Labels"
                    res_model="<module_name>.print_qty"
                    src_model="product.product"
                    view_mode="form" target="new" view_type="form"/>
    </odoo>

Trong đó tag `act_window` sẽ chịu trách nhiệm tạo 1 opt trong menu print của model `product.product`, khi click vào sẽ tạo 1 record mới của model ở res_model (`module_name.print_qty`), kiểu là form, target new nghĩa là ở dạng modal. Form này gồm những gì thì ở record id=view_product_barcode_label định nghĩa. Đơn giản chỉ là 1 cái field đã khai báo trong file python phía trên, và 1 nút trỏ về method print_report. Key2 (client_print_multi) cho biết cái act_window này nó nằm trong chỗ client, menu print, khi multi select model.

Trong method print đó, dòng return sẽ gọi `mekongoo_barcode.report_product_barcode_label` --> trong file report/product_report.xml đã khai báo. Chỗ này cần set menu=False để nó k hiện ra 1 lần nữa trong menu print.

Sau khi gọi mekongoo_barcode.report_product_barcode_label, template phù hợp sẽ được gọi (template khai báo trong report/product_product_template.xml)
