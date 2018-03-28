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
                    print_report_name="'Product Barcode Labels - %s' % (object.name)"/>
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
