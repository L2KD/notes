###### This will assign some users to a POS, prevent others from accessing/creating session (Testest on Odoo 11)

1. Create field `x_user_ids`

       model `pos.config`
       type `many2many`
       relation `res.users`

2. Views `pos.config.form.view`, then add field to view.

3. Rule (`Settings > Security > Record Rule`)

       obj: `pos.config`
       domain filter: [('x_user_ids', '=', user.id)]
       group: `pos/user`
