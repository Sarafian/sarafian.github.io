---
title: Before blaming relational databases, consider the data access layer.
excerpt: In a world of NOSQL databases, relational databases are often pointed out as the problem but when digging a bit deeper, there are other factors at fault like cultural and lack of knowledge.
---

Over the last two years, I've run into many people who have either argued against the dependency to a database or against RDBMS databases. The usual complaint is about performance and stability. Unfortunately, people without enough knowledge make uninformed conclusions blaming the databases or stating that a no-sql database would have been so much better. First, they somehow manage to dismiss the historical applications of relational databases during the last decades while they completely fail to understand the impact of a poorly implemented data access layer. A poor implementation will be poor regardless of the type of the database.

Relational databases have been the workhorse in the industry for decades. For example, they've been managing warehouse data with millions of records and they've done this very well given the limitations of the hardware. When not working with *big data*, arguing that relational databases are inadequate, is very strange. When challenged about the volume, many counter with the argument that they are working with *big data*, because they think they work with too much data and that probably means *big*. Although there is no clear definition on [wikipedia][1] of what constitutes data as *big*, I would argue that capturing a couple of hundred thousand records during a day, doesn't automatically mean *big data*. Before one jumps on the wagon of *big data*, people should consider first if their data is really *big* and its lifetime. Besides, according to Wikipedia, [big data][1] is mostly about the analysis phase. Relational databases are not without their limitations especially when operating at a global scale but this is not the case for most applications.

In my experience, when the applications are not behaving well, the usual culprit is a slow performing data access layer due to poor implementation. In the .NET world, this usually means that the DAL is developed with [Entity Framework][2]. EF had its problems in the first versions but even with those limitations, it was never the reason for so poorly performing DALs. Once the application was optimized for scale, then the EF pipeline was identified as the weak component. 

The problem with EF and any ORM in fact is that it gives the developer a false perception that he/she works with in-memory datasets like arrays and lists. First, .NET offered [Linq][3] to drastically simplify working with in-memory sets and then EF took this concept applied over tables in relational databases. Before ORMs, developers worked on raw SQL and had much more knowledge and insight into what happens at the database level. Nowadays, many developers don't because they work with an abstraction tool. EF has seen big adoption because it is arguably cheaper to code but also requires fewer SQL skills from the developers who often complained about SQL. As I mentioned, DAL is one of the most critical parts for the performance and stability of an application. Going to EF without understanding the implication will have its consequences. 

The most typical mistake is when an EF code fragment results in big chunks data being transferred from the database to the application's memory space for further processing when the same could have been done within the database domain. This is so easy to do, because of how .NET hides what happens when the `IQueryable<T>` interface is converted to `IEnumerable<T>`. It is an easy and honest mistake from the developer, especially a junior, who thinks that his code is still preparing an EF query but instead, a SQL query was rendered, executed and the data is copied to the application memory space. The transfer is very slow and can also endanger the stability of the process by putting stress into the memory.

This is an example

| Case | EF Code | SQL Server |
| ---- | ------- | ---------- |
| Slow | ![Slow EF Code](/assets/images/posts/2019-07-01-ef-not-optimized.png "Slow EF Code") | ![Slow on SQL Server](/assets/images/posts/2019-07-01-sql-server-profiler-not-optimized.png "Slow on SQL Server") |
| Fast | ![Fast EF Code](/assets/images/posts/2019-07-01-ef-optimized.png "Fast EF Code") | ![Fast on SQL Server](/assets/images/posts/2019-07-01-sql-server-profiler-optimized.png "Fast on SQL Server") |

When working with SQL Server, simple analysis on the profiler would have revealed the problem. An analysis of the generated SQL can also help to further optimize queries and indexes, so at the end of the day a relatively good understanding of SQL is still required.

Overall, my advice to people is to avoid, making arbitrary conclusions that are based on personal bias, frustration and lack of relevance. Just consider the history and all the technology around us before **no-sql** became even a word. Look into the data access layer first when an application is struggling. Analyze the queries and the technologies involved. Look into the processes to understand how decisions are made, the expectations and the possibilities.

Pictures captured from NDC conference [Why databases cry at night? - Michael Yarichuk][4]

[1]: https://en.wikipedia.org/wiki/Big_data
[2]: https://docs.microsoft.com/en-us/ef/
[3]: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/
[4]: https://www.youtube.com/watch?v=CUbZ5tNZi7M