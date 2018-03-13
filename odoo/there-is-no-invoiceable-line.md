# This error sucks...

Trong file `sale/models/sale.py`

    for order in self:
        group_key = order.id if grouped else (order.partner_invoice_id.id, order.currency_id.id)
        for line in order.order_line.sorted(key=lambda l: l.qty_to_invoice < 0):
            if float_is_zero(line.qty_to_invoice, precision_digits=precision):
                continue
            [...]

    if not invoices:
        raise UserError(_('There is no invoicable line.'))
