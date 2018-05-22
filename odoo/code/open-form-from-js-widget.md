Just simple as:

    this.do_action({
        type: 'ir.actions.act_window',
        res_model: 'sale.order',
        res_id: id,
        view_mode: 'form',
        views: [[false, 'form']],
        target: 'current'
    });
