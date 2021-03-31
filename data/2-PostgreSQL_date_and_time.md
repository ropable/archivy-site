---
date: 03-31-21
id: 2
path: ''
tags:
- SQL
title: PostgreSQL date and time
type: note
---

Get the date and time time right now:

```sql
select now(); -- date and time
select current_date; -- date
select current_time; -- time
```

Find rows between two absolute timestamps:

```sql
select count(1)
from events
where time between '2018-01-01' and '2018-01-31';
```

Find rows created within the last week:

```sql
select count(1)
from events
where time > now() - interval '1 week'; -- or '1 week'::interval, as you like
```

Find rows created between one and two weeks ago:

```sql
select count(1)
from events
where time between (now() - '1 week'::interval) and (now() - '2 weeks'::interval);
```

Extracting part of a timestamp:

```sql
select date_part('minute', now()); -- or hour, day, month
```
Get the day of the week from a timestamp:

```sql
-- returns 0-6 (integer), where 0 is Sunday and 6 is Saturday
select date_part('dow', now());

-- returns a string like monday, tuesday, etc
select to_char(now(), 'day');
```

Converting a timestamp to a unix timestamp (integer seconds):

```sql
select date_part('epoch', now());
```

Calculate the difference between two timesetamps:

```sql
-- Difference in seconds
select date_part('epoch', delivered_at) - date_part('epoch', shipped_at); -- or minute, hour, week, day, etc

-- Alternatively, you can do this with `extract`
select extract(epoch from delivered_at) - extract(epoch from shipped_at);
```