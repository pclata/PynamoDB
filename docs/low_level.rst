Low Level API
=============

PynamoDB was designed with high level features in mind, but includes a fully featured low level API.
Any operation can be performed with the low level API, and the higher level PynamoDB features were all
written on top of it.

Creating a connection
^^^^^^^^^^^^^^^^^^^^^

Creating a connection is simple::

    from pynamodb.connection import Connection

    conn = Connection()

You can specify a different DynamoDB url:

::

    conn = Connection(host='http://alternative-domain/')

By default, PynamoDB will connect to the us-east-1 region, but you can specify a different one.

::

    conn = Connection(region='us-west-1')


Modifying tables
^^^^^^^^^^^^^^^^

You can easily list tables::

    >>> conn.list_tables()
    {u'TableNames': [u'Thread']}

or delete a table::

    >>> conn.delete_table('Thread')

If you want to change the capacity of a table, that can be done as well::

    >>> conn.update_table('Thread', read_capacity_units=20, write_capacity_units=20)

You can create tables as well, although the syntax is verbose. You should really use the model API instead,
but here is a low level example to demonstrate the point:

::

    kwargs = {
        'write_capacity_units': 1,
        'read_capacity_units': 1
        'attribute_definitions': [
            {
                'attribute_type': 'S',
                'attribute_name': 'key1'
            },
            {
                'attribute_type': 'S',
                'attribute_name': 'key2'
            }
        ],
        'key_schema': [
            {
                'key_type': 'HASH',
                'attribute_name': 'key1'
            },
            {
                'key_type': 'RANGE',
                'attribute_name': 'key2'
            }
        ]
    }
    conn.create_table('tablename', **kwargs)


Modifying items
^^^^^^^^^^^^^^^

The low level API can perform item operationst too, such as getting an item::

    conn.get_item('table_name', 'hash_key', 'range_key')

You can put items as well, specifying the keys and any other attributes::

    conn.put_item('table_name', 'hash_key', 'range_key', attributes={'key': 'value'})

Deleting an item has similar syntax::

    conn.delete_item('table_name', 'hash_key', 'range_key')
