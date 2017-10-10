---
title: How to browse Safari History with Dates on MacOS 10.13
layout: post
date: 2017-10-10
---

We want to see our history sorted by date. Unfortunatly, Apple only provides exact times on accounts with Parental Controls enabled.

## Via Terminal 

Using the [fish](https://fishshell.com) shell.

```shell
sqlite3 ~/Library/Safari/History.db "select visit_time, title,
     datetime(visit_time + 978307200, 'unixepoch', 'localtime')
     as human_readable_time
     from history_visits
     order by visit_time desc;"
```


## Via GUI

Install [DB Browser for SQLite](http://sqlitebrowser.org), hopefully by [homebrew](https://homebrew.sh).

Execute the following query: 

```sql
SELECT 
    visit_time, title,
    datetime(visit_time + 978307200, 'unixepoch', 'localtime')
        AS human_readable_time
FROM 
    history_visits
ORDER BY 
    visit_time DESC;
```


