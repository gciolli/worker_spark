worker_spark
============

A background worker for PostgreSQL 9.3 which executes a procedure at
regular intervals.


Quickstart
----------

First, create a procedure that produces some visible effects:

	CREATE FUNCTION public.my_spark()
	RETURNS void
	LANGUAGE plpgsql
	AS $$
	BEGIN
	    RAISE LOG 'Spark!';
	END;
	$$;

Then, add the following lines to postgresql.conf:

	shared_preload_libraries = 'worker_spark'
	worker_spark.database = 'my_db'
	worker_spark.schema = 'public'
	worker_spark.procedure = 'my_spark'
	worker_spark.naptime = 5

Finally, restart PostgreSQL and watch its logfile. You should see a
"Spark!" LOG message every 5 seconds.


Remarks
-------

You can produce more complex effects with a more complex procedure.

All the worker_spark.* settings require a restart to be changed.

If the procedure does not exist, no error message is raised.


Author
------

Gianni Ciolli <gianni.ciolli@2ndquadrant.it>


Licence
-------

The code is modelled on the worker_spi contrib module distributed with
PostgreSQL 9.3, and is released under the same licence.
