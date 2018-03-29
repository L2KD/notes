.do_action({
                            type: 'ir.actions.act_window',
                            res_model: model,
                            res_id: id,
                            view_mode: 'form',
                            views: [[false, 'form']],
                            target: 'current',
                            context: {'form_view_initial_mode': 'edit','force_detailed_view': true}, // This gets the job done
                        });
