## System Errors encountered in ADF-Mapping Dataflow

SystemErrors are a broad category, in the dataflow setting, under this category first and most important are called internal server errors. These errors have codes such as 135,136,136,138. Internal server errors have different contexts according to which application is generating the error. When these errors are intermittent, in that they resolve on their own after some time, or after a retry right after failure, these errors are outage related. Outages are rare events in cloud computing, they stem from the cloud providers side rather than user side, hence the right classification for a SysError. These sometimes stem from backend changes, in which case rollbacks are useful for mitigation, in other cases, load spikes on infrastructure resources, especially when provisioning gets bottleneck (since clouds rely on datacenters for processing power etc.), or live-site incidents may be to blame. It is also good to keep in mind that cloud applications are constantly evolving and being worked on by engineers, so sometimes changes done to improve the application might lead to unexpected results for users.

Error text messages usually start with "DF-" meaning dataflow, and next categorized by the related application such as Dynamics or CosmosDB and finally on third, the exact error classification. So one example would be "DF-Executor-OutOfMemorySparkBroadcastError" or "DF-Executor-InternalServerError". The latter being of course, probably the most common issue in dataflows, and take up the bulk of support cases. All errors are listed in public documentation of dataflow troubleshooting guide.

In Synapse dataflows, these kind of errors are generally the same across the board as ADF. The differences are usually dependent on the clusters.

Here is an example of this error:

![internal2](https://user-images.githubusercontent.com/50174304/214174334-ed923dd8-a7d0-4c58-9ebf-3c394e438fcc.png)

This error only resolved after 4 hours. Incidents take all the way from seconds to hours to get auto-mitigated

Setting aside exceptional scenarios, here is how to handle them:

Even though they are system errors, in Out Of Memory(OOM) cases, the error results from user side, in that the user did not set up enough compute to handle the load, in this case usually upscaling the core size and switching to memory optimized would work. They can also be categorized as performance errors, thus troubleshooting the performance is also useful. For example, if source is SQL, then SQL partioning is important. For example if using Azure SQL, select source partioning under Optimize tab. In file based scenarios, rather than handling many small files, consider putting data into fewer but larger files. Also make adjustments for partioning under Optimize tab of the dataflow, such as Hash or Round Robin. For OOM related internal errors, retrying will not help, and is a permenant error, unlike the outage related ones. It is good to keep in mind that expensive transformations exponentially affect the memory and compute that is required. These transformations such as pivot, unpivot, window, joins, aggregations might run eventually into OOM related errors even though you think you have enough compute or memory on your side. Increasing big data sizes combined with shuffles in spark is a way to stress the memory.  One of the features of dataflows is the ability to read in parallel from many different sources, however, too many parallel running flows might stress the memory as well . 

It is sometimes hard to distinguish these internal server errors especially when they are permenant, in that case, create a support ticket with ADF or Synapse (whichever dataflow you are using). 



