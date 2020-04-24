---
layout: post
title:  'One month of Google Summer of Code with FOSSi'
date:   '2017-07-03 00:07:31+05:30'
categories: stories
tags: ['librecores', 'google summer of code', 'symfony', 'php']
comments: true
redirect_to: 'https://medium.com/@agathver/one-month-of-google-summer-of-code-with-fossi-7925ff973695'
---

It has been a month since I have been working on my [Google Summer of Code][1] Project with [Free and Open Source Sillicon Foundation][2]. And in this month I have been working to [extract and display statistics about code of projects listed in LibreCores.org][3].

## 1st and the 2nd Week

In the first two weeks, I have extracted the commits and contributors of the repositories of the projects. Since, Librecores.org clones the project's repository in order to extract the README and license, extracting commit information was a matter of running a few additional git commands on the clone. However, the process was not so straightforward. Parsing and properly storing the commits was a challenge in itself.

The extraction of commits has been added as a step to the reopsitory update pipeline. Commits are extracted and stored in the database as per their commit ID and user identity. To detect history rewrites, we check the presence of last commit in the current tree. If it is absent, then the commit history present in our database is rewritten - we drop all the commit records from our database and recreate all commit entries, from the beginning.

## 3rd Week

In the third week I worked on displaying the commit and contributor metrics. Commit activity are being displayed as a small line-graph, using a very lightweight jQuery library called [peity][4]. It is not very interactive as the sole purpose of it is to make it a generic activity graph which will indicate activity in a project. In future, we plan to add integrations with various issue trackers, which would also contribute to the activity graph.

This week was the most challenging of all. Extracting time-series data from SQL database is not a easy task. Sorting the values, filling the missing data with zero values, complicated `GROUP BY` clauses etc. These things deserve their own standalone article.

Additionally, for each project, top five contributors and their [Gravataar][5] images are also displayed.

## 4th week

The work of the third week, extended well into the 4th week, as those challenges were difficult to tackle. The fourth was spent in integrating [CLOC][6], a tool for identifying languages and counting the lines of code and comments in the project's source code. This also required a innovative way to store data, as serialized arrays.

### Structured Query meets Unstructured Data

With the introduction of JSON columns in MySQL, with full support of queries against the JSON fields, it seems as an interesting way to store data such as per-language breakdown of lines of code analysis. Even though a full relational model is possible in this case, it is lot easier to update and fetch data this way.

However, [Doctrine][7], the ORM used in our project does not support this natively (support is coming soon), we have to drop into PHP Serialization for now.

This month's work has been full of interesting observations and experiences. I am eagerly looking forward for the next months GSoC work, which will largely involve, making full use of Github APIs to avoid a full repository clones which is mildly risky.

## Contributions

These are the pull requests opened by me in the last month:

1. [#150 Extract and display project activity metrics #150](https://github.com/librecores/librecores-web/pull/150)
2. [#152 Configure Travis CI to run functional tests](https://github.com/librecores/librecores-web/pull/152)
3. [#149 Add nginx gzip compression](https://github.com/librecores/librecores-web/pull/149)
4. [#147 Add phpunit testing and travis CI](https://github.com/librecores/librecores-web/pull/147)
5. [#145 Add support for private Vagrant config](https://github.com/librecores/librecores-web/pull/145)


[1]:https://summerofcode.withgoogle.com
[2]:https://fossi-foundation.org
[3]:https://summerofcode.withgoogle.com/projects/#5672435520110592
[4]:http://benpickles.github.io/peity/
[5]:https://gravataar.com
[6]:https://github.com/AlDanial/cloc
[7]:http://www.doctrine-project.org/
