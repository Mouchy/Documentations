Commandes Oracle
################

select * from dba_objects where object_name = 'PARMSCOF_REPRISE_404_P2'

substr 
translate

select frqanas from cdsepr;
select frqanas||'!', substr(frqanas, 1, 4)||'  ' || translate(substr(frqanas, 1, 4), ' YHQM', ' ZTRN') from cdsepr;