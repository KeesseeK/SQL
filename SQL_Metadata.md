Query for PostgreSQL, which return name of tables used by specific materialized view, without using iformation_schema




SELECT DISTINCT
  pg_class.oid::regclass
FROM pg_rewrite
JOIN pg_depend ON
  pg_depend.classid = 'pg_rewrite'::regclass AND
  pg_depend.objid = pg_rewrite.oid AND
  pg_depend.refclassid = 'pg_class'::regclass
  
JOIN pg_class ON
  pg_class.oid = pg_depend.refobjid
  
WHERE
  pg_rewrite.ev_class = 'films_ext'::regclass AND
  pg_depend.refobjid <> pg_rewrite.ev_class AND
  pg_class.relkind IN ('r','f','p','v','m')