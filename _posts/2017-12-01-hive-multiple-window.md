---
layout: post
title: Multiple Window Clauses in Hive Queries
comments: true
category: technical
tags: [hive, apache hive, hadoop, sql, database]
---

Recently, I had to write a Hive query that contains multiple window functions. As the number of windows grew, the query became an unreadable mess.

It started like this ---

```sql
-- Query 1
SELECT AVG(a) OVER (PARTITION BY b, c ORDER BY d ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS s1,
       MAX(a) OVER (PARTITION BY b, C ORDER BY d ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS s2,
       AVG(a) OVER (PARTITION BY x, y ORDER BY z) AS t1,
       MAX(a) OVER (PARTITION BY x, y ORDER BY z) AS t2
  FROM table_name
```

The problem was that several windows repeated in the query, so I thought there must be a way to remove such redundancy. I found that Hive supports [window clause](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+WindowingAndAnalytics#LanguageManualWindowingAndAnalytics-WINDOWclause), but for several weeks I couldn't figure out how to define multiple window clauses in the same query. It turned out, like many programming headaches, that the comma was messing with me --- you only need to know where to put it.

```sql
-- Query 2
SELECT AVG(a) OVER w1 AS s1,
       MAX(a) OVER w1 AS s2,
       AVG(a) OVER w2 AS t1,
       MAX(a) OVER w2 AS t2
  FROM table_name
WINDOW w1 AS (PARTITION BY b, c ORDER BY d ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW),
       w2 AS (PARTITION BY x, y ORDER BY z)
```

**Is there any performance gain?** No, in my use case, Query 1 and 2 generated the same execution plan in Hive for me. I have not tested, however, on window functions in a real RDBMS. Please let me know if you know the answer!
