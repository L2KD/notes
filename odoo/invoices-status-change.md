# Odoo 11

1. Create Invoice from Sale order

    sale_order.action_invoice_create()

2. After creating, account_invoice will be in state 'draft'. Draft state account_invoice is able to be modified line_ids (qty...)

3. Change from draft to open
