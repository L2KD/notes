# Odoo 11 - Programmatically order (invoice, shipment, credit note) processing, status changing

## sale_order

### Create order

[ ] *TODO this need to be filled*

### Confirm `sale_order`

    sale_order.action_confirm()

After confirming `sale_order`, there will automatically be a new delivery (called in Odoo `picking_ids`)

## account.invoice

1. Create Invoice from Sale order

        sale_order.action_invoice_create()

2. After creating, `account_invoice` will be in state `draft`. Draft state `account_invoice` is able to be modified `line_ids` (qty...)

3. Change from `draft` to `open`.

        account_invoice.action_invoice_open()

4. Change from `open` to `paid`. Need to look for a journal first (e.g. Bank).

        journal_id = self.env['account.journal'].search([('type','=', 'bank')], limit=1)
        account_invoice.pay_and_reconcile(journal_id, date=magento_updated_date)

## Credit notes

1. To create credit note, an account_invoice is required

        account_invoice.refund()

2. Til this, the credit note is in `draft`. We can change it to open by `action_invoice_open()` or to paid by `pay_and_reconcile(journal_id, date=magento_updated_date)` like the account.invoice
