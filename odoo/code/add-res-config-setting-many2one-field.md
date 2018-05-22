Trong module, tạo `models/setting.py`

    class ResConfigSettings(models.TransientModel):
        _inherit = 'res.config.settings'

        mkgc_stock_export_warehouse_id = fields.Many2one('stock.location', string="Default Warehouse To Export Stock")

    @api.model
    def get_values(self):
        stock_export_wh_id = self.env['stock.location'].search([('id', '=', mkgc_stock_export_warehouse_id)], limit=1)
        if stock_export_wh_id:
            stock_export_wh_id = stock_export_wh_id.id
            res.update(mkgc_stock_export_warehouse_id=stock_export_wh_id)

        return res

    @api.multi
    def set_values(self):
        super(MekongooConnectorSettings, self).set_values()
        ICPSudo = self.env['ir.config_parameter'].sudo()
        ICPSudo.set_param("mekongoo_connector.mkgc_stock_export_warehouse_id", int(self.mkgc_stock_export_warehouse_id))


This should get the job done.
