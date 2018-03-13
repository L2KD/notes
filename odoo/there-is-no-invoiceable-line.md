# This error sucks...

Trong file `sale/models/sale.py:405`

    for order in self:
        group_key = order.id if grouped else (order.partner_invoice_id.id, order.currency_id.id)
        for line in order.order_line.sorted(key=lambda l: l.qty_to_invoice < 0):
            if float_is_zero(line.qty_to_invoice, precision_digits=precision):
                continue
            [...]

            if group_key not in invoices:
                invoices[group_key] = invoice

    if not invoices:
        raise UserError(_('There is no invoicable line.'))

--> Nếu biến `invoices` rỗng thì sẽ dính lỗi này.

#### Vậy tại sao `invoices` bị rỗng?

Do trong dòng for line in đó không đi qua hết được, hay nói cách khác là `qty_to_invoice = 0` cho tất cả các line.

#### Vậy tại sao `qty_to_invoice = 0`?

Xem `sale/models/sale.py:730`

    def _get_to_invoice_qty(self):
        for line in self:
            if line.order_id.state in ['sale', 'done']:
                if line.product_id.invoice_policy == 'order':
                    line.qty_to_invoice = line.product_uom_qty - line.qty_invoiced
                else:
                    line.qty_to_invoice = line.qty_delivered - line.qty_invoiced
            else:
                line.qty_to_invoice = 0

Như vậy nếu order ở state không ở `sale` hoặc `done` thì `qty_to_invoice` lúc nào cũng = 0
