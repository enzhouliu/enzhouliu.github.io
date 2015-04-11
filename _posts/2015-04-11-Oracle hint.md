---
published: false
---

## Two hints of Oracle:
	### /*+ PARALLEL (Thread) */
This hint used to let Oracle use multi-thread to do the operation. My understanding is oracle will raise number of Thread based on second paramenter and patition the table based on the first parameter, and then use multi-thread to access the partition. 
For more information, please refer 
[How Parallel Execution Work](http://docs.oracle.com/cd/E11882_01/server.112/e25523/parallel002.htm)


What I encounter this week is that our ETL job running in PROD environment is 10x slower than QA box. Since PROD has better machine with more CPU and memory, although more applications are running at same time, it still should not behave like that. But after we hand over our SQL to DBA team, they found we used a lot of /*+ PARALLEL(DEFAULT) */, and after analysis the execution, they found since there are 192 threading running parallel on PROD vs 12 in QA. Most of the time wasted on coordinator, the <b>degree of parallel</b> is too high.

	### /*+ APPEND*/
The solution is reduce the degree of parallel by using hint /*+ PARALLEL (4)*/, and normaly 12 degree mean high degree of parallel.



Another hint I want to memtion is /*+ APPEND*/. This hint will indicate Oracle to use <b>direct path insert</b>, which means insert the data into the files inside of oracle buffer cache. According to Oracle, this way is using paralell and faster than normal insert. But this hint has its limitations:  1. the insert must be bulk insert which is INSERT SELECT statment. 2. In one transaction, you can not query the table you insert using this hit, a commit/rollback is required.

