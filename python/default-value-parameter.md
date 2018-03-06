# Py 3

Instead of this

    def _parse_api_data(self, data_list, value_key, value_type=str):
        # Do something with value_type

This should be

    def _parse_api_data(self, data_list, value_key, value_type=None):
        if value_type is None:
            value_type = str

        # Do something with value_type

This reminds me a lot of JS 
