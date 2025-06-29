---
layout: post
title:  "Brooms vs Vacuums: When to use an ORM"
date:   2025-06-29
categories: programming databases orm
---

One rather frustrating topic issue that I've encountered as a software engineer is the groupthink around using an [Object-Relational Mapper (ORM)](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping). Some people don't understand the tradeoffs of using an ORM, vs writing data access code by hand. As a result, they can't comprehend why someone would write their own queries or populate their own objects.

To summarize the tradeoffs:

- **Hand-written SQL and manual creation of objects**: This approach can get quite time-consuming and tedious. It also might require learning arcane database syntax, or significant rewrites if the database changes. In contrast, hand-written queries normally run faster than queries from an ORM, and the application will use less CPU and RAM.
- **ORM**: Often, it is much faster (developer time) to use an ORM, but getting high performance can require tedious optimization or learning many more details about the ORM than the actual database. Often, if the database changes, the ORM can adapt to a new database with minimal changes.

The best way to think through the tradeoffs is to ask yourself: Do you need a broom or a vacuum.

## When to use a broom vs a vacuum?

Thin about where you live: You probably vacuum once a week or so. You might even pay housekeepers, who vacuum your home for you. But, if you spill some dirt or food, will you use a vacuum to clean the mess every time? Let's think through the decision about which tool to use for a mess:

- **Weight**: A broom weighs much less than a vacuum. Do you want to lug the vacuum to the mess?
- **Cord management**: A broom doesn't require electricity? Do you want to take the time to plug the vacuum in? And yes, there are cordless vacuums. I used to own one. But this leads to the next point.
- **Reliability**: A vacuum is more likely to break in a way that a broom doesn't. My cordless vacuum broke twice on me.
- **Size of mess**: Did you spill a small amount of dirt, or food?

You might think, "Is it worth it to lug out the vacuum, plug it in, and hope that it's not broken?" Certainly, if it's a big mess, it's worth it. But for a small mess, it's not. You'll take out a broom and a dustpan, sweep it up in one or two motions, empty the dustpan into the trash, and be done. You could choose to buy a small hand vacuum, but once it breaks, you'll find that it takes more time to futz with the hand vacuum instead of just using a damn broom.

But what if your housekeepers chose to bring brooms instead of vacuums? If you're home while they broom your house, you might appreciate the quiet; but if you have carpets, or a large house, brooming instead of vacuuming might not be practical.

Don't assume that "brooms are for small messes, vacuums are professionals." I can think of some cases where a professional will choose a broom:

- The staff at a movie theater may use a lobby broom and dustpan to clean up popcorn between shows. TODO image
- An archeologist will use a soft-bristle brush to remove dust and dirt from an artifact. TODO image

## Deciding to use an ORM or writing queries by hand

TODO: Make graph of bell curve

Choosing to write your own queries by hand, or using an ORM, simply requires understanding your needs.

When I worked at [Syncplicity](https://www.syncplicity.com/), we decided to refactor the desktop client to use [SQLite](https://sqlite.org/) without an ORM. Initially, this decision was mildly controversial: When I joined the company, it used a very early version of [NHibernate](https://nhibernate.info/); but it performed poorly because it used NHibernate incorrectly.

I pointed out to my team members two important facts:

- We had so few tables in our schema, and so few queries, that learning to use NHibernate correctly would take more time then just writing some queries by hand.
- All newcomers to our team would need to invest time in learning how to use NHibernate
- NHibernate's binary was many times larger than our binary

The problem that Syncplicity had with NHibernate was that the desktop client treated its SQLite database as a large, in-memory data structure. This is not how databases, and thus ORMs, are designed to work! As a consequence, the Syncplicity Desktop client performed very poorly whenever it used the database. This was not NHibernate's fault, as their documentation explicitly stated not to use it to treat a database as a large, in-memory data structure. The problem was that, in order to use the database correctly, everyone had to first learn how to program with a database, and then learn how to use NHibernate correctly. The problem with NHibernate (and most ORMs in general) is that it takes more time to learn how to use the ORM *correctly* than it takes to write a handful of simple database queries. (And, remember that you still need to know how to program with a database in order to use an ORM. You can't just introduce an ORM to a product as a replacement for requiring that your engineers know how to use a database.)

The decision to remove NHibernate was correct: Later, newcomers to the team appreciated that we had a small amount of very simple database code that was easy to learn, and easy to maintain. As we went through acquisitions, we didn't need to "defend" NHibernate to each parent company's legal team. We also didn't need to worry about [NHibernate's security vulnerabilities](https://www.cvedetails.com/cve/CVE-2024-39677/). We never switched from SQLite to another database, but if we did, there was so little "database" code that rewriting it wouldn't have taken a lot of time.

It would be a different case if the Syncplicity Desktop client had a more complicated schema and/or made significantly more database queries. In that case, the learning curve, and the implicit cost of a 3rd party library, would have been "worth it."

After Syncplicity, I joined [OptiRTC](https://www.optirtc.com/). Our backend service probably has in excess of 100 tables, and there are probably at least 1000, if not more, queries. We also run a lot of ad-hoc queries using [LINQPad](https://www.linqpad.net/). In our case, we use [Entity Framework](https://github.com/dotnet/efcore). Writing queries "by hand", except in performance critical areas, is a "fools errand" for us due to our team size.

Entity Framework, with LINQPad, allows us to more easily write ad-hoc queries, and read the data, than we could using a traditional SQL console. Even if all application code used hand-written SQL, it would be "worth it" for us to create Entity Framework bindings merely to support LINQPad.

We do have some older code, written by junior programmers, that is sensitive to [cartesian explosion](https://en.wikipedia.org/wiki/Cartesian_explosion). This is a consequence of junior programmers not understanding the limits of ORMs in database programming.

In contrast to OptiRTC, a few years ago, one weekend I quickly made a proof-of-concept web application with a database. It had one or two tables, and a few queries. I believed that it would take me more time to set up Entity Framework than to just write queries by hand, so that's what I did. Nothing ever came out of the weekend proof-of-concept; but if it did turn into a larger project, I would have included Entity Framework.

## When using a database, do you need a broom or a vacuum?

I think the best way to make this decision is to break the kind of database use into three categories:

1. **Broom because of simplicity**: Your database has few tables and simple query needs. Skip the ORM because you'll never "break even" on the time it takes to set it up and learn how to use it correctly.
2. **Vacuum because of breadth**: Your database had many tables and queries. You will "break even" on the time it takes to pick an ORM, set it up, and learn how to use it correctly.
3. **Archeologist's paintbrush because of specialization**: Your application has "hot" queries, is performance sensitive, or otherwise needs every kind of performance boost it can get. In this case, it's "worth it" to invest the time, or manpower, in hand-writing optimized queries and code to populate objects
