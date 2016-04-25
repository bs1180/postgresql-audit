This is a fork of the canonical [PostgreSQL audit trigger example](https://wiki.postgresql.org/wiki/Audit_trigger), updated to use jsonb instead of hstore. It adds two functions which are name spaced in the audit schema, but might prove useful elsewhere:

## jsonb_diff 
`audit.jsonb_diff(val1::jsonb, val2::jsonb, columns_to_remove::text[])`
Returns difference between two columns. Optional parameter is to any keys to omit completely.
Based on this [stackoverflow answer](http://stackoverflow.com/questions/36041784/postgresql-compare-two-jsonb-objects)

## jsonb_omit
`audit.jsonb_omit(val::jsonb, columns_to_remove::text[])`
Removes keys from jsonb object.
Similar to the delete operator (-), but works with an array of key names
rather than just one.

Note that both of these only work based on top level key names, not nested data.

---

Once installed, the trigger must be added to the table. In order to make it more performant, use the WHEN clause in addition to ignoring columns.

`SELECT audit.audit_table('users', 't'::boolean, t::boolean, '{"last_login_date"}')`

