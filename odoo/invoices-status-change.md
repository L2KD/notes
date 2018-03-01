# Odoo 11

1. Create Invoice from Sale order

        sale_order.action_invoice_create()

2. After creating, `account_invoice` will be in state `draft`. Draft state `account_invoice` is able to be modified `line_ids` (qty...)

3. Change from `draft` to `open`.

        account_invoice.action_invoice_open()

4. Change from `open` to `paid`. Need to look for a journal first (e.g. Bank).

        journal_id = self.env['account.journal'].search([('type','=', 'bank')], limit=1)
        account_invoice.pay_and_reconcile(journal_id, date=magento_updated_date)
