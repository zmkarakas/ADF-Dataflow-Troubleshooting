UserErrors encountered on Dataflows in ADF and Synapse pipelines can be categorized in different ways. Firstly, there are performance related problems that are not otherwise put into the SystemError category. An example is abnormally long-running flows, or hanging at execution. Another kind of very common issue is running into problems in a certain row or certain column due to different causes such as datatype mismatch, misconfigurations in mapping, or simply because a character somewhere in the setup is interfering with spark.


Besides these, most commonly run into errors usually specify the location of the error, and the type of application or connector associated with it. Snippets from error message might be like this {"Status Code" :"DF-File-InvalidSparkFolder", "Message": "Failure at source". Since there are many connectors(ODBC/JDBC) in Dataflows, the issues associated with them are also varied. Certain applications are more prone to erroring. These error messages are very helpful in localizing the problem and swiftly taking actions for mitigation.


From a permenancy point of view, User errors can be put into 2 types. 
- Permanent: like bad credentials, even on retries this job will not succeed. 
- Transient: User errors can also be transient, like SQL server under load and not able to run the query fully and fail, these kinds of runs can end successfully in subsequent retries. So in the second kind of intermittent errors which happen during a job is running can result ,for example, in duplicate data.

UserErrors are called as such because they stem from the user configurations, environment, or simply put, from their side. But even some driver related errors are included into UserErrors. It could be an issue with an outdated driver behind a DB2 database for example. In this case usually the error would be specifically for certain rows or columns, for no apparent reason, suggesting a deprecated driver. 

More generalized UserErrors usually have an error message or a code associated, such as 4503. The Code indicates problematic parallel execution of flows and resulting in throttling.


For more reference, see the official documentation, almost all of the errors included here are UserErrors:

https://learn.microsoft.com/en-us/azure/data-factory/data-flow-troubleshoot-guide

Please note that this doc does not cover all of the possible errors, as they run into thousands. Of course even a book can be written about these errors since there are so many, I just wanted to give a small bit of my experience here.
